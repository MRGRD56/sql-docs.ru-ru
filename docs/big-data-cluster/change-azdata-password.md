---
title: Обновление AZDATA_PASSWORD
description: Обновление `AZDATA_PASSWORD` вручную
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 03/01/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 062e574772c2a44b78772da4a979c81ed3deb959
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836301"
---
# <a name="manually-update-azdata_password"></a>Ручное обновление `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Параметр `AZDATA_PASSWORD` задается во время развертывания, независимо от того, работает ли [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] с интеграцией Active Directory. Он обеспечивает базовую проверку подлинности для контроллера кластера и главного экземпляра. В этом документе описано, как вручную обновить `AZDATA_PASSWORD`.

## <a name="change-azdata_password-for-controller"></a>Изменение `AZDATA_PASSWORD` для контроллера

Если кластер работает в режиме без Active Directory, обновите пароль шлюза Apache Knox, выполнив следующие действия.

1. Получите учетные данные контроллера [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], выполнив следующие команды:

   а. Выполните эту команду от имени администратора Kubernetes:

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Декодируйте секрет из кодировки Base64.
   
   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. В отдельном командном окне предоставьте порт сервера базы данных контроллера:

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```
 
1. Используйте только что полученный пароль системного администратора для подключения к серверу базы данных контроллера из клиентского средства SQL.

1. Создайте новый сложный пароль для `AZDATA_USERNAME`, чтобы заменить существующий `AZDATA_PASSWORD`.

   Для упрощения примера следующие шаги используют "newPassword", потому что созданный пароль — это "newPassword". 

1. Получите `hexsalt` из пользовательской таблицы:

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` возвращает случайную шестнадцатеричную строку (например, `64FC59DF31244FFEE02F457BC0750226`).

1. Зашифруйте новый сложный пароль с помощью `hexsalt`.

   Для вашего удобства мы предоставляем готовый инструмент `pbkdf2` для шифрования пароля. Скачайте подходящее для платформы приложение .NET Core для [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries).

   Приложение является самодостаточным и не требует никаких предварительных условий, таких как среда выполнения .NET. Чтобы зашифровать пароль выполните команду:

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. Обновите пароль в пользовательской таблице:

   ```sql
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Изменение `AZDATA_PASSWORD` в главном экземпляре SQL Server

1. Подключитесь к главной конечной точке SQL с помощью любого пользователя-администратора.

1. Чтобы изменить пароль для учетных данных, которые вы определили во время развертывания в параметре `AZDATA_USERNAME`, выполните следующую команду TSQL:

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```

## <a name="manually-updating-password-for-grafana-and-kibana"></a>Обновление пароля вручную для Grafana и Kibana

После выполнения действий по обновлению AZDATA_PASSWORD вы увидите, что [Grafana](app-monitor.md) и [Kibana](cluster-logging-kibana.md) по-прежнему принимают старый пароль. Это обусловлено тем, новый секрет Kubernetes для них недоступен. Необходимо вручную обновить пароль поочередно для Grafana и Kibana.

## <a name="update-grafana-password"></a>Обновление пароля Grafana

Выполните следующие действия, чтобы вручную обновить пароль для [Grafana](app-monitor.md).

1. Вам понадобится служебная программа htpasswd. Ее можно установить на любой клиентский компьютер.
    
    #### <a name="for-ubuntu"></a>[Для Ubuntu:](#tab/ubuntu) 
    ```bash
    sudo apt install apache2-utils
    ```
    
    #### <a name="for-rhel"></a>[Для RHEL:](#tab/rhel) 
    ```bash
    sudo yum install httpd-tools
    ```
    
    ---

2. Создайте новый пароль. 
    
    ```bash
    htpasswd -nbs <username> <password>
    admin:{SHA}<secret>
    ```
    
    Замените значения для /<username/>, /<password/>, /<secret/> соответствующим образом, например:
    
    ```bash
    htpasswd -nbs admin Test@12345
    admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=
    ```

3. Теперь выполните кодирование пароля:
    
    ```bash
    echo "admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=" | base64
    ```             
    
    Сохраните выходную строку Base64 для последующего использования.
    
4. Затем измените параметр mgmtproxy-secret:
    
    ```bash
    kubectl edit secret -n mssql-cluster mgmtproxy-secret
    ```
         
5. Обновите значение controller-login-htpasswd созданной ранее строкой Base64 нового кодированного пароля:
    
    ```console
    # Please edit the object below. Lines beginning with a '#' will be ignored,
    # and an empty file will abort the edit. If an error occurs while saving this file will be
    # reopened with the relevant failures.
    #
    apiVersion: v1
    data:
       controller-login-htpasswd: <base64 string from before>
       mssql-internal-controller-password: <password>
       mssql-internal-controller-username: <username>
    ```         

6. Найдите и удалите объект pod mgmtproxy. 
     
    При необходимости определите имя объекта pod mgmtproxy.
    
    #### <a name="for-windows"></a>[Для Windows](#tab/windows): 
    На сервере Windows можно выполнить такую команду:
    
    ```bash 
    kubectl get pods -n <namespace> -l app=mgmtproxy
    ```
    
    #### <a name="for-linux"></a>[Для Linux](#tab/linux). 
    В Linux используйте следующую команду:
    
    ```bash
    kubectl get pods -n <namespace> | grep 'mgmtproxy'
    ```
    
    ---
    
    Удалите объект pod mgmtproxy:
    ```bash
    kubectl delete pod mgmtproxy-xxxxx -n mssql-clutser
    ```

7. Дождитесь подключения объекта pod mgmtproxy к сети и запуска панели мониторинга Grafana.  
 
    На это может потребоваться несколько секунд. Чтобы проверить состояние объекта pod, можно использовать ту же команду `get pods`, что и на предыдущем шаге. 
    Если вы видите, что объект pod mgmtproxy не возвращается к состоянию готовности, используйте команду kubectl для его описания:
    
    ```bash
    kubectl describe pods mgmtproxy-xxxxx  -n <namespace>
    ```
    
    Для устранения неполадок и дальнейшего сбора журналов используйте команду Azure Data CLI `[azdata bdc debug copy-logs](../azdata/reference/reference-azdata-bdc-debug.md)`.
    
8. Теперь войдите в Grafana с помощью нового пароля. 


## <a name="update-the-kibana-password"></a>Обновление пароля Kibana

Выполните следующие действия, чтобы вручную обновить пароль для [Kibana](cluster-logging-kibana.md).

> [!NOTE]
> Браузер Microsoft Edge старой версии несовместим с Kibana. Чтобы эта панель мониторинга отображалась корректно, необходимо использовать браузер Edge на базе Chromium. При загрузке панелей мониторинга в неподдерживаемом браузере вы увидите пустую страницу (см. [поддерживаемые Kibana браузеры](https://www.elastic.co/support/matrix#matrix_browsers)).

1. Откройте URL-адрес Kibana.
    
    URL-адрес конечной точки службы Kibana можно найти в [Azure Data Studio](manage-with-controller-dashboard#controller-dashboard) или с помощью следующей команды **azdata**:
    
    ```azurecli
    azdata login
    azdata bdc endpoint list -e logsui -o table
    ```
    
    Например: https://11.111.111.111:30777/kibana/app/kibana#/discover

2. На панели слева выберите параметр **Security** (Безопасность).
    
    ![Снимок экрана: меню в Kibana в области слева с выбранным параметром Security (Безопасность).](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-1.jpg)

3. На открывшейся странице в разделе Authentication Backends (Серверная часть для аутентификации) выберите **Internal User Database** (Внутренняя база данных пользователей).

    ![Снимок экрана: страница безопасности с выбранным параметром Internal User Database (Внутренняя база данных пользователей).](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-2.jpg)

4. Под заголовком Internal Users Database (Внутренняя база данных пользователей) вы найдете список пользователей. На этой странице можно добавлять и удалять пользователей для доступа Kibana к конечной точке, а также изменять их данные. Чтобы обновить пароль для пользователя, нажмите кнопку **Edit** (Изменить) справа от его имени.

    ![Снимок экрана: страница Internal Users Database (Внутренняя база данных пользователей) с выбранной кнопкой Edit (Изменить) для пользователя KubeAdmin в списке.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-3.jpg)

5. Дважды введите новый пароль и нажмите **Submit** (Отправить):

    ![Снимок экрана: форма Internal User (Внутренний пользователь) для изменения данных пользователя. Пароль нужно ввести в полях Password (Пароль) и Repeat password (Повтор пароля).](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-4.jpg)

6. Закройте браузер и повторно подключитесь к URL-адресу Kibana, используя новый пароль.

> [!Note]
> Если после входа с новым паролем в Kibana отображаются пустые страницы, выполните выход вручную (с помощью элемента в правом верхнем углу) и повторите вход.

## <a name="see-also"></a>См. также раздел

* [azdata bdc (Azure Data CLI)](../../sql/azdata/reference/reference-azdata-bdc.md) 
* [Мониторинг приложений с помощью azdata и панели мониторинга Grafana](app-monitor.md)  
* [Извлечение журналов кластера с помощью панели мониторинга Kibana](cluster-logging-kibana.md)  
---
title: Настройка проверки подлинности Active Directory в SQL Server на основе контейнеров Linux с помощью adutil
description: Пошаговые инструкции по настройке проверки подлинности Active Directory в SQL Server на основе контейнеров Linux с помощью adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: c82362cf9a3d73d1bd9a7c714957e222ad15af18
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837635"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux--containers"></a>Руководство по Настройка проверки подлинности Active Directory в SQL Server на основе контейнеров Linux

> [!NOTE]
> В настоящее время программа **adutil** предоставляется в режиме **общедоступной предварительной версии**.

В этом учебнике описывается, как настроить в SQL Server на основе контейнеров Linux поддержку проверки подлинности Active Directory, также известную как встроенная проверка подлинности. Общие сведения см. в статье [Проверка подлинности Active Directory для SQL Server на Linux](sql-server-linux-active-directory-auth-overview.md).

В этом руководстве рассматриваются следующие задачи:

> [!div class="checklist"]
> - Установка adutil-preview
> - Присоединение узла Linux к домену Active Directory
> - Создание пользователя Active Directory для SQL Server и настройка свойства ServicePrincipalName (имени субъекта-службы) с помощью программы adutil
> - Создание файла keytab службы SQL Server
> - Создание файлов mssql.conf и krb5.conf, которые будут использоваться контейнером SQL Server
> - Подключение файлов конфигурации и развертывание контейнера SQL Server
> - Создание имен входа SQL Server на основе Active Directory с помощью Transact-SQL
> - Подключение к SQL Server с помощью проверки подлинности Active Directory

## <a name="prerequisites"></a>Предварительные условия

Перед настройкой проверки подлинности Active Directory необходимо выполнить указанные ниже условия.

- В сети должен быть контроллер домена Active Directory (Windows).
- На хост-компьютере Linux, присоединенном к домену, должна быть установлена программа adutil-preview. Чтобы установить программу adutil-preview, следуйте указаниям в разделе [Установка adutil-preview](#install-adutil-preview) для используемого дистрибутива Linux.

## <a name="container-deployment-and-preparation"></a>Развертывание и подготовка контейнера

При настройке контейнера необходимо заранее знать порт, который будет использоваться контейнером на узле. Порт по умолчанию 1433 может быть сопоставлен по-иному на вашем узле контейнера. В этом учебнике порт 5433 на узле сопоставляется с портом 1433 контейнера. Дополнительные сведения см. в кратком руководстве [Запуск образов контейнеров SQL Server в Docker](quickstart-install-connect-docker.md).

При регистрации имен субъектов-служб можно использовать имя узла компьютера или имя контейнера, но необходимо настроить его так, как оно должно выглядеть при подключении к контейнеру извне.

В Active Directory должна быть добавлена запись узла переадресации (A) для IP-адреса узла Linux, сопоставленного с именем контейнера SQL Server. В этом учебнике IP-адрес хост-компьютера `myubuntu` — `10.0.0.10`, а имя контейнера SQL Server — `sql1`. Мы добавим указанную ниже запись узла переадресации в Active Directory. Она гарантирует, что при подключении пользователей к sql1.contoso.com они будут обращаться к нужному узлу.

:::image type="content" source="media/sql-server-linux-containers-ad-auth-adutil-tutorial/host-a-record.png" alt-text="добавление записи узла":::

В этом учебнике используется среда Azure с тремя виртуальными машинами. Первая виртуальная машина выступает в роли контроллера домена Windows с доменным именем `contoso.com`. Контроллер домена имеет имя `adVM.contoso.com`. Вторая машина — это компьютер Windows с именем `winbox`. На нем установлена ОС Windows 10 Desktop, и он используется в качестве клиента со средой SQL Server Management Studio (SSMS). Третья машина — это компьютер с Ubuntu 18.04 LTS. Он имеет имя `myubuntu`, и в нем размещаются контейнеры SQL Server. Все машины присоединены к домену `contoso.com`. Дополнительные сведения см. в статье [Присоединение SQL Server на узле Linux к домену Active Directory](sql-server-linux-active-directory-join-domain.md).

> [!NOTE]
> Присоединять компьютер, в котором размещаются контейнеры, к домену необязательно, как станет понятно далее в этой статье.

## <a name="install-adutil-preview"></a>Установка adutil-preview

Чтобы установить программу adutil-preview, на хост-компьютере Linux выполните приведенные ниже команды для используемого дистрибутива Linux.

> [!NOTE]
> Учтите, что в некоторых дистрибутивах Linux при попытке установить эту предварительную версию adutil без параметра `ACCEPT_EULA` процесс установки будет затруднен. Мы рекомендуем устанавливать программу adutil-preview с заданным параметром `ACCEPT_EULA=Y`. Перед установкой можно ознакомиться с [лицензионным соглашением](https://go.microsoft.com/fwlink/?linkid=2151376) для предварительной версии. Мы активно работаем над этой проблемой, и она должна быть исправлена в общедоступной версии.

### <a name="rhel"></a>RHEL

1. Скачайте файл конфигурации репозитория Microsoft Red Hat.

    ```bash
    sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
    ```

1. Если у вас установлена предыдущая версия adutil, удалите все старые пакеты adutil.

    ```bash
    sudo yum remove adutil
    ```

1. Выполните приведенные ниже команды для установки adutil-preview. Используйте параметр `ACCEPT_EULA=Y`, чтобы принять лицензионное соглашение для предварительной версии adutil. Лицензионное соглашение находится по пути `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y yum install -y adutil-preview
    ```

### <a name="ubuntu"></a>Ubuntu

1. Импортируйте ключи GPG в общедоступном репозитории, а затем зарегистрируйте репозиторий Microsoft Ubuntu.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
    ```

1. Если у вас установлена предыдущая версия adutil, удалите все старые пакеты adutil с помощью приведенных ниже команд.

    ```bash
    sudo apt-get remove adutil
    ```

1. Выполните приведенную ниже команду для установки adutil-preview. Используйте параметр `ACCEPT_EULA=Y`, чтобы принять лицензионное соглашение для предварительной версии adutil. Лицензионное соглашение находится по пути `/usr/share/adutil/`.

    ```bash
    sudo apt-get update
    sudo ACCEPT_EULA=Y apt-get install -y adutil-preview
    ```

### <a name="sles"></a>SLES

1. Добавьте репозиторий Microsoft SQL Server в Zypper.

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo
    ```

1. Если у вас установлена предыдущая версия adutil, удалите все старые пакеты adutil.

    ```bash
    sudo zypper remove adutil
    ```

1. Выполните приведенную ниже команду для установки adutil-preview. Используйте параметр `ACCEPT_EULA=Y`, чтобы принять лицензионное соглашение для предварительной версии adutil. Лицензионное соглашение находится по пути `/usr/share/adutil/`.

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="creating-the-ad-user-spns-and-sql-server-service-keytab"></a>Создание пользователя Active Directory, имен субъектов-служб и файла keytab службы SQL Server

Если вы не хотите, чтобы узел контейнера SQL Server на Linux был частью домена, и не выполнили инструкции по присоединению компьютера к домену, то на другом компьютере Linux, который уже входит в домен Active Directory, выполните указанные ниже действия.

 1. Создайте пользователя Active Directory для SQL Server и задайте имя субъекта-службы с помощью программы adutil.

 2. Создайте и настройте файл keytab службы SQL Server.

Скопируйте созданный файл mssql.keytab на хост-компьютер, на котором будет выполняться контейнер SQL Server, и настройте контейнер так, чтобы он использовал скопированный файл mssql.keytab. При необходимости можно также присоединить узел Linux, на котором будет выполняться контейнер SQL Server, к домену Active Directory и выполнить приведенные ниже действия на том же компьютере.

### <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-using-the-adutil-tool"></a>Создание пользователя Active Directory для SQL Server и настройка свойства ServicePrincipalName с помощью программы adutil

Чтобы включить проверку подлинности Active Directory для контейнеров SQL Server на Linux, необходимо выполнить описанные ниже шаги 1–3 на компьютере Linux, входящем в домен Active Directory.

1. Получите или продлите билет предоставления билета (TGT) Kerberos с помощью команды `kinit`. Для выполнения команды `kinit` используйте привилегированную учетную запись. Учетная запись должна иметь разрешение на подключение к домену, а также возможность создавать учетные записи и имена субъектов-служб в домене.

    > [!IMPORTANT]
    > Перед выполнением этой команды узел уже должен быть включен в домен согласно предыдущим инструкциям.

    ```bash
    kinit privilegeduser@DOMAIN.COM
    ```

    Пример Для описанной выше среды привилегированной учетной записью является `amvin@CONTOSO.COM`.

    ```bash
    kinit amvin@CONTOSO.COM
    ```

2. С помощью программы adutil создайте пользователя, который будет выступать в роли привилегированной учетной записи Active Directory для SQL Server.

   ```bash
   adutil user create --name sqluser --distname CN=sqluser,CN=Users,DC=CONTOSO,DC=COM --password 'P@ssw0rd'
   ```

    > [!NOTE]
    > Пароли можно задавать тремя способами:
    >
    > - флаг пароля: --password \<password\>;
    > - переменные среды: `ADUTIL_ACCOUNT_PWD`;
    > - интерактивный ввод.
    >
    > Эти способы перечислены в порядке их приоритета. Рекомендуется указывать пароли с помощью переменных среды или интерактивного ввода, так как эти способы более безопасны по сравнению с флагом password.

    В качестве имени учетной записи можно указать различающееся имя (`-distname`), как показано выше, или имя подразделения. Если указаны оба имени, имя подразделения (`--ou`) имеет приоритет над различающимся именем. Чтобы получить дополнительные сведения, можно выполнить следующую команду:

    ```bash
    adutil user create --help
    ```

3. Зарегистрируйте имена субъектов-служб для созданного ранее пользователя. При необходимости можно использовать имя хост-компьютера вместо имени контейнера. Это зависит от того, как подключение должно представляться извне. В этом учебнике вместо порта 1433 используется порт 5433. Это сопоставление портов для контейнера. В вашем случае номер порта может быть другим.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H sql1.contoso.com -p 5433
    ```

    > [!NOTE]
    >
    > - `addauto` автоматически создаст имена субъектов-служб при наличии достаточных прав у учетной записи kinit.
    > - `-n`. Имя учетной записи, которой будут назначены имена субъектов-служб.
    > - `-s`. Имя службы, используемое для создания имен субъектов-служб. В этом случае они предназначены для службы SQL Server, поэтому имя службы — MSSQLSvc.
    > - `-H`. Имя узла, используемое для создания имен субъектов-служб. Если не указать, будет использоваться полное доменное имя локального узла. Укажите также полное доменное имя для имени контейнера. В этом случае имя контейнера — `sql1`, а полное доменное имя — `sql1.contoso.com`.
    > - `-p`. Порт, используемый для создания имен субъектов-служб. Если не указать, имена субъектов-служб будут создаваться без порта. Подключения SQL будут работать в этом случае, только если SQL Server прослушивает порт по умолчанию 1433.

### <a name="create-the-sql-server-service-keytab-file"></a>Создание файла keytab службы SQL Server

Создайте файл keytab, содержащий записи для каждого из четырех созданных ранее имен субъектов-служб и одну запись для пользователя. Файл keytab будет подключен к контейнеру, поэтому его можно создать в любом месте на узле. Путь можно спокойно изменять при условии, что итоговый файл keytab будет правильно подключен при использовании docker/podman для развертывания контейнера.

Чтобы создать файл keytab для всех имен субъектов-служб, можно использовать параметр `createauto`:

```bash
adutil keytab createauto -k /container/sql1/secrets/mssql.keytab -p 5433 -H sql1.contoso.com --password 'P@ssw0rd' -s MSSQLSvc
```

> [!NOTE]
>
> - `-k`. Путь, по которому необходимо создать файл `mssql.keytab`. В приведенном выше примере каталог /container/sql1/secrets уже должен существовать на узле.
> - `-p`. Порт, используемый для создания имен субъектов-служб. Если не указать, имена субъектов-служб будут создаваться без порта.
> - `-H`. Имя узла, используемое для создания имен субъектов-служб. Если не указать, будет использоваться полное доменное имя локального узла. Укажите также полное доменное имя для имени контейнера. В этом случае имя контейнера — `sql1`, а полное доменное имя — `sql1.contoso.com`.
> - `-s`. Имя службы, используемое для создания имен субъектов-служб. В этом случае они предназначены для службы SQL Server, поэтому имя службы — MSSQLSvc.
> - `--password`. Пароль созданной ранее привилегированной учетной записи пользователя Active Directory.
> - `-e` или `--enctype`: Типы шифрования для записи в файле keytab. Используйте разделенный запятыми список значений. Если этот параметр не указан, будет выводиться интерактивный запрос.

Если есть возможность выбрать типы шифрования, то можно выбрать несколько. В этом примере выбраны `aes256-cts-hmac-sha1-96` и `arcfour-hmac`. Выбранный тип шифрования должен поддерживаться узлом и доменом.

Чтобы выбрать тип шифрования в неинтерактивном режиме, можно указать его с помощью аргумента -e в приведенной выше команде. Чтобы получить дополнительные сведения о командах adutil, выполните приведенную ниже команду.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` — это слабое шифрование, поэтому его не рекомендуется использовать в рабочей среде.

Чтобы создать запись keytab для пользователя, выполните следующую команду:

```bash
adutil keytab create -k /container/sql1/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`. Путь, по которому необходимо создать файл `mssql.keytab`. В приведенном выше примере каталог /container/sql1/secrets уже должен существовать на узле.
> - `-p`. Субъект, добавляемый в файл keytab.

При создании файла keytab с помощью adutil, в том числе в автоматическом режиме, предыдущие файлы не перезаписываются. Новые записи просто добавляются в существующий файл.

У создаваемого файла keytab должны быть соответствующие разрешения при развертывании контейнера.

```bash
chmod 440 /container/sql1/secrets/mssql.keytab
```

> [!NOTE]
> На данном этапе можно скопировать файл mssql.keytab с текущего узла Linux на узел Linux, где будет развернут контейнер SQL Server, и выполнить остальные действия на этом узле. Если описанные выше действия были выполнены на узле Linux, где будут развернуты контейнеры SQL, выполните следующие действия на том же узле.

## <a name="create-the-config-files-to-be-used-by-the-sql-server-container"></a>Создание файлов конфигурации, которые будут использоваться контейнером SQL Server

1. Создайте файл `mssql.conf` с параметрами Active Directory. Его можно создать в любом месте на узле, но нужно правильно подключить при выполнении команды docker run. В этом примере файл `mssql.conf` был помещен в каталог `/container/sql1 `, то есть каталог контейнера. Содержимое файла `mssql.conf` показано ниже:

    ```output
    [network]
    privilegedadaccount = sqluser
    kerberoskeytabfile = /var/opt/mssql/secrets/mssql.keytab
    ```

    > [!NOTE]
    >
    > - `privilagedadaccount`. Привилегированный пользователь AD, используемый для проверки подлинности AD.
    > - `kerberoskeytabfile`. Путь в контейнере, где будет находиться файл mssql.keytab.

1. Создайте файл krb5.conf. Ниже приведен пример. В этих файлах регистр учитывается.

    ```output
    [libdefaults]
    default_realm = DOMAIN.COM

    [realms]
     CONTOSO.COM = {
         kdc = adVM.contoso.com
         admin_server = adVM.contoso.com
         default_domain = CONTOSO.COM
     }

    [domain_realm]
     .contoso.com = CONTOSO.COM
     contoso.com = CONTOSO.COM


1. Copy all files, `mssql.conf`, `krb5.conf`, `mssql.keytab` to a location that will be mounted to the SQL Server container. In this example, these files are placed on the host at the following locations: `mssql.conf` and `krb5.conf` at `/container/sql1/`. `mssql.keytab` is placed at the location `/container/sql1/secrets/`.

1. Make sure there's enough permission on these folders for the user running the docker/podman command. When the container starts, the user needs access to the folder path created. In this example, we provided the below permissions given to the folder path:

    ```bash
    sudo chmod 755 /container/sql1/
    ```

## <a name="mount-the-config-files-and-deploy-the-sql-server-container"></a>Подключение файлов конфигурации и развертывание контейнера SQL Server

Запустите контейнер SQL Server и подключите соответствующие файлы конфигурации Active Directory, которые были созданы ранее, как показано ниже.

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=\<YourStrong@Passw0rd\>" \
-p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
> При запуске контейнера в LSM (модуль безопасности Linux), например на узлах с системой SELinux, необходимо подключить тома с использованием параметра `Z`, который предписывает docker применить к содержимому метку закрытых данных без общего доступа. Дополнительные сведения см. в разделе о [настройке метки selinux](https://docs.docker.com/storage/bind-mounts/#configure-the-selinux-label).

Наш пример включает следующие команды:

```bash
sudo docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 5433:1433 --name sql1 \
-v /container/sql1:/var/opt/mssql/ \
-v /container/sql1/krb5.conf:/etc/krb5.conf \
--dns-search contoso.com \
--dns 10.0.0.4 \
--add-host adVM.contoso.com:10.0.0.4 \
--add-host contoso.com:10.0.0.4 \
--add-host contoso:10.0.0.4 \
-d mcr.microsoft.com/mssql/server:2019-latest
```

> [!NOTE]
>
> - Файлы `mssql.conf` и `krb5.conf` находятся на узле по пути `/container/sql1`.
> - Созданный файл `mssql.keytab` находится на узле по пути `/container/sql1/secrets`.
> - Так как хост-компьютер находится в Azure, данные Active Directory должны быть добавлены в команду docker run в том же порядке. В нашем примере контроллер домена `adVM` находится в домене `contoso.com` с IP-адресом `10.0.0.4`. В контроллере домена выполняются службы DNS и KDC.

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Создание имен входа SQL Server на основе Active Directory с помощью Transact-SQL

Подключитесь к контейнеру SQL и выполните приведенные ниже команды, чтобы создать имя входа, а затем проверьте, приводится ли оно в списке. Эту команду можно выполнить на клиентском компьютере (Windows или Linux) с SSMS, Azure Data Studio (ADS) или любым другим интерфейсом командной строки (CLI).

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Подключение к SQL Server с помощью проверки подлинности AD.

Чтобы подключиться с помощью [SSMS](../ssms/download-sql-server-management-studio-ssms.md) или [ADS](../azure-data-studio/download-azure-data-studio.md), войдите в SQL Server с учетными данными Windows, используя имя SQL Server (имя контейнера или имя узла) и номер порта. В нашем примере имя сервера — `sql1.contoso.com, 5433`.

Для подключения к SQL Server в контейнере можно также использовать такое средство, как [sqlcmd](../tools/sqlcmd-utility.md).

```bash
sqlcmd -E -S 'sql1.contoso.com, 5433'
```

## <a name="next-steps"></a>Дальнейшие действия

- [Краткое руководство. Запуск образов контейнеров SQL Server с использованием Docker](quickstart-install-connect-docker.md)
- [Присоединение SQL Server на узле Linux к домену Active Directory](sql-server-linux-active-directory-auth-overview.md)

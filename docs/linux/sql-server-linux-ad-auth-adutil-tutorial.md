---
title: Настройка проверки подлинности Active Directory в SQL Server на Linux с помощью adutil
description: Пошаговые инструкции по настройке проверки подлинности Active Directory в SQL Server на Linux с помощью adutil
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.date: 12/10/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 || =sqlallproducts-allversions'
ms.openlocfilehash: f4b084d020c40bf57ae5cbaa2c5a1306808ec1d2
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101834791"
---
# <a name="tutorial-configure-active-directory-authentication-with-sql-server-on-linux-using-adutil"></a>Руководство по Настройка проверки подлинности Active Directory в SQL Server на Linux с помощью adutil

> [!NOTE]
> В настоящее время программа **adutil** предоставляется в режиме **общедоступной предварительной версии**.

В этом учебнике приводятся инструкции по настройке проверки подлинности Active Directory (AD) для SQL Server на Linux с помощью adutil. Другой способ настройки проверки подлинности Active Directory с помощью ktpass см. в статье [Учебник. Использование проверки подлинности Azure Active Directory с SQL Server на Linux](sql-server-linux-active-directory-authentication.md).

В этом руководстве рассматриваются следующие задачи:

> [!div class="checklist"]
> - Установка adutil-preview
> - Присоединение компьютера Linux к домену Active Directory
> - Создание пользователя Active Directory для SQL Server и настройка свойства ServicePrincipalName (имени субъекта-службы) с помощью программы adutil
> - Создание файла keytab службы SQL Server
> - Настройка SQL Server для использования файла keytab
> - Создание имен входа SQL Server на основе Active Directory с помощью Transact-SQL
> - Подключение к SQL Server с помощью проверки подлинности Active Directory

## <a name="prerequisites"></a>Предварительные условия

Перед настройкой проверки подлинности Active Directory необходимо выполнить указанные ниже условия.

- В сети должен быть контроллер домена Active Directory (Windows).
- На хост-компьютере Linux должна быть установлена программа adutil-preview. Чтобы установить программу adutil-preview, следуйте приведенным ниже указаниям для используемого дистрибутива Linux.

## <a name="install-adutil-preview"></a>Установка adutil-preview

Чтобы установить программу adutil-preview, на хост-компьютере Linux выполните приведенные ниже команды.

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

1. Выполните приведенные ниже команды для установки adutil-preview. Используйте параметр `ACCEPT_EULA=Y`, чтобы принять лицензионное соглашение для предварительной версии adutil. Лицензионное соглашение находится по пути /usr/share/adutil/.

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

1. Выполните приведенную ниже команду для установки adutil-preview. Используйте параметр `ACCEPT_EULA=Y`, чтобы принять лицензионное соглашение для предварительной версии adutil. Лицензионное соглашение находится по пути /usr/share/adutil/.

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

1. Выполните приведенную ниже команду для установки adutil-preview. Используйте параметр `ACCEPT_EULA=Y`, чтобы принять лицензионное соглашение для предварительной версии adutil. Лицензионное соглашение находится по пути /usr/share/adutil/.

    ```bash
    sudo ACCEPT_EULA=Y zypper install -y adutil-preview
    ```

## <a name="domain-machine-preparation"></a>Подготовка компьютера домена

В Active Directory должна быть добавлена запись узла переадресации (A) для IP-адреса узла Linux. В этом учебнике IP-адрес хост-компьютера `myubuntu` — `10.0.0.10`. Мы добавим указанную ниже запись узла переадресации в Active Directory. Она гарантирует, что при подключении пользователей к myubuntu.contoso.com они будут обращаться к нужному узлу.

:::image type="content" source="media/sql-server-linux-ad-auth-adutil-tutorial/host-a-record.png" alt-text="добавление записи узла":::

В этом учебнике используется среда Azure с тремя виртуальными машинами. Первая виртуальная машина выступает в роли контроллера домена Windows с доменным именем `contoso.com`. Контроллер домена имеет имя `adVM.contoso.com`. Вторая машина — это компьютер Windows с именем `winbox`. На нем установлена ОС Windows 10 Desktop, и он используется в качестве клиента со средой SQL Server Management Studio (SSMS). Третья машина — это компьютер с Ubuntu 18.04 LTS. Он имеет имя `myubuntu`, и в нем размещается SQL Server.

## <a name="join-the-linux-host-machine-to-your-ad-domain"></a>Присоединение хост-компьютера Linux к домену Active Directory

Присоедините узел SQL Server на Linux к контроллеру домена Active Directory. Сведения о присоединении к домену Active Directory см. в статье [Присоединение узла SQL Server на Linux к домену Active Directory](sql-server-linux-active-directory-join-domain.md).

## <a name="create-an-ad-user-for-sql-server-and-set-the-serviceprincipalname-spn-using-the-adutil-tool"></a>Создание пользователя Active Directory для SQL Server и настройка свойства ServicePrincipalName (имени субъекта-службы) с помощью программы adutil

1. Получите или продлите билет предоставления билета (TGT) Kerberos с помощью команды `kinit`. Для выполнения команды `kinit` используйте привилегированную учетную запись. Учетная запись должна иметь разрешение на подключение к домену, а также возможность создавать учетные записи и имена субъектов-служб в домене.

    > [!IMPORTANT]
    > Перед выполнением этой команды хост-компьютер уже должен быть включен в домен согласно предыдущим инструкциям.

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

3. Зарегистрируйте имена субъектов-служб для созданного ранее субъекта. Используйте полное доменное имя компьютера. В этом учебнике используется порт SQL Server по умолчанию 1433. В вашем случае номер порта может быть другим.

    ```bash
    adutil spn addauto -n sqluser -s MSSQLSvc -H myubuntu.contoso.com -p 1433
    ```

    > [!NOTE]
    >
    > - `addauto` автоматически создаст имена субъектов-служб при наличии достаточных прав у учетной записи kinit.
    > - `-n`. Имя учетной записи, которой будут назначены имена субъектов-служб.
    > - `-s`. Имя службы, используемое для создания имен субъектов-служб. В этом случае они предназначены для службы SQL Server, поэтому имя службы — `MSSQLSvc`.
    > - `-H`. Имя узла, используемое для создания имен субъектов-служб. Если не указать, будет использоваться полное доменное имя локального узла. В этом случае имя узла — `myubuntu`, а полное доменное имя — `myubuntu.contoso.com`.
    > - `-p`. Порт, используемый для создания имен субъектов-служб. Если не указать, имена субъектов-служб будут создаваться без порта. Подключения SQL будут работать в этом случае, только если SQL Server прослушивает порт по умолчанию 1433.

## <a name="create-the-sql-server-service-keytab-file"></a>Создание файла keytab службы SQL Server

Создайте файл keytab, содержащий записи для каждого из четырех созданных ранее имен субъектов-служб и одну запись для пользователя.

```bash
adutil keytab createauto -k /var/opt/mssql/secrets/mssql.keytab -p 1433 -H myubuntu.contoso.com --password 'P@ssw0rd' -s MSSQLSvc 
```

> [!NOTE]
>
> - `-k`. Путь, по которому необходимо создать файл `mssql.keytab`. В приведенном выше примере каталог `/var/opt/mssql/secrets/` уже должен существовать на узле.
> - `-p`. Порт, используемый для создания имен субъектов-служб. Если не указать, имена субъектов-служб будут создаваться без порта.
> - `-H`. Имя узла, используемое для создания имен субъектов-служб. Если не указать, будет использоваться полное доменное имя локального узла. В этом случае имя узла — `myubuntu`, а полное доменное имя — `myubuntu.contoso.com`.
> - `-s`. Имя службы, используемое для создания имен субъектов-служб. В этом случае они предназначены для службы SQL Server, поэтому имя службы — `MSSQLSvc`.
> - `--password`. Пароль созданной ранее привилегированной учетной записи пользователя Active Directory.
> - `-e` или `--enctype`: Типы шифрования для записи в файле keytab. Используйте разделенный запятыми список значений. Если этот параметр не указан, будет выводиться интерактивный запрос.

Если есть возможность выбрать типы шифрования, то можно выбрать несколько. В этом примере выбраны `aes256-cts-hmac-sha1-96` и `arcfour-hmac`. Выбранный тип шифрования должен поддерживаться узлом и доменом.

Чтобы выбрать тип шифрования в неинтерактивном режиме, можно указать его с помощью аргумента -e в приведенной выше команде. Чтобы получить дополнительные сведения о командах adutil, выполните приведенную ниже команду.

```bash
adutil keytab createauto --help
```

> [!NOTE]
> `arcfour-hmac` — это слабое шифрование, поэтому его не рекомендуется использовать в рабочей среде.

Добавьте в файл keytab запись с именем и паролем субъекта, которые будут использоваться для подключения SQL Server к Active Directory:

```bash
adutil keytab create -k /var/opt/mssql/secrets/mssql.keytab -p sqluser --password 'P@ssw0rd!'
```

> [!NOTE]
>
> - `-k`. Путь, по которому необходимо создать файл `mssql.keytab`.
> - `-p`. Субъект, добавляемый в файл keytab.

При создании файла keytab с помощью adutil, в том числе в автоматическом режиме, предыдущие файлы не перезаписываются. Новые записи просто добавляются в существующий файл.

Создаваемый файл keytab должен принадлежать пользователю `mssql`, и только у пользователя `mssql` должен быть доступ к нему для чтения и записи. Вы можете выполнить команды `chown` и `chmod`, как показано ниже.

```bash
chown mssql. /var/opt/mssql/secrets/mssql.keytab
chmod 440 /var/opt/mssql/secrets/mssql.keytab
```

## <a name="configure-sql-server-to-use-the-keytab"></a>Настройка SQL Server для использования файла keytab

Чтобы настроить SQL Server для использования файла keytab, созданного на предыдущем шаге, и задать в качестве привилегированной учетной записи Active Directory созданного ранее пользователя, выполните приведенные ниже команды. В нашем примере имя пользователя — `sqluser`.

```bash
/opt/mssql/bin/mssql-conf set network.kerberoskeytabfile /var/opt/mssql/secrets/mssql.keytab
/opt/mssql/bin/mssql-conf set network.privilegedadaccount sqluser
```

## <a name="restart-sql-server"></a>Перезапуск SQL Server

Чтобы перезапустить службу SQL Server, выполните следующую команду:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-ad-based-sql-server-logins-in-transact-sql"></a>Создание имен входа SQL Server на основе Active Directory с помощью Transact-SQL

Подключитесь к SQL Server и выполните приведенные ниже команды, чтобы создать имя входа, а затем проверьте, приводится ли оно в списке.

```sql
create login [contoso\amvin] From Windows
SELECT name FROM sys.server_principals;
```

## <a name="connect-to-sql-server-using-ad-authentication"></a>Подключение к SQL Server с помощью проверки подлинности AD.

Чтобы подключиться с помощью [SSMS](../ssms/download-sql-server-management-studio-ssms.md) или [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), войдите в SQL Server с учетными данными Windows.

Для подключения к SQL Server с использованием проверки подлинности Windows можно также использовать такое средство, как [sqlcmd](../tools/sqlcmd-utility.md).

```bash
sqlcmd -E -S 'myubuntu.contoso.com'
```

## <a name="next-steps"></a>Дальнейшие действия

- [Присоединение SQL Server на узле Linux к домену Active Directory](sql-server-linux-active-directory-auth-overview.md)
- Сведения о настройке проверки подлинности Active Directory в контейнерах SQL Server на Linux см. в статье [Настройка проверки подлинности Active Directory в SQL Server на основе контейнеров Linux](sql-server-linux-containers-ad-auth-adutil-tutorial.md).

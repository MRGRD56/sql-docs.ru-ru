---
title: Установка подключения с использованием проверки подлинности Azure Active Directory
description: Узнайте о разработке приложений Java для использования функции проверки подлинности Azure Active Directory в Microsoft JDBC Driver for SQL Server.
ms.custom: ''
ms.date: 04/14/2021
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9c9d97be-de1d-412f-901d-5d9860c3df8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 80162239e6ef9b47f57c8dd0634bb28457c727c7
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2021
ms.locfileid: "107583968"
---
# <a name="connecting-using-azure-active-directory-authentication"></a>Установка подключения с использованием проверки подлинности Azure Active Directory

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В этой статье содержатся сведения о разработке приложений Java для использования функции проверки подлинности Azure Active Directory в Microsoft JDBC Driver for SQL Server.

Вы можете использовать проверку подлинности Azure Active Directory, которая является механизмом подключения к Базе данных SQL Azure версии 12 с помощью удостоверений в Azure Active Directory. Проверка подлинности Azure Active Directory используется для централизованного управления удостоверениями пользователей базы данных и в качестве альтернативы проверке подлинности SQL Server. JDBC Driver позволяет указать учетные данные Azure Active Directory в строке подключения JDBC для подключения к Базе данных SQL Azure. Сведения о настройке проверки подлинности Azure Active Directory см. в [этой статье](/azure/azure-sql/database/authentication-aad-overview). 

Для поддержки проверки подлинности Azure Active Directory в Microsoft JDBC Driver for SQL Server доступны следующие свойства подключения:
*   **authentication**:  с помощью этого свойства можно указать метод проверки подлинности SQL, используемый для установки подключения. Возможны следующие значения: 
    * **ActiveDirectoryMSI**
        * Поддерживается начиная с версии драйвера **7.2**. `authentication=ActiveDirectoryMSI` может использоваться для подключения к Базе данных SQL Azure или Synapse Analytics изнутри ресурса Azure с включенной поддержкой "Удостоверение". При необходимости в свойствах "Подключение" или "Источник данных" в этом режиме проверки подлинности можно указать **msiClientId**, который должен содержать идентификатор клиента Управляемого удостоверения, необходимого для получения **accessToken** при установке подключения.
    * **ActiveDirectoryIntegrated**
        * Поддерживается начиная с версии драйвера **6.0**. `authentication=ActiveDirectoryIntegrated` может использоваться для подключения к Базе данных SQL Azure или Synapse Analytics с помощью встроенной проверки подлинности. Чтобы использовать этот режим проверки подлинности, необходимо объединить локальные службы федерации Active Directory (ADFS) в федерацию с Azure Active Directory в облаке. После настройки вы можете установить подключение, добавив собственную библиотеку "mssql-jdbc_auth-\<version>-\<arch>.dll" в путь к классам приложений в ОС Windows или настроив билет Kerberos для поддержки кроссплатформенной проверки подлинности. Вы сможете получить доступ к Базе данных SQL Azure или Azure Synapse Analytics без запроса учетных данных при входе в систему компьютера, присоединенного к домену.

    * **ActiveDirectoryPassword**
        * Поддерживается начиная с версии драйвера **6.0**. `authentication=ActiveDirectoryPassword` может использоваться для подключения к Базе данных SQL Azure или Synapse Analytics с использованием имени и пароля пользователя Azure AD.
    * **ActiveDirectoryInteractive**
        * Поддерживается начиная с версии драйвера **9.2**. `authentication=ActiveDirectoryInteractive` может использоваться для подключения к Базе данных SQL Azure или Synapse Analytics с помощью интерактивного процесса проверки подлинности (многофакторной проверки подлинности).
    * **ActiveDirectoryServicePrincipal**
        * Поддерживается начиная с версии драйвера **9.2**. `authentication=ActiveDirectoryServicePrincipal` может использоваться для подключения к Базе данных SQL Azure или Synapse Analytics с использованием идентификатора клиента и секрета удостоверения субъекта-службы.

    * **SqlPassword**
        * Используйте `authentication=SqlPassword` для подключения к SQL Server с помощью свойств userName, user и password.
    * **NotSpecified**
        * Используйте `authentication=NotSpecified` или оставьте значение по умолчанию, если ни один из этих методов проверки подлинности не требуется.

*   **accessToken**. Используйте это свойство подключения для установки подключения к Базе данных SQL с помощью токена доступа. accessToken можно задать только с помощью параметра Properties метода getConnection() в классе DriverManager. Его нельзя использовать в URL-адресе подключения.  

Дополнительные сведения об установке свойства authentication см. в статье [Setting the connection properties](setting-the-connection-properties.md) (Установка свойств подключения).  


## <a name="client-setup-requirements"></a>Требования к настройке клиента
Для выполнения проверки подлинности **ActiveDirectoryMSI** на клиентском компьютере должны быть установлены следующие компоненты:
* Java 8 или более поздней версии.
* Microsoft JDBC Driver 7.2 (или более поздней версии) for SQL Server.
* В качестве клиентской среды необходимо использовать Ресурс Azure. Кроме того, в среде необходимо включить поддержку функции "Удостоверение".
* Пользователь автономной базы данных, представляющий управляемое удостоверение, назначаемое системой, или управляемое удостоверение, назначаемое пользователем, Ресурса Azure или одну из групп, к которым принадлежит ваше управляемое удостоверение, должен существовать в целевой базе данных и иметь разрешение CONNECT.

Для других режимов проверки подлинности на клиентском компьютере должны быть установлены следующие компоненты:
* Java 8 или более поздней версии.
* Microsoft JDBC Driver 6.0 (или более поздней версии) for SQL Server.
* Если вы используете режим проверки подлинности на основе маркеров доступа, то для выполнения примеров из этой статьи необходима либо [Библиотека проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий, либо [Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Дополнительные сведения см. в разделе [Установка подключения с использованием токена доступа](#connecting-using-access-token).

* Если вы используете режим проверки подлинности **ActiveDirectoryPassword**, то необходима либо [Библиотека проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий, либо [Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Дополнительные сведения см. в разделе [Установка подключения с использованием режима проверки подлинности ActiveDirectoryPassword](#connecting-using-activedirectorypassword-authentication-mode).
* Если вы используете режим проверки подлинности **ActiveDirectoryIntegrated**, то необходима либо [Библиотека проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий, либо [Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Дополнительные сведения см. в разделе [Установка подключения с использованием режима проверки подлинности ActiveDirectoryIntegrated](#connecting-using-activedirectoryintegrated-authentication-mode).
* Если вы используете режим проверки подлинности **ActiveDirectoryInteractive**, то необходима либо [Библиотека проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий, либо [Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Дополнительные сведения см. в разделе [Установка подключения с использованием режима проверки подлинности ActiveDirectoryInteractive](#connecting-using-activedirectoryinteractive-authentication-mode).
* Если вы используете режим проверки подлинности **ActiveDirectoryServicePrincipal**, то необходима либо [Библиотека проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий, либо [Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Дополнительные сведения см. в разделе [Установка подключения с использованием режима проверки подлинности ActiveDirectoryServicePrincipal](#connecting-using-activedirectoryserviceprincipal-authentication-mode).


## <a name="connecting-using-activedirectorymsi-authentication-mode"></a>Установка подключения с использованием режима проверки подлинности ActiveDirectoryMSI
Следующий пример иллюстрирует использование режима `authentication=ActiveDirectoryMSI`. Запустите этот пример из Ресурса Azure, например виртуальной машины Azure, Службы приложений или приложения-функции, включенных в федерацию с Azure Active Directory.

Перед выполнением примера замените имена сервера и базы данных именами ваших сервера и базы данных в следующих строках:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
//Optional
ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used
```

Пример использования режима проверки подлинности ActiveDirectoryMSI:

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AAD_MSI {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryMSI");
        // Optional
        ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

Выполнение этого примера на виртуальной машине Azure извлекает токен доступа из _управляемого удостоверения, назначаемого системой,_ или _управляемого удостоверения, назначаемого пользователем,_ (если указано значение **msiClientId**) и устанавливает подключение с помощью этого токена. Если подключение установлено, отобразится следующее сообщение:

```bash
You have successfully logged on as: <your Managed Identity username>
```

## <a name="connecting-using-activedirectoryintegrated-authentication-mode"></a>Установка подключения с использованием режима проверки подлинности ActiveDirectoryIntegrated
В Microsoft JDBC Driver версии 6.4 добавлена поддержка проверки подлинности ActiveDirectoryIntegrated с помощью билета Kerberos на нескольких платформах (Windows, Linux и macOS).
Дополнительные сведения см. в разделе [Настройка билета Kerberos в Windows, Linux и macOS](#set-kerberos-ticket-on-windows-linux-and-macos). Кроме того, в Windows можно выполнять проверку подлинности ActiveDirectoryIntegrated с помощью JDBC Driver, используя файл mssql-jdbc_auth-\<version>-\<arch>.dll.

> [!NOTE]
>  Если у вас установлена более старая версия драйвера, проверьте наличие соответствующих зависимостей, необходимых для использования этого режима проверки подлинности, перейдя по этой [ссылке](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). 

Следующий пример иллюстрирует использование режима `authentication=ActiveDirectoryIntegrated`. Запустите этот пример на присоединенном к домену компьютере, включенном в федерацию с Azure Active Directory. Пользователь автономной базы данных, представляющий вашего пользователя Azure Active Directory или одну из групп, к которым вы принадлежите, должен существовать в базе данных и иметь разрешение CONNECT. 

Перед сборкой и запуском примера скачайте на клиентском компьютере (где будет запускаться пример) [Библиотеку проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий либо [Библиотеку проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Включите эти компоненты в путь сборки Java.

Перед выполнением примера замените имена сервера и базы данных именами ваших сервера и базы данных в следующих строках:

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
```

Пример использования режима проверки подлинности ActiveDirectoryIntegrated:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADIntegrated {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryIntegrated");

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

При выполнении этого примера на клиентском компьютере автоматически используется билет Kerberos. При этом пароль не требуется. Если подключение установлено, отобразится следующее сообщение:

```
You have successfully logged on as: <your domain user name>
```

### <a name="set-kerberos-ticket-on-windows-linux-and-macos"></a>Настройка билета Kerberos в Windows, Linux и macOS

Вам необходимо настроить билет Kerberos, связывающий текущего пользователя с учетной записью домена Windows. Ниже приведено краткое описание основных шагов.

#### <a name="windows"></a>Windows
JDK поставляется с программой `kinit`, которую можно использовать для получения TGT из центра распространения ключей (KDC) на присоединенном к домену компьютере, включенном в федерацию с Azure Active Directory.

##### <a name="step-1-ticket-granting-ticket-retrieval"></a>Шаг 1. Извлечение билета на получение билетов
- **Выполнение в**: Windows
- **Action** (Действие).
  - Выполните команду `kinit username@DOMAIN.COMPANY.COM`, чтобы получить TGT из KDC, и появится запрос на пароль вашего домена.
  - Выполните команду `klist`, чтобы просмотреть доступные билеты. Если команда kinit выполнена успешно, отобразится билет из krbtgt/DOMAIN.COMPANY.COM@DOMAIN.COMPANY.COM.

> [!NOTE]
>  Чтобы приложение нашло KDC, может потребоваться указать файл `.ini` с `-Djava.security.krb5.conf`.

#### <a name="linux-and-macos"></a>Linux и macOS

##### <a name="requirements"></a>Требования
Доступ к компьютеру, присоединенному к домену Windows, для обращения к контроллеру домена Kerberos.

##### <a name="step-1-find-kerberos-kdc"></a>Шаг 1. Поиск центра распространения ключей Kerberos
- **Выполнение в**: Командная строка Windows
- **Действие**: `nltest /dsgetdc:DOMAIN.COMPANY.COM` (где DOMAIN.COMPANY.COM — ваше доменное имя).
- **Образец вывода**
  ```
  DC: \\co1-red-dc-33.domain.company.com
  Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
  ...
  The command completed successfully
  ```
- **Информация для извлечения**: доменное имя, в данном случае — `co1-red-dc-33.domain.company.com`.

##### <a name="step-2-configuring-kdc-in-krb5conf"></a>Шаг 2. Настройка KDC в файле krb5.conf
- **Выполнение в**: Linux/macOS
- **Action** (Действие). Измените /etc/krb5.conf в любом редакторе. Настройте следующие ключи.
  ```
  [libdefaults]
    default_realm = DOMAIN.COMPANY.COM
   
  [realms]
  DOMAIN.COMPANY.COM = {
     kdc = co1-red-dc-28.domain.company.com
  }
  ```
  Сохраните файл krb5.conf и закройте редактор.

> [!NOTE]
>  Домен должен быть указан прописными буквами.

##### <a name="step-3-testing-the-ticket-granting-ticket-retrieval"></a>Шаг 3. Тестирование извлечения билета на получение билетов
- **Выполнение в**: Linux/macOS
- **Action** (Действие).
  - Выполните команду `kinit username@DOMAIN.COMPANY.COM`, чтобы получить TGT из KDC, и появится запрос на пароль вашего домена.
  - Выполните команду `klist`, чтобы просмотреть доступные билеты. Если команда kinit выполнена успешно, отобразится билет из krbtgt/DOMAIN.COMPANY.COM@DOMAIN.COMPANY.COM.

## <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Установка подключения с использованием режима проверки подлинности ActiveDirectoryPassword
Следующий пример иллюстрирует использование режима `authentication=ActiveDirectoryPassword`.

Перед сборкой и запуском примера выполните следующие действия:
1.  Скачайте на клиентском компьютере (где будет запускаться пример) [Библиотеку проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий либо [Библиотеку проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Включите эти компоненты в путь сборки Java.
2.  Найдите приведенные ниже строки кода и замените имена сервера и базы данных именами своих сервера и базы данных.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Найдите приведенные ниже строки кода и замените имя пользователя именем пользователя Azure AD, от имени которого вы хотите установить подключение.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ds.setPassword("password");     // replace with your password
    ```

Пример использования режима проверка подлинности ActiveDirectoryPassword:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADUserPassword {
    
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        ds.setPassword("password"); // Replace with your password
        ds.setAuthentication("ActiveDirectoryPassword");
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Если подключение установлено успешно, отобразится следующее сообщение:
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Обязательно создайте автономную пользовательскую базу данных. Пользователь автономной базы данных, представляющий указанного пользователя Azure Active Directory или одну из групп, к которым принадлежит указанный пользователь Azure Active Directory, должен существовать в этой базе данных с разрешением CONNECT (за исключением администратора сервера или группы Azure Active Directory).

## <a name="connecting-using-activedirectoryinteractive-authentication-mode"></a>Установка подключения с использованием режима проверки подлинности ActiveDirectoryInteractive 
Следующий пример иллюстрирует использование режима `authentication=ActiveDirectoryInteractive`.

Перед сборкой и запуском примера выполните следующие действия:
1.  Скачайте на клиентском компьютере (где будет запускаться пример) [Библиотеку проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий либо [Библиотеку проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Включите эти компоненты в путь сборки Java.
2.  Найдите приведенные ниже строки кода и замените имена сервера и базы данных именами своих сервера и базы данных.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Найдите приведенные ниже строки кода и замените имя пользователя именем пользователя Azure AD, от имени которого вы хотите установить подключение.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

Пример использования режима проверки подлинности ActiveDirectoryInteractive:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADInteractive {
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryInteractive");
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
При запуске программы откроется браузер, в котором пользователю нужно пройти проверку подлинности. Конкретный интерфейс зависит от того, как настроена служба Azure AD. При многофакторной проверке подлинности может запрашиваться такая информация,как имя пользователя, пароль или ПИН-код, либо требоваться проверка подлинности через дополнительное устройство, например телефон. Когда в одной программе выполняется несколько интерактивных запросов проверки подлинности, пользователь может даже не получать последующие запросы, если библиотека проверки подлинности может повторно использовать ранее кэшированный маркер.

Сведения о том, как настроить Azure AD таким образом, чтобы требовалась многофакторная проверка подлинности, см. в статье [Приступая к работе со службой Многофакторной идентификации Azure AD в облаке](/azure/active-directory/authentication/howto-mfa-getstarted).

Снимки экрана этих диалоговых окон см. в статье [Настройка Многофакторной идентификации для SQL Server Management Studio и Azure AD](/azure/azure-sql/database/authentication-mfa-ssms-configure).

Если проверка подлинности пользователя успешно завершена, в браузере должно появиться следующее сообщение.
```
Authentication complete. You can close the browser and return to the application.
```
Обратите внимание, что оно означает лишь то, что проверка подлинности пройдена успешно, но необязательно то, что соединение с сервером установлено. Если подключение к серверу установлено, вернувшись в приложение, вы должны увидеть следующее сообщение.
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Обязательно создайте автономную пользовательскую базу данных. Пользователь автономной базы данных, представляющий указанного пользователя Azure Active Directory или одну из групп, к которым принадлежит указанный пользователь Azure Active Directory, должен существовать в этой базе данных с разрешением CONNECT (за исключением администратора сервера или группы Azure Active Directory).

## <a name="connecting-using-activedirectoryserviceprincipal-authentication-mode"></a>Установка подключения с использованием режима проверки подлинности ActiveDirectoryServicePrincipal
Следующий пример иллюстрирует использование режима `authentication=ActiveDirectoryServicePrincipal`.

Перед сборкой и запуском примера выполните следующие действия:
1.  Скачайте на клиентском компьютере (где будет запускаться пример) [Библиотеку проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий либо [Библиотеку проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Включите эти компоненты в путь сборки Java.
2.  Найдите приведенные ниже строки кода и замените имена сервера и базы данных именами своих сервера и базы данных.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Найдите приведенные ниже строки кода и замените имя пользователя именем пользователя Azure AD, от имени которого вы хотите установить подключение.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

Пример использования режима проверки подлинности ActiveDirectoryInteractive:
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADServicePrincipal {
    public static void main(String[] args) throws Exception{
        String principalId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your AAD secure principal ID.
        String principalSecret = "..."; // Replace with your AAD principal secret.

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryServicePrincipal");
    ds.setAADSecurePrincipalId(principalId);
    ds.setAADSecurePrincipalSecret(principalSecret);
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Если подключение установлено успешно, отобразится следующее сообщение.
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Обязательно создайте автономную пользовательскую базу данных. Пользователь автономной базы данных, представляющий указанного пользователя Azure Active Directory или одну из групп, к которым принадлежит указанный пользователь Azure Active Directory, должен существовать в этой базе данных с разрешением CONNECT (за исключением администратора сервера или группы Azure Active Directory).

## <a name="connecting-using-access-token"></a>Установка подключения с использованием токена доступа
Приложения или службы могут извлечь маркер доступа из Azure Active Directory и использовать его для подключения к Базе данных SQL Azure или Synapse Analytics.

> [!NOTE] 
> **accessToken** можно задать только с помощью параметра Properties метода getConnection() в классе DriverManager. Его нельзя использовать в строке подключения.

В приведенном ниже примере показано простое приложение Java, которое подключается к Базе данных SQL Azure или Synapse Analytics с использованием проверки подлинности на основе маркеров доступа. Перед сборкой и запуском примера выполните следующие шаги:
1.  Создайте учетную запись приложения в Azure Active Directory для своей службы.
    1. Войдите на портал Azure.
    2. Щелкните Azure Active Directory в области навигации слева.
    3. Перейдите на вкладку "Регистрация приложений".
    4. На панели щелкните "Регистрация нового приложения".
    5. Введите mytokentest в качестве понятного имени для приложения. Выберите "Веб-приложение или API".
    6. Указывать URL-адрес для входа не нужно. Просто укажите следующее: https://mytokentest.
    7. Нажмите внизу кнопку "Создать".
    9. Не выходя из портала Azure, перейдите на вкладку параметров своего приложения и откройте вкладку "Свойства".
    10. Найдите значение поля "Идентификатор приложения" (также известен как "Идентификатор клиента"), скопируйте и сохраните его (например, 1846943b-ad04-4808-aa13-4702d908b5c1). Оно понадобится вам позже при настройке приложения. См. следующий моментальный снимок.
    11. В разделе "Ключи" создайте ключ, заполнив поле имени, выбрав срок действия ключа и сохранив конфигурацию (оставьте поле значения пустым). После сохранения поле значения будет заполнено автоматически. Скопируйте сгенерированное значение. Это секрет клиента.
    12. На панели слева щелкните Azure Active Directory. В разделе "Регистрация приложений" найдите вкладку End points (Конечные точки). Скопируйте URL-адрес в разделе OATH 2.0 TOKEN ENDPOINT (Конечная точка токена OATH 2.0). Это URL-адрес службы токенов безопасности.
    
    ![Конечная точка для регистрации приложений на портале Azure — URL-адрес службы токенов безопасности](media/jdbc_aad_token.png)  
2. Войдите в систему пользовательской базы данных Azure SQL Server в качестве администратора Azure Active Directory и с помощью команды T-SQL подготовьте пользователя автономной базы данных для субъекта приложения. Сведения о создании администратора Azure Active Directory и пользователя автономной базы данных см. в разделе [Подключение к Базе данных SQL или Azure Synapse Analytics с помощью проверки подлинности Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).

    ```
    CREATE USER [mytokentest] FROM EXTERNAL PROVIDER
    ```

3.  Скачайте на клиентском компьютере (где будет запускаться пример) [Библиотеку проверки подлинности Майкрософт (MSAL) для Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) и ее зависимости для JDBC Driver 9.1 и более поздних версий либо [Библиотеку проверки подлинности Microsoft Azure Active Directory (ADAL) для Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) и ее зависимости для версий драйвера до JDBC Driver 9.1. Включите эти компоненты в путь сборки Java. Обратите внимание, что библиотека microsoft-authentication-library-for-java требуется только для запуска этого конкретного примера. В данном примере API-интерфейсы из этой библиотеки используются для получения токена доступа из Azure AD. Если у вас уже есть токен доступа, вы можете пропустить этот шаг. Обратите внимание, что вам также необходимо удалить в примере раздел, который извлекает токен доступа.

В приведенном ниже примере замените собственными значениями URL-адрес службы токенов безопасности, идентификатор клиента, секрет клиента, имя сервера и базы данных.

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

// The azure-activedirectory-library-for-java is needed to retrieve the access token from the AD.
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.IClientCredential;

public class AADTokenBased {

    public static void main(String[] args) throws Exception {

        // Retrieve the access token from the AD.
        String spn = "https://database.windows.net/";
        String stsurl = "https://login.microsoftonline.com/..."; // Replace with your STS URL.
        String clientId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your client ID.
        String clientSecret = "..."; // Replace with your client secret.

    String scope = spn +  "/.default";
    Set<String> scopes = new HashSet<>();
        scopes.add(scope);
    
    ExecutorService executorService = Executors.newSingleThreadExecutor();
    IClientCredential credential = ClientCredentialFactory.createFromSecret(clientSecret);
    ConfidentialClientApplication clientApplication = ConfidentialClientApplication
            .builder(clientId, credential).executorService(executorService).authority(stsurl).build();
    CompletableFuture<IAuthenticationResult> future = clientApplication
            .acquireToken(ClientCredentialParameters.builder(scopes).build());
            
    IAuthenticationResult authenticationResult = future.get();
    String accessToken = authenticationResult.accessToken();

        System.out.println("Access Token: " + accessToken);

        // Connect with the access token.
        SQLServerDataSource ds = new SQLServerDataSource();

        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name.
        ds.setDatabaseName("demo"); // Replace with your database name.
        ds.setAccessToken(accessToken);

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
``` 

Если подключение установлено успешно, отобразится следующее сообщение:

```bash
Access Token: <your access token>
You have successfully logged on as: <your client ID>    
```

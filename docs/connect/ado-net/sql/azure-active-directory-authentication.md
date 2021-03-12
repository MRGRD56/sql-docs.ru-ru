---
title: Использование проверки подлинности Azure Active Directory для SqlClient
description: Узнайте, как использовать поддерживаемые режимы проверки подлинности Azure Active Directory для подключения к источникам данных Azure SQL с помощью SqlClient
ms.date: 11/20/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: karinazhou
ms.author: v-jizho2
ms.reviewer: v-daenge
ms.openlocfilehash: c57c2d10854ed902a6230eafc3a912cd0508c989
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836017"
---
# <a name="using-azure-active-directory-authentication-with-sqlclient"></a>Использование проверки подлинности Azure Active Directory для SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

В этой статье показано, как подключаться к источникам данных Azure SQL с помощью проверки подлинности Azure Active Directory (AAD) из приложения .NET с помощью SqlClient.

Проверка подлинности AAD использует удостоверения AAD для доступа к источникам данных Azure SQL, таким как База данных SQL Azure, Управляемый экземпляр Azure SQL и Azure Synapse Analytics. Пространство имен **Microsoft.Data.SqlClient** позволяет клиентским приложениям указывать учетные данные AAD в разных режимах проверки подлинности при подключении к Базе данных SQL Azure.

Указывая свойство подключения `Authentication` в строке подключения, клиент может выбрать предпочтительный режим проверки подлинности AAD с учетом указанного значения:

- Самая ранняя версия **Microsoft.Data.SqlClient** поддерживает `Active Directory Password` для .NET Framework, .NET Core и .NET Standard. Также она поддерживает проверку подлинности `Active Directory Integrated` и `Active Directory Interactive` для .NET Framework. 

- Начиная с версии **Microsoft.Data.SqlClient** 2.0.0, проверка подлинности `Active Directory Integrated` и `Active Directory Interactive` также поддерживается для .NET Framework, .NET Core и .NET Standard. 

  Также в SqlClient 2.0.0 добавлен новый режим проверки подлинности `Active Directory Service Principal`. Для выполнения проверки подлинности используется идентификатор клиента и секрет удостоверения субъекта-службы. 

- Дополнительные режимы проверки подлинности добавлены в **Microsoft.Data.SqlClient** версии 2.1.0, в том числе `Active Directory Device Code Flow` и `Active Directory Managed Identity` (или `Active Directory MSI`). Эти новые режимы позволяют приложению получить маркер доступа для подключения к серверу. 

Если вам недостаточно сведений о проверке подлинности Azure AD, которые представлены в следующих разделах, см. статью [Использование проверки подлинности Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).

## <a name="setting-azure-active-directory-authentication"></a>Настройка проверки подлинности Azure Active Directory

Когда приложение подключается к источникам данных SQL Azure с использованием проверки подлинности Azure AD, должен использоваться допустимый режим проверки подлинности. В следующей таблице перечислены поддерживаемые режимы проверки подлинности. Приложение может задать режим через свойство подключения `Authentication` в строке подключения.

| Значение | Описание  | Инфраструктура    | Версия Microsoft.Data.SqlClient |
|:--|:--|:--|:--:|
| Пароль Active Directory | Проверка подлинности по удостоверению Azure AD с использованием имени пользователя и пароля. | .NET Framework 4.6 и выше, .NET Core 2.1 и выше, .NET Standard 2.0 и выше  | 1.0+|
| Встроенная проверка подлинности Active Directory |Проверка подлинности по удостоверению Azure AD с использованием встроенной проверки подлинности. | .NET Framework 4.6 и выше, .NET Core 2.1 и выше, .NET Standard 2.0 и выше | 2.0.0 и выше<sup>1</sup> |
| Интерактивная проверка подлинности Active Directory | Интерактивная проверка подлинности с использованием удостоверения Azure AD. | .NET Framework 4.6 и выше, .NET Core 2.1 и выше, .NET Standard 2.0 и выше | 2.0.0 и выше<sup>1</sup> |
| Субъект-служба Active Directory | Проверка подлинности по удостоверению Azure AD с использованием идентификатора клиента и секрета удостоверения субъекта-службы. | .NET Framework 4.6 и выше, .NET Core 2.1 и выше, .NET Standard 2.0 и выше | 2.0.0 и выше |
| Потока кода устройства Active Directory | Проверка подлинности по удостоверению Azure AD в режиме потока кода устройства. | .NET Framework 4.6 и выше, .NET Core 2.1 и выше, .NET Standard 2.0 и выше | 2.1.0 и выше |
| Управляемое удостоверение Active Directory, <br>Active Directory MSI | Проверка подлинности по удостоверению Azure AD с использованием управляемого удостоверения, назначаемого системой или пользователем. | .NET Framework 4.6 и выше, .NET Core 2.1 и выше, .NET Standard 2.0 и выше | 2.1.0 и выше |

<sup>1</sup> В версиях **Microsoft.Data.SqlClient** ниже 2.0.0 режимы проверки подлинности `Active Directory Integrated` и `Active Directory Interactive` поддерживаются только для .NET Framework 4.6 и выше.

## <a name="using-active-directory-password-authentication"></a>Использование проверки подлинности по паролю Active Directory

Режим проверки подлинности `Active Directory Password` поддерживает проверку подлинности в источниках данных Azure с использованием Azure AD для собственных и федеративных пользователей Azure AD. При использовании этого режима в строке подключения необходимо указать учетные данные пользователя. Следующий пример демонстрирует применение проверки подлинности `Active Directory Password`.

```c#
// Use your own server, database, user ID, and password.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Password; Database=testdb; User Id=user@domain.com; Password=***";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-integrated-authentication"></a>Использование интегрированной проверки подлинности Active Directory

Чтобы использовать режим проверки подлинности `Active Directory Integrated`, нужно настроить федерацию локального экземпляра Active Directory с Azure AD в облаке. Например, федерацию можно выполнять с помощью службы федерации Active Directory (AD FS).

В этом режиме после входа на присоединенный к домену компьютер вы сможете получать доступ к источникам данных Azure SQL, не вводя учетные данные. Имя пользователя и пароль нельзя указывать в строке подключения для приложений .NET Framework. Имя пользователя является необязательным параметром в строке подключения для приложений .NET Core и .NET Standard. В этом режиме нельзя задать свойство `Credential` для SqlConnection. 

Следующий фрагмент кода демонстрирует пример использования проверки подлинности `Active Directory Integrated`.

```c#
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is optional for .NET Core and .NET Standard.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-interactive-authentication"></a>Использование интерактивной проверки подлинности Active Directory

Проверка подлинности `Active Directory Interactive` поддерживает технологию многофакторной проверки подлинности для подключения к источникам данных Azure SQL. Если в строке подключения указан этот режим проверки подлинности, отобразится экран проверки подлинности Azure, где пользователю будет предложено ввести допустимые учетные данные. Вы не сможете указать пароль в строке подключения. 

В этом режиме нельзя задать свойство `Credential` для SqlConnection. При использовании **Microsoft.Data.SqlClient** версии 2.0.0 и выше в интерактивном режиме можно указывать имя пользователя в строке подключения. 

Следующий пример демонстрирует применение проверки подлинности `Active Directory Interactive`.

```c#
// Use your own server, database, and user ID.
// User ID is optional.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is not provided.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-service-principal-authentication"></a>Использование проверки подлинности субъекта-службы Active Directory

В режиме проверки подлинности `Active Directory Service Principal` клиентское приложение может подключаться к источникам данных SQL Azure, предоставляя идентификатор клиента и секрет удостоверения субъекта-службы. Проверка подлинности на основе субъекта-службы включает следующие действия:
1. настройка регистрации приложения с секретом;
1. предоставление приложению разрешений в экземпляре базы данных Azure SQL;
1. подключение с правильными учетными данными. 

Следующий пример демонстрирует применение проверки подлинности `Active Directory Service Principal`.

```c#
// Use your own server, database, app ID, and secret.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Service Principal; Database=testdb; User Id=AppId; Password=secret";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-device-code-flow-authentication"></a>Использование проверки подлинности в потоке кода устройства Active Directory

При использовании [библиотеки проверки подлинности Майкрософт](/azure/active-directory/develop/msal-overview) для .NET (MSAL.NET) клиентское приложение сможет использовать проверку подлинности `Active Directory Device Code Flow` для подключения к источникам данных SQL Azure с устройств и операционных систем, в которых нет интерактивного веб-браузера. Интерактивная проверка подлинности выполняется на другом устройстве. Дополнительные сведения см. в статье [Платформа удостоверений Майкрософт и поток предоставления авторизации устройства OAuth 2.0](/azure/active-directory/develop/v2-oauth2-device-code). 

Если используется этот режим, нельзя задать свойство `Credential` для `SqlConnection`. Кроме того, нельзя указать имя пользователя и пароль в строке подключения. 

Следующий фрагмент кода демонстрирует пример с проверкой подлинности `Active Directory Device Code Flow`.

```c#
// Use your own server and database.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Device Code Flow; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-managed-identity-authentication"></a>Использование проверки подлинности Active Directory с управляемым удостоверением

*Управляемые удостоверения* для ресурсов Azure — это новое название службы "Управляемое удостоверение службы" (MSI). Когда клиентское приложение использует ресурс Azure для обращения к службе Azure, которая поддерживает проверку подлинности Azure AD, вы можете использовать для проверки подлинности управляемые удостоверения, предоставив удостоверение для ресурса Azure в Azure AD. Предоставленное удостоверение можно использовать для получения маркеров доступа. При таком способе проверки подлинности управлять учетными данными и секретами не нужно.

Существует два типа управляемых удостоверений.

- _Назначаемое системой управляемое удостоверение_ создается в экземпляре службы в Azure AD. Оно привязано к жизненному циклу этого экземпляра службы. 
- _Назначаемое пользователем управляемое удостоверение_ создается как изолированный ресурс Azure. Его можно назначить одному или нескольким экземплярам службы Azure. 

Дополнительные сведения об управляемых удостоверениях см. в статье [Что такое управляемые удостоверения для ресурсов Azure?](/azure/active-directory/managed-identities-azure-resources/overview)

Начиная с версии **Microsoft.Data.SqlClient** 2.1.0, драйвер поддерживает проверку подлинности в Базе данных SQL Azure, Azure Synapse Analytics и Управляемом экземпляре Azure SQL путем получения маркеров доступа по управляемому удостоверению. Чтобы использовать такую проверку подлинности, укажите `Active Directory Managed Identity` или `Active Directory MSI` в строке подключения. Пароль при этом не требуется. 

В этом режиме также нельзя задать свойство `Credential` для `SqlConnection`. Для управляемого удостоверения, назначаемого пользователем, необходимо указать имя пользователя. 

Следующий пример демонстрирует, как использовать проверку подлинности `Active Directory Managed Identity` с управляемым удостоверением, назначаемым системой.

```c#
// For system-assigned managed identity
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```

Следующий пример демонстрирует, как использовать проверку подлинности `Active Directory Managed Identity` с управляемым удостоверением, назначаемым пользователем.

```c#
// For user-assigned managed identity
// Use your own server, database, and user ID.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="customizing-active-directory-authentication"></a>Настройка проверки подлинности Active Directory

Помимо возможности использования встроенной в драйвер проверки подлинности Active Directory, **Microsoft.Data.SqlClient** версии 2.1.0 и выше позволяет приложениям настраивать проверку подлинности Active Directory. Эта настройка основывается на классе `ActiveDirectoryAuthenticationProvider`, который является производным от абстрактного класса [`SqlAuthenticationProvider`](/dotnet/api/system.data.sqlclient.sqlauthenticationprovider). 

В процессе проверки подлинности Active Directory клиентское приложение может определить собственный класс `ActiveDirectoryAuthencationProvider` одним из следующих способов:

- через пользовательский метод обратного вызова;
- передавая идентификатора клиента приложения в библиотеку MSAL через драйвер SqlClient для получения маркеров доступа.

Следующий пример демонстрирует, как использовать пользовательский обратный вызов при работе с проверкой подлинности `Active Directory Device Code Flow`. 

[!code-csharp [AADAuthenticationCustomDeviceFlowCallback#1](~/../sqlclient/doc/samples/AADAuthenticationCustomDeviceFlowCallback.cs#1)]

Используя настраиваемый класс `ActiveDirectoryAuthenticationProvider`, можно передать определяемый пользователем идентификатор клиента в SqlClient при работе с любым поддерживаемым режимом проверки подлинности Active Directory. Поддерживаются следующие режимы проверки подлинности Active Directory: `Active Directory Password`, `Active Directory Integrated`, `Active Directory Interactive`, `Active Directory Service Principal` и `Active Directory Device Code Flow`.

Также идентификатор клиента приложения можно настроить с помощью `SqlAuthenticationProviderConfigurationSection` или `SqlClientAuthenticationProviderConfigurationSection`. Свойство конфигурации `applicationClientId` применяется к .NET Framework 4.6 и выше и .NET Core 2.1 и выше.

Следующий фрагмент кода демонстрирует пример использования настраиваемого класса `ActiveDirectoryAuthenticationProvider` с определяемым пользователем идентификатором клиента приложения при работе с проверкой подлинности `Active Directory Interactive`.

[!code-csharp [ApplicationClientIdAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/ApplicationClientIdAzureAuthenticationProvider.cs#1)]

Следующий пример демонстрирует, как задать идентификатор клиента приложения в разделе конфигурации.

```xml
<configuration>
  <configSections>
    <section name="SqlClientAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>

<!--or-->

<configuration>
  <configSections>
    <section name="SqlAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>
```

## <a name="support-for-a-custom-sql-authentication-provider"></a>Поддержка пользовательского поставщика проверки подлинности SQL

Благодаря гибким возможностям клиентское приложение может использовать собственный поставщик проверки подлинности Active Directory вместо стандартного класса `ActiveDirectoryAuthenticationProvider`. Пользовательский поставщик проверки подлинности нужно реализовать в виде подкласса `SqlAuthenticationProvider` с переопределенными методами. 

Следующий пример демонстрирует, как использовать новый поставщик проверки подлинности для проверки подлинности `Active Directory Device Code Flow`.

[!code-csharp [CustomDeviceCodeFlowAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/CustomDeviceCodeFlowAzureAuthenticationProvider.cs#1)]

Наряду с улучшением интерфейса для проверки подлинности `Active Directory Interactive` в **Microsoft.Data.SqlClient** версии 2.1.0 и выше клиентским приложениям предоставляются следующие API для настройки интерактивной проверки подлинности и проверки подлинности в потоке кода устройства.

```c#
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    // Sets a reference to the current System.Windows.Forms.IWin32Window that triggers the browser to be shown. 
    // Used to center the browser pop-up onto this window.
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    // Sets a reference to the ViewController (if using Xamarin.iOS), Activity (if using Xamarin.Android) IWin32Window, or IntPtr (if using .NET Framework). 
    // Used for invoking the browser for Active Directory Interactive authentication.
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Sets a callback method that's invoked with a custom web UI instance that will let the user sign in with Azure AD, present consent if needed, and get back the authorization code. 
    // Applicable when working with Active Directory Interactive authentication.
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Clears cached user tokens from the token provider.
    public static void ClearUserTokenCache();
}
```


## <a name="see-also"></a>См. также
- [Application and service principal objects in Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals) (Объекты приложения и субъекта-службы в Azure Active Directory)
- [Потоки проверки подлинности](/azure/active-directory/develop/msal-authentication-flows)

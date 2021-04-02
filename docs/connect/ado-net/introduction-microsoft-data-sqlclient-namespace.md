---
title: Введение в пространство имен Microsoft.Data.SqlClient
description: Узнайте подробнее о пространстве имен Microsoft.Data.SqlClient и о том, почему это предпочтительный способ подключения к SQL для приложений .NET.
ms.date: 11/19/2020
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 8fdb8f726fc33ccb5acc6129c693a5233f87e313
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633287"
---
# <a name="introduction-to-microsoftdatasqlclient-namespace"></a>Введение в пространство имен Microsoft.Data.SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="release-notes-for-microsoftdatasqlclient-21"></a>Заметки о выпуске для Microsoft.Data.SqlClient 2.1

Заметки о выпуске также доступны в репозитории GitHub: [Заметки о выпуске для версии 2.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.1).

### <a name="new-features-in-21"></a>Новые возможности в версии 2.1

### <a name="cross-platform-support-for-always-encrypted"></a>Кроссплатформенная поддержка для Always Encrypted

В Microsoft.Data.SqlClient версии 2.1 добавлена поддержка Always Encrypted для следующих платформ:

| Поддержка Always Encrypted | Поддержка Always Encrypted с безопасными анклавами  | Требуемая версия .NET Framework | Версия Microsoft.Data.SqlClient | Операционная система |
|:--|:--|:--|:--:|:--:|
| Да | Да | .NET Framework 4.6+ | 1.1.0 и выше | Windows |
| Да | Да | .NET Core 2.1+ | 2.1.0 и выше<sup>1</sup> | Windows, Linux, macOS |
| Да | Нет<sup>2</sup> | .NET Standard 2.0 | 2.1.0 и выше | Windows, Linux, macOS |
| Да | Да | .NET Standard 2.1 и выше | 2.1.0 и выше | Windows, Linux, macOS |

> [!NOTE]
> <sup>1</sup> В версиях Microsoft.Data.SqlClient ниже 2.1 функцию Always Encrypted поддерживает только Windows.
> <sup>2</sup> .NET Standard 2.0 не поддерживает Always Encrypted с безопасными анклавами.

### <a name="azure-active-directory-device-code-flow-authentication"></a>Проверка подлинности в потоке кода устройства Azure Active Directory

Microsoft.Data.SqlClient версии 2.1 поддерживает проверку подлинности "Поток кода устройства" с помощью MSAL.NET.
Справочная документация: [Поток предоставления авторизации устройства OAuth 2.0](/azure/active-directory/develop/v2-oauth2-device-code)

Пример строки подключения:

`Server=<server>.database.windows.net; Authentication=Active Directory Device Code Flow; Database=Northwind;`

Следующий API позволяет настраивать механизм обратного вызова для потока кода устройства:

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetDeviceCodeFlowCallback(Func<DeviceCodeResult, Task> deviceCodeFlowCallbackMethod)
}
```

### <a name="azure-active-directory-managed-identity-authentication"></a>Проверка подлинности Azure Active Directory с использованием управляемого удостоверения

В Microsoft.Data.SqlClient версии 2.1 добавлена поддержка проверки подлинности Azure Active Directory с использованием [управляемых удостоверений](/azure/active-directory/managed-identities-azure-resources/overview).

Режим проверки подлинности поддерживает такие ключевые слова:

- Active Directory Managed Identity (управляемое удостоверение Active Directory);
- Active Directory MSI (для совместимости с драйверами MS SQL).

Примеры строк подключения:

```cs
// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; Initial Catalog={db};"

// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"
```

### <a name="azure-active-directory-interactive-authentication-enhancements"></a>Расширения интерактивной проверки подлинности Azure Active Directory

В Microsoft.Data.SqlClient версии 2.1 добавлены следующие API-интерфейсы для настройки взаимодействия с интерактивной проверкой подлинности Active Directory:

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void ClearUserTokenCache();
}
```

### <a name="sqlclientauthenticationproviders-configuration-section"></a>Раздел конфигурации `SqlClientAuthenticationProviders`

В Microsoft.Data.SqlClient версии 2.1 добавлен новый раздел конфигурации, `SqlClientAuthenticationProviders` (это клон существующего `SqlAuthenticationProviders`). Существующий раздел конфигурации `SqlAuthenticationProviders` также поддерживается для обеспечения обратной совместимости, если определен соответствующий тип.

Этот новый раздел позволяет включать в файлы конфигурации одновременно раздел SqlAuthenticationProviders для System.Data.SqlClient и раздел SqlClientAuthenticationProviders для Microsoft.Data.SqlClient.

### <a name="azure-active-directory-authentication-using-an-application-client-id"></a>Проверка подлинности Azure Active Directory с помощью идентификатора клиента приложения

В Microsoft.Data.SqlClient версии 2.1 добавлена поддержка передачи определяемого пользователем идентификатора клиента приложения в библиотеку проверки подлинности Майкрософт. Идентификатор клиента приложения используется для проверки подлинности в Azure Active Directory.

Также добавлены следующие API-интерфейсы:

1. В ActiveDirectoryAuthenticationProvider добавлен новый конструктор:\
_[Применимо ко всем платформам .NET (.NET Framework, .NET Core и .NET Standard)]_

    ```csharp
    public ActiveDirectoryAuthenticationProvider(string applicationClientId)
    ```

    Использование:

    ```csharp
    string APP_CLIENT_ID = "<GUID>";
    SqlAuthenticationProvider customAuthProvider = new ActiveDirectoryAuthenticationProvider(APP_CLIENT_ID);
    SqlAuthenticationProvider.SetProvider(SqlAuthenticationMethod.ActiveDirectoryInteractive, customAuthProvider);
    
    using (SqlConnection sqlConnection = new SqlConnection("<connection_string>")
    {
        sqlConnection.Open();
    }
    ```

2. Добавлено новое свойство конфигурации в `SqlAuthenticationProviderConfigurationSection` и `SqlClientAuthenticationProviderConfigurationSection`:\
_[Применимо к .NET Framework и .NET Core]_

    ```csharp
    internal class SqlAuthenticationProviderConfigurationSection : ConfigurationSection
    {
        ...
        [ConfigurationProperty("applicationClientId", IsRequired = false)]
        public string ApplicationClientId => this["applicationClientId"] as string;
    }
    
    // Inheritance
    internal class SqlClientAuthenticationProviderConfigurationSection : SqlAuthenticationProviderConfigurationSection
    { ... }
    ```

    Использование:

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

### <a name="data-classification-v2-support"></a>Поддержка Классификации данных версии 2

В Microsoft.Data.SqlClient версии 2.1 добавлена поддержка информации "Ранг конфиденциальности" согласно Классификации данных. Доступны следующие API-интерфейсы:

```csharp
public class SensitivityClassification
{
    public SensitivityRank SensitivityRank;
}

public class SensitivityProperty
{
    public SensitivityRank SensitivityRank;
}

public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}
```

### <a name="server-process-id-for-an-active-sqlconnection"></a>Идентификатор процесса сервера для активного `SqlConnection`

В Microsoft.Data.SqlClient версии 2.1 введено новое свойство `SqlConnection` для активного подключения: `ServerProcessId`.

```csharp
public class SqlConnection
{
    // Returns the server process Id (SPID) of the active connection.
    public int ServerProcessId;
}
```

### <a name="trace-logging-support-in-native-sni"></a>Поддержка журнала трассировки в собственном SNI

Microsoft.Data.SqlClient версии 2.1 пополняет существующую реализацию `SqlClientEventSource` возможностью трассировки событий в SNI.dll. Эти события должны собираться таким инструментом, как Xperf.

Чтобы включить трассировку, можно отправить команду `SqlClientEventSource`, как показано ниже:

```csharp
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

### <a name="command-timeout-connection-string-property"></a>Свойство строки подключения "Command Timeout" (время ожидания команды)

В Microsoft.Data.SqlClient версии 2.1 введено новое свойство строки подключения "Command Timeout", которое позволяет переопределить стандартное время ожидания (30 секунд). Время ожидания для отдельных команд можно также переопределить с помощью свойства `CommandTimeout` в команде SqlCommand.

Примеры строк подключения:

`"Server={serverURL}; Initial Catalog={db}; Integrated Security=true; Command Timeout=60"`

### <a name="removal-of-symbols-from-native-sni"></a>Удаление символов из собственного SNI

Начиная с Microsoft.Data.SqlClient версии 2.1, мы удалили символы, добавленные в [версии 2.0.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI/2.0.0), из пакета NuGet [Microsoft.Data.SqlClient.SNI.runtime](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime) (начиная с [версии 2.1.1](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime/2.1.1)). Теперь общедоступные символы публикуются на сервере символов Майкрософт для BinSkim и других инструментов, которым требуется доступ к общедоступным символам.

### <a name="source-linking-of-microsoftdatasqlclient-symbols"></a>Сопоставление с источником для символов Microsoft.Data.SqlClient

Начиная с Microsoft.Data.SqlClient версии 2.1, символы Microsoft.Data.SqlClient сопоставляются с источником и публикуются на сервере символов Майкрософт. Это позволяет поддерживать расширенную отладку без скачивания исходного кода.

## <a name="release-notes-for-microsoftdatasqlclient-20"></a>Заметки о выпуске для Microsoft.Data.SqlClient 2.0

Заметки о выпуске также доступны в репозитории GitHub: [Заметки о выпуске 2.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.0).

### <a name="breaking-changes-in-20"></a>Критические изменения в версии 2.0

- Модификатор доступа для интерфейса поставщика анклава `SqlColumnEncryptionEnclaveProvider` изменен с `public` на `internal`.
- Константы в классе `SqlClientMetaDataCollectionNames` были обновлены, чтобы отражать изменения в SQL Server.
- Теперь драйвер будет выполнять проверку сертификата сервера, когда целевой SQL Server применяет TLS-шифрование, которое используется по умолчанию для подключений Azure.
- `SqlDataReader.GetSchemaTable()` теперь возвращает пустой `DataTable` вместо `null`.
- Теперь драйвер выполняет округление по десятичной шкале, что соответствует поведению SQL Server. Для обеспечения обратной совместимости с помощью параметра AppContext можно включить предыдущее поведение, когда выполнялось усечение.
- Для приложений .NET Framework, использующих **Microsoft.Data.SqlClient**, файлы SNI.dll, ранее скачиваемые в папки `bin\x64` и `bin\x86`, теперь называются `Microsoft.Data.SqlClient.SNI.x64.dll` и `Microsoft.Data.SqlClient.SNI.x86.dll` и будут скачиваться в каталог `bin`.
- Новые синонимы свойств строки подключения заменят старые свойства при получении строки подключения из `SqlConnectionStringBuilder` для обеспечения согласованности. [Подробнее](#new-connection-string-property-synonyms)

### <a name="new-features-in-20"></a>Новые возможности в версии 2.0

В Microsoft.Data.SqlClient 2.0 появились следующие новые возможности.

#### <a name="dns-failure-resiliency"></a>Устойчивость к сбоям DNS

Теперь драйвер будет кэшировать IP-адреса всех успешных подключений к конечной точке SQL Server, которая поддерживает эту функцию. Если во время попытки подключения происходит сбой разрешения DNS, драйвер пытается установить подключение с помощью кэшированного IP-адреса для этого сервера, если таковой существует.

#### <a name="eventsource-tracing"></a>Трассировка EventSource

В этом выпуске реализована поддержка записи журналов трассировки событий для отладки приложений. Чтобы записать эти события, клиентские приложения должны ожидать передачи данных о событиях от реализации EventSource для SqlClient.

```csharp
Microsoft.Data.SqlClient.EventSource
```

Дополнительные сведения см. в разделе [Включение трассировки событий в SqlClient](enable-eventsource-tracing.md).

#### <a name="enabling-managed-networking-on-windows"></a>Включение управляемых сетей в Windows

Новый параметр AppContext **Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows** позволяет использовать управляемую реализацию SNI в Windows для тестирования и отладки. Этот параметр позволяет переключить поведение драйвера на использование управляемого SNI в проектах .NET Core 2.1+ и .NET Standard 2.0+ в Windows, устраняя все зависимости от собственных библиотек для библиотеки Microsoft.Data.SqlClient.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Полный список доступных параметров в драйвере см. в разделе [Параметры AppContext в SqlClient](appcontext-switches.md).

#### <a name="enabling-decimal-truncation-behavior"></a>Включение усечения десятичных чисел

Драйвер будет по умолчанию округлять десятичные числа, как это делается SQL Server. Для обеспечения обратной совместимости можно задать для параметра AppContext **Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal** значение **true**.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

#### <a name="new-connection-string-property-synonyms"></a>Новые синонимы свойств строки подключения

Для следующих свойств строки подключения были добавлены новые синонимы, чтобы избежать путаницы в свойствах, состоящих более чем из одного слова. Старые имена свойств будут по-прежнему поддерживаться для обратной совместимости, но при получении строки подключения из [SqlConnectionStringBuilder](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder) будут включаться новые свойства строки подключения.

|Существующее свойство строки подключения|Новый синоним|
|-----------------------------------|-----------|
| ApplicationIntent | Назначение приложения |
| ConnectRetryCount | Connect Retry Count |
| ConnectRetryInterval | Connect Retry Interval |
| PoolBlockingPeriod | Pool Blocking Period |
| MultipleActiveResultSets | Multiple Active Result Sets |
| MultiSubnetFailover | Multiple Subnet Failover |
| TransparentNetworkIPResolution | Transparent Network IP Resolution |
| TrustServerCertificate | Надежный сертификат сервера |

#### <a name="sqlbulkcopy-rowscopied-property"></a>Свойство SqlBulkCopy RowsCopied

Свойство RowsCopied предоставляет доступ только для чтения к количеству строк, которые были обработаны в текущей операции с массовым копированием. Это значение не обязательно должно быть равно конечному количеству строк, добавляемых в целевую таблицу.

#### <a name="connection-open-overrides"></a>Переопределение открытия соединения

Поведение по умолчанию для SqlConnection.Open() можно переопределить, чтобы отключить 10-секундную задержку и автоматические попытки подключения, активируемые временными ошибками.

```csharp
using SqlConnection sqlConnection = new SqlConnection("Data Source=(local);Integrated Security=true;Initial Catalog=AdventureWorks;");
sqlConnection.Open(SqlConnectionOverrides.OpenWithoutRetry);
```

> [!NOTE]
> Обратите внимание, что это переопределение можно применить только к SqlConnection.Open(), а не к SqlConnection.OpenAsync().

#### <a name="username-support-for-active-directory-interactive-mode"></a>Поддержка имени пользователя для интерактивного режима Active Directory

Имя пользователя может быть указано в строке подключения при использовании режима интерактивной проверки подлинности Azure Active Directory для .NET Framework и .NET Core.

Задайте имя пользователя, используя **идентификатор пользователя** или свойство строки подключения **UID**.

```csharp
"Server=<server name>; Database=<db name>; Authentication=Active Directory Interactive; User Id=<username>;"
```

#### <a name="order-hints-for-sqlbulkcopy"></a>Указания порядка для SqlBulkCopy

Для повышения производительности операций массового копирования в таблицах с кластеризованными индексами можно предоставить указания порядка. Дополнительные сведения см. в разделе об [операциях массового копирования](sql/bulk-copy-order-hints.md).

#### <a name="sni-dependency-changes"></a>Изменения зависимостей SNI

Microsoft.Data.SqlClient (.NET Core и .NET Standard) в Windows теперь зависят от **Microsoft.Data.SqlClient.SNI.runtime** вместо **runtime.native.System.Data.SqlClient.SNI**. Новая зависимость добавляет поддержку платформы ARM в дополнение к поддерживаемым платформам ARM64, x64 и x86 в Windows.

## <a name="release-notes-for-microsoftdatasqlclient-110"></a>Заметки о выпуске для Microsoft.Data.SqlClient 1.1.0

Заметки о выпуске также доступны в репозитории GitHub: [Заметки о выпуске 1.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

### <a name="new-features-in-11"></a>Новые возможности в версии 1.1

#### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted с безопасными анклавами.

Always Encrypted доступна начиная с Microsoft SQL Server 2016. Безопасные анклавы доступны начиная с Microsoft SQL Server 2019. Чтобы использовать функцию анклава, строки подключения должны включать необходимый протокол и URL-адрес аттестации. Пример:

```csharp
"Attestation Protocol=HGS;Enclave Attestation Url=<attestation_url_for_HGS>"
```

Дополнительные сведения см. в разделе:

- [Поддержка SqlClient для Always Encrypted](sql/sqlclient-support-always-encrypted.md)
- [Руководство. Разработка приложения .NET с помощью Always Encrypted с безопасными анклавами](sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)

## <a name="release-notes-for-microsoftdatasqlclient-10"></a>Заметки о выпуске для Microsoft.Data.SqlClient 1.0

Первоначальный выпуск пространства имен Microsoft.Data.SqlClient предлагает больше функциональных возможностей по сравнению с существующим пространством имен System.Data.SqlClient.
Заметки о выпуске также доступны в репозитории GitHub: [Заметки о выпуске 1.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.0).

### <a name="new-features-in-10"></a>Новые возможности в версии 1.0

#### <a name="new-features-over-net-framework-472-systemdatasqlclient"></a>Новые возможности по сравнению с .NET Framework 4.7.2 System.Data.SqlClient

- **Классификация данных**. Доступна в Базе данных SQL Azure и Microsoft SQL Server 2019.

- **Поддержка UTF-8**. Доступна в Microsoft SQL Server 2019.

#### <a name="new-features-over-net-core-22-systemdatasqlclient"></a>Новые возможности по сравнению с .NET Core 2.2 System.Data.SqlClient

- **Классификация данных**. Доступна в Базе данных SQL Azure и Microsoft SQL Server 2019.

- **Поддержка UTF-8**. Доступна в Microsoft SQL Server 2019.

- **Проверки подлинности**. Режим проверки подлинности с помощью пароля AD DS.

### <a name="data-classification"></a>Классификация данных

Классификация данных предоставляет новый набор API-интерфейсов, которые предоставляют сведения только для чтения о конфиденциальности и классификации данных для объектов, полученных через SqlDataReader, если базовый источник поддерживает эту функцию и содержит метаданные о [конфиденциальности и классификации данных](../../relational-databases/security/sql-data-discovery-and-classification.md). См. пример приложения в разделе [Обнаружение и классификация данных в SqlClient](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

```csharp
public class SqlDataReader
{
    public Microsoft.Data.SqlClient.DataClassification.SensitivityClassification SensitivityClassification
}

namespace Microsoft.Data.SqlClient.DataClassification
{
    public class ColumnSensitivity
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.SensitivityProperty> SensitivityProperties
    }
    public class InformationType
    {
        public string Id
        public string Name
    }
    public class Label
    {
        public string Id
        public string Name
    }
    public class SensitivityClassification
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.ColumnSensitivity> ColumnSensitivities
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.InformationType> InformationTypes
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.Label> Labels
    }
    public class SensitivityProperty
    {
        public Microsoft.Data.SqlClient.DataClassification.InformationType InformationType
        public Microsoft.Data.SqlClient.DataClassification.Label Label
    }
}
```

### <a name="utf-8-support"></a>Поддержка UTF-8.

Поддержка UTF-8 не требует изменения кода приложения. Эти изменения SqlClient оптимизируют связь между клиентом и сервером, когда сервер поддерживает UTF-8, а базовым параметром сортировки столбца является UTF-8. См. раздел о UTF-8 в статье [Новые возможности SQL Server 2019](../../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted с безопасными анклавами

В целом, существующая документация, использующая System.Data.SqlClient на .NET Framework **и встроенные поставщики хранилища главных ключей столбцов**, теперь должны также работать с .NET Core.

 [Разработка с использованием постоянного шифрования с поставщиком данных .NET Framework](../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)

 [Always Encrypted: защита конфиденциальных данных и хранение ключей шифрования в хранилище сертификатов Windows](/azure/sql-database/sql-database-always-encrypted)

### <a name="authentication"></a>Аутентификация

С помощью параметра строки подключения _Аутентификация_ можно указать различные режимы проверки подлинности. Дополнительные сведения см. в [документации по SqlAuthenticationMethod](/dotnet/api/system.data.sqlclient.sqlauthenticationmethod?view=netframework-4.7.2&preserve-view=true).

> [!NOTE]
> Настраиваемые поставщики хранилища ключей, например поставщик Azure Key Vault, необходимо обновить, чтобы они поддерживали Microsoft.Data.SqlClient. Также для поддержки Microsoft.Data.SqlClient необходимо обновить поставщики анклава.
> Always Encrypted поддерживается только для целевых объектов .NET Framework и .NET Core. Функция не поддерживается для .NET Standard, поскольку в .NET Standard отсутствуют определенные зависимости шифрования.

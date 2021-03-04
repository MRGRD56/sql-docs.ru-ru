---
title: DSN ODBC и ключевые слова в строке подключения
description: На этой странице перечислены ключевые слова для строк подключения и имен DSN, а также атрибуты подключения для SQLSetConnectAttr и SQLGetConnectAttr, доступные в драйвере ODBC для SQL Server.
ms.custom: ''
ms.date: 02/04/2019
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-chojas
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: d905ac9c7f2ea02bfac04b54e247d833ae337b96
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837389"
---
# <a name="dsn-and-connection-string-keywords-and-attributes"></a>Ключевые слова и атрибуты строки подключения и имени DSN

На этой странице перечислены ключевые слова для строк подключения и имен DSN, а также атрибуты подключения для SQLSetConnectAttr и SQLGetConnectAttr, доступные в драйвере ODBC для SQL Server.

## <a name="supported-dsnconnection-string-keywords-and-connection-attributes"></a>Поддерживаемые ключевые слова для имен DSN и строк подключения, а также атрибуты подключения

В приведенной ниже таблице перечислены доступные ключевые слова и атрибуты для каждой платформы (L: Linux M: macOS; W: Windows). Чтобы получить дополнительные сведения, щелкните ключевое слово или атрибут.

| Ключевые слова строки подключения и имени DSN | Атрибут подключения | Платформа |
|-|-|-|
| [Addr](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Адрес](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [AnsiNPW](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ANSI_NPW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssansinpw) | LMW |
| [APP](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ApplicationIntent](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_APPLICATION_INTENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssapplicationintent) | LMW |
| [AttachDBFileName](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ATTACHDBFILENAME](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssattachdbfilename) | LMW |
| [Аутентификация](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | [SQL_COPT_SS_AUTHENTICATION](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | LMW |
| [AutoTranslate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRANSLATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstranslate) | LMW |
| [ColumnEncryption](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | [SQL_COPT_SS_COLUMN_ENCRYPTION](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | LMW |
| [ConnectRetryCount](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_COUNT](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [ConnectRetryInterval](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_INTERVAL](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [База данных](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_ATTR_CURRENT_CATALOG](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| [Описание](../../connect/odbc/dsn-connection-string-attribute.md#description) | | LMW |
| [Драйвер](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [DSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Encrypt](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ENCRYPT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssencrypt) | LMW |
| [Failover_Partner](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssfailoverpartner) | W |
| [FailoverPartnerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | W |
| [FileDSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [KeepAlive](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (версия 17.4+, только DSN)| | LMW |
| [KeepAliveInterval](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (версия 17.4+, только DSN) | | LMW |
| [KeystoreAuthentication](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystorePrincipalId](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystoreSecret](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [Язык](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [MARS_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MARS_ENABLED](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmarsenabled) | LMW |
| [MultiSubnetFailover](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MULTISUBNET_FAILOVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmultisubnetfailover) | LMW |
| [Net](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Network](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [PWD](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [QueryLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquery) | W |
| [QueryLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquerylog) | W |
| [QueryLogTIme](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_INTERVAL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfqueryinterval) | W |
| [QuotedId](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_QUOTED_IDENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssquotedident) | LMW |
| [Regional](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [SaveFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Server](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ServerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_SERVER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| [StatsLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdata) | W |
| [StatsLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalog) | W |
| [TransparentNetworkIPResolution](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | [SQL_COPT_SS_TNIR](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | LMW |
| [Trusted_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_INTEGRATED_SECURITY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssintegratedsecurity) | LMW |
| [TrustServerCertificate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRUST_SERVER_CERTIFICATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstrustservercertificate) | LMW |
| [UID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [UseFMTONLY](../../connect/odbc/dsn-connection-string-attribute.md#usefmtonly) | | LMW |
| [WSID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| | [SQL_ATTR_ACCESS_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ACCESS_MODE) | LMW |
| | [SQL_ATTR_ASYNC_DBC_EVENT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCALLBACK](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCONTEXT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_AUTO_IPD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_AUTOCOMMIT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_AUTOCOMMIT) | LMW |
| | [SQL_ATTR_CONNECTION_DEAD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_CONNECTION_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_DBC_INFO_TOKEN](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_LOGIN_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_LOGIN_TIMEOUT) | LMW |
| | [SQL_ATTR_METADATA_ID](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_ODBC_CURSORS](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ODBC_CURSORS) | LMW |
| | [SQL_ATTR_PACKET_SIZE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_PACKET_SIZE) | LMW |
| | [SQL_ATTR_QUIET_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_QUIET_MODE) | LMW |
| | [SQL_ATTR_RESET_CONNECTION](../../odbc/reference/develop-driver/upgrading-a-3-5-driver-to-a-3-8-driver.md#connection-pooling) <br> (SQL_COPT_SS_RESET_CONNECTION) | LMW |  
| | [SQL_ATTR_TRACE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACE) | LMW |
| | [SQL_ATTR_TRACEFILE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACEFILE) | LMW |
| | [SQL_ATTR_TRANSLATE_LIB](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_DLL) | LMW |
| | [SQL_ATTR_TRANSLATE_OPTION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_OPTION) | LMW |
| | [SQL_ATTR_TXN_ISOLATION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TXN_ISOLATION) | LMW |
| | [SQL_COPT_SS_ACCESS_TOKEN](dsn-connection-string-attribute.md#sql_copt_ss_access_token) | LMW |
| | [SQL_COPT_SS_ANSI_OEM](dsn-connection-string-attribute.md#sql_copt_ss_ansi_oem)| W |
| | [SQL_COPT_SS_AUTOBEGINTXN](dsn-connection-string-attribute.md#sql_copt_ss_autobegintxn)| LMW |
| | [SQL_COPT_SS_BCP](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbcp) | LMW |
| | [SQL_COPT_SS_BROWSE_CACHE_DATA](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) | LMW |
| | [SQL_COPT_SS_BROWSE_CONNECT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseconnect) | LMW |
| | [SQL_COPT_SS_BROWSE_SERVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseserver) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREDATA](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoredata) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREPROVIDER](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoreprovider) | LMW |
| | [SQL_COPT_SS_CLIENT_CONNECTION_ID](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) | LMW |
| | [SQL_COPT_SS_CONCAT_NULL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconcatnull) | LMW |
| | [SQL_COPT_SS_CONNECTION_DEAD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconnectiondead) | LMW |
| | [SQL_COPT_SS_ENLIST_IN_DTC](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssenlistindtc) | W |
| | [SQL_COPT_SS_ENLIST_IN_XA](dsn-connection-string-attribute.md#sql_copt_ss_enlist_in_xa) | LMW |
| | [SQL_COPT_SS_FALLBACK_CONNECT](dsn-connection-string-attribute.md#sql_copt_ss_fallback_connect) | LMW |
| | [SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_MUTUALLY_AUTHENTICATED](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_OLDPWD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssoldpwd) | LMW |
| | [SQL_COPT_SS_PERF_DATA_LOG_NOW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalognow) | W |
| | [SQL_COPT_SS_PRESERVE_CURSORS](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsspreservecursors) | LMW |
| | [SQL_COPT_SS_SPID](../../connect/odbc/dsn-connection-string-attribute.md#sql_copt_ss_spid) (версия 17.5+) | LMW |
| | [SQL_COPT_SS_TXN_ISOLATION](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstxnisolation) | LMW |
| | [SQL_COPT_SS_USER_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssuserdata) | LMW |
| | [SQL_COPT_SS_WARN_ON_CP_ERROR](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsswarnoncperror) | LMW |
| [ClientCertificate](../../connect/odbc/dsn-connection-string-attribute.md#clientcertificate) | | LMW | 
| [ClientKey](../../connect/odbc/dsn-connection-string-attribute.md#clientkey) | | LMW | 


Ниже приведены некоторые ключевые слова для строк подключения и атрибуты подключения, которые не описаны в статьях [Использование ключевых слов строки подключения с собственным клиентом SQL Server](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md), [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) и [Функция SQLSetConnectAttr](../../odbc/reference/syntax/sqlsetconnectattr-function.md).

### <a name="description"></a>Описание

Используется для описания источника данных.

### <a name="sql_copt_ss_ansi_oem"></a>SQL_COPT_SS_ANSI_OEM

Управляет преобразованием данных из ANSI в OEM. 

| Значение атрибута | Описание |
|-|-|
| SQL_AO_OFF | (По умолчанию) Преобразование не выполняется. |
| SQL_AO_ON | Преобразование выполняется. |

### <a name="sql_copt_ss_autobegintxn"></a>SQL_COPT_SS_AUTOBEGINTXN

Версия 17.6+. Когда автофиксация отключена, управляет автоматическим выполнением BEGIN TRANSACTION после ROLLBACK или COMMIT.

| Значение атрибута | Описание |
|-|-|
| SQL_AUTOBEGINTXN_ON | (По умолчанию.) BEGIN TRANSACTION выполняется автоматически после ROLLBACK или COMMIT. |
| SQL_AUTOBEGINTXN_OFF | BEGIN TRANSACTION не выполняется автоматически после ROLLBACK или COMMIT. |

### <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT

Управляет использованием резервных подключений SQL Server. Больше не поддерживается.

| Значение атрибута | Описание |
|-|-|
| SQL_FB_OFF | (По умолчанию) Резервные подключения отключены. |
| SQL_FB_ON | Резервные подключения включены. |



## <a name="new-connection-string-keywords-and-connection-attributes"></a>Новые ключевые слова для строк подключения и атрибуты подключения

###  <a name="authentication---sql_copt_ss_authentication"></a>Проверка подлинности — SQL_COPT_SS_AUTHENTICATION

Задает режим проверки подлинности для соединения с SQL Server. Дополнительные сведения см. в статье об [использовании Azure Active Directory](using-azure-active-directory.md).

| Значение ключевого слова | Значение атрибута | Описание |
|-|-|-|
| |SQL_AU_NONE|(По умолчанию) Не задано. Режим проверки подлинности определяется сочетанием других атрибутов.|
|SqlPassword|SQL_AU_PASSWORD|Проверка подлинности SQL Server (с использованием имени пользователя и пароля).|
|ActiveDirectoryIntegrated|SQL_AU_AD_INTEGRATED|Интегрированная проверка подлинности Azure Active Directory.|
|ActiveDirectoryPassword|SQL_AU_AD_PASSWORD|Проверка подлинности с использованием пароля Azure Active Directory.|
|ActiveDirectoryInteractive|SQL_AU_AD_INTERACTIVE|Интерактивная проверка подлинности Azure Active Directory.|
|ActiveDirectoryMsi|SQL_AU_AD_MSI|Проверка подлинности Azure Active Directory с использованием управляемого удостоверения. Для назначенного пользователем удостоверения в качестве идентификатора пользователя задается идентификатор объекта удостоверения пользователя. |
| |SQL_AU_RESET|Сброс. Переопределяет любой параметр имени DSN или строки подключения.|

> [!NOTE]
> При использовании ключевого слова или атрибута `Authentication` явно присвойте параметру `Encrypt` нужное значение в строке подключения, имени DSN или атрибуте подключения. Подробные сведения см. в статье [Использование ключевых слов строки подключения с собственным клиентом SQL Server](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).

### <a name="columnencryption---sql_copt_ss_column_encryption"></a>ColumnEncryption — SQL_COPT_SS_COLUMN_ENCRYPTION

Управляет прозрачным шифрованием столбцов (Always Encrypted). Дополнительные сведения см. в статье [Использование функции Always Encrypted с драйвером ODBC](using-always-encrypted-with-the-odbc-driver.md).

| Значение ключевого слова | Значение атрибута | Описание |
|-|-|-|
|Активировано|SQL_CE_ENABLED|Включает функцию Always Encrypted.|
|Выключено|SQL_CE_DISABLED|(По умолчанию) Отключает функцию Always Encrypted.|
| |SQL_CE_RESULTSETONLY|Включает только расшифровку (результатов и возвращаемых значений).|

### <a name="transparentnetworkipresolution---sql_copt_ss_tnir"></a>TransparentNetworkIPResolution — SQL_COPT_SS_TNIR

Управляет функцией "Разрешение IP-адресов прозрачной сети", которая взаимодействует с MultiSubnetFailover для ускорения повторных попыток подключения. Дополнительные сведения см. в статье [Использование разрешения IP-адресов прозрачной сети](using-transparent-network-ip-resolution.md).

| Значение ключевого слова | Значение атрибута| Описание |
|-|-|-|
|Активировано|SQL_IS_ON|(По умолчанию). Включает разрешения IP-адресов прозрачной сети.|
|Выключено|SQL_IS_OFF|Отключает разрешения IP-адресов прозрачной сети.|

### <a name="usefmtonly"></a>UseFMTONLY

Управляет использованием параметра SET FMTONLY для метаданных при подключении к SQL Server 2012 и более поздних версий.

| Значение ключевого слова | Описание |
|-|-|
|нет|(По умолчанию) Для метаданных (при их наличии) используется процедура sp_describe_first_result_set. |
|Да| Для метаданных используется параметр SET FMTONLY. |


## <a name="clientcertificate"></a>ClientCertificate

Указывает сертификат, который будет использоваться для аутентификации. Доступны следующие возможности: 

| Значение параметра | Описание |
|-|-|
| sha1:`<hash_value>` | Для поиска сертификата в Хранилище Сертификатов Windows драйвер ODBC использует хэш SHA1. |
| subject:`<subject>` | Для поиска сертификата в Хранилище Сертификатов Windows драйвер ODBC использует тему. |
| файл:`<file_location>`[, пароль:`<password>`] | Драйвер ODBC использует файл сертификата. |

Если формат сертификата — PFX и закрытый ключ внутри PFX-сертификата защищен паролем, требуется ввести пароль-ключ. Для сертификатов в PEM и форматах DER требуется атрибут ClientKey


## <a name="clientkey"></a>ClientKey

Указывает расположение файла закрытого ключа для сертификатов PEM или DER, заданных атрибутом ClientCertificate. Формат: 

| Значение параметра | Описание |
|-|-|
| файл:`<file_location>`[, пароль:`<password>`] | Указывает расположение файла закрытого ключа. |

Если файл закрытого ключа защищен паролем, необходимо ввести пароль-ключ. Если пароль содержит символы ",", после каждого из них сразу же добавляется символ ",". Например, если пароль имеет значение "a, b, c", в строке подключения содержится пароль со знаком перехода "a,, b,, c". 
    

### <a name="sql_copt_ss_access_token"></a>SQL_COPT_SS_ACCESS_TOKEN

Разрешает использование маркера доступа Azure Active Directory для проверка подлинности. Дополнительные сведения см. в статье об [использовании Azure Active Directory](using-azure-active-directory.md).

| Значение атрибута | Описание |
|-|-|
| NULL | (По умолчанию) Маркер доступа не предоставлен. |
| ACCESSTOKEN* | Указатель на маркер доступа. |

### <a name="sql_copt_ss_cekeystoredata"></a>SQL_COPT_SS_CEKEYSTOREDATA

Взаимодействует с загруженной библиотекой поставщика хранилища ключей. См. описание атрибута, управляющего прозрачным шифрованием столбцов (Always Encrypted). Этот атрибут не имеет значения по умолчанию. Дополнительные сведения см. в статье [Пользовательские поставщики хранилища ключей](custom-keystore-providers.md).

| Значение атрибута | Описание |
|-|-|
| CEKEYSTOREDATA * | Структура данных взаимодействия для библиотеки поставщика хранилища ключей. |

### <a name="sql_copt_ss_cekeystoreprovider"></a>SQL_COPT_SS_CEKEYSTOREPROVIDER

Загружает библиотеку поставщика хранилища ключей для Always Encrypted или получает имена загруженных библиотек поставщиков хранилища ключей. Дополнительные сведения см. в статье [Пользовательские поставщики хранилища ключей](custom-keystore-providers.md). Этот атрибут не имеет значения по умолчанию.

| Значение атрибута | Описание |
|-|-|
| char * | Путь к библиотеке поставщика хранилища ключей |

### <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA

Чтобы включить транзакции XA с совместимым с XA обработчиком транзакций, приложение должно вызвать функцию **SQLSetConnectAttr** с атрибутом SQL_COPT_SS_ENLIST_IN_XA и указателем на объект `XACALLPARAM`. Этот параметр поддерживается в Windows (версии 17.3 и более поздних), Linux и macOS.
```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, param, SQL_IS_POINTER);  // XACALLPARAM *param
``` 
 Чтобы связать транзакцию XA только с подключением ODBC, при вызове функции **SQLSetConnectAttr** укажите значение TRUE или FALSE с атрибутом SQL_COPT_SS_ENLIST_IN_XA вместо указателя. Это действует только в Windows. Таким образом нельзя настроить операции XA через клиентское приложение. 
 ```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, (SQLPOINTER)TRUE, 0);
``` 

|Значение|Описание|Платформы|  
|-----------|-----------------|-----------------|  
|Объект XACALLPARAM *|Указатель на объект `XACALLPARAM`.|Windows, Linux и macOS|
|TRUE|Связывает транзакцию XA с подключением ODBC. Все связанные действия базы данных будут выполняться под защитой XA-транзакции.|Windows|  
|FALSE|Отменяет связь транзакции с подключением ODBC.|Windows|

 Дополнительные сведения о транзакциях XA см. в статье [Использование транзакций XA](../../connect/odbc/use-xa-with-dtc.md).

### <a name="sql_copt_ss_spid"></a>SQL_COPT_SS_SPID

Возвращает идентификатор серверного процесса соединения. Он эквивалентен переменной T-SQL [@@SPID](../../t-sql/functions/spid-transact-sql.md), за исключением того, что она не требует дополнительного кругового пути на сервер.

| Значение атрибута | Описание |
|-|-|
| DWORD | SPID |

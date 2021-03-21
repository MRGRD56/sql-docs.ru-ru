---
title: Использование ключевых слов строки подключения с OLE DB Driver
description: Некоторые API Microsoft OLE DB Driver for SQL Server используют строки подключения, которые представляют собой список ключевых слов и значений, определяющих конкретные атрибуты соединения.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
f1_keywords:
- sql13.swb.connecttoserver.options.registeredservers.f1
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], connection string keywords
- MSOLEDBSQL, connection string keywords
- connection strings [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, connection string keywords
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c5ddba7e6b2ac0faef93ff544dbb5f23f3fb5d15
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751204"
---
# <a name="using-connection-string-keywords-with-ole-db-driver-for-sql-server"></a>Использование ключевых слов строки подключения с драйвером OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Некоторые API OLE DB Driver for SQL Server используют строки подключения, чтобы указать атрибуты подключения. Строки подключения — это список ключевых слов и связанных значений, причем каждое ключевое слово определяет определенный атрибут соединения.  
  
> [!NOTE]
> Драйвер OLE DB для SQL Server поддерживает неоднозначность строк соединения для обеспечения обратной совместимости (например, некоторые ключевые слова могут быть указаны несколько раз, и конфликты ключевых слов могут разрешаться на основании их положения или приоритета). В следующих версиях OLE DB Driver for SQL Server неоднозначность в строках подключения может стать недопустимой. При изменении приложения для работы с драйвером OLE DB для SQL Server необходимо предусмотреть устранение любых зависимостей от неоднозначности строки соединения.  
  
 В следующих разделах описываются ключевые слова, которые могут использоваться с OLE DB Driver for SQL Server и объектами данных ActiveX (ADO) при использовании OLE DB Driver for SQL Server в качестве поставщика данных.  

## <a name="ole-db-driver-connection-string-keywords"></a>Ключевые слова в строке подключения драйвера OLE DB  

 Приложения OLE DB могут инициализировать объекты источника данных двумя способами:  
  
-   **IDBInitialize::Initialize**  
  
-   **IDataInitialize::GetDataSource**  
  
 В первом случае строка поставщика может использоваться для инициализации свойств соединения посредством установки свойства DBPROP_INIT_PROVIDERSTRING в наборе свойств DBPROPSET_DBINIT. Во втором случае строка инициализации может быть передана методу **IDataInitialize::GetDataSource** для инициализации свойств соединения. При использовании каждого из этих методов инициализируются одни и те же свойства соединения OLE DB, однако, для этого применяются разные наборы ключевых слов. Набор ключевых слов, используемых методом **IDataInitialize::GetDataSource**, представляет собой по меньшей мере описание свойств, входящих в группу свойств инициализации.  
  
 Для любого параметра, указанного в строке поставщика, который имеет соответствующее свойство OLE DB, устанавливаемое в некоторое значение по умолчанию или явным образом установленное в некоторое значение, значение свойства OLE DB переопределяет значение, указанное в строке поставщика.  
  
 Логические свойства, установленные в строках поставщика через значения DBPROP_INIT_PROVIDERSTRING, устанавливаются с помощью значений `yes` и `no`. Логические свойства, установленные в строках инициализации с помощью метода **IDataInitialize::GetDataSource**, устанавливаются с использованием значений `true` и `false`.  
  
 Приложения, использующие метод **IDataInitialize::GetDataSource**, также могут использовать ключевые слова, используемые методом **IDBInitialize::Initialize**, но только для свойств, у которых отсутствуют значения по умолчанию. Если приложение одновременно использует в строке инициализации ключевые слова методов **IDataInitialize::GetDataSource** и **IDBInitialize::Initialize**, используется ключевое слово метода **IDataInitialize::GetDataSource**. Не рекомендуется использовать в приложениях ключевые слова **IDBInitialize::Initialize** в строках подключения **IDataInitialize:GetDataSource**, так как это поведение может не поддерживаться в будущих версиях.  
  
> [!NOTE]  
>  Строка подключения, передаваемая в метод **IDataInitialize::GetDataSource**, преобразуется в свойства, которые применяются методом **IDBProperties::SetProperties**. Если службы компонентов обнаруживают описание свойства в **IDBProperties::GetPropertyInfo**, это свойство будет применяться в качестве изолированного. В противном случае оно будет применяться через свойство DBPROP_PROVIDERSTRING. Например, если указать строку подключения **Data Source=server1;Server=server2**, ключевое слово **Data Source** будет задано в качестве свойства, но **Server** перейдет в строку поставщика.  
  
 Если указать несколько экземпляров одного свойства поставщика, то будет использоваться первое значение первого свойства.  

## <a name="using-idbinitializeinitialize"></a>Использование IDBInitialize::Initialize

 Строки подключения, используемые в приложениях OLE DB, в которых используется DBPROP_INIT_PROVIDERSTRING и **IDBInitialize::Initialize**, имеют следующий синтаксис.  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=[{]attribute-value[}]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 Значения атрибутов можно (и рекомендуется) дополнительно заключить в фигурные скобки. Это позволяет избежать проблем, если значения атрибутов содержат символы, отличные от буквенно-цифровых. Предполагается, что первая закрывающая фигурная скобка в значении служит признаком конца значения, поэтому само значение не может содержать символы закрывающей фигурной скобки.  
  
 Пробельный символ после знака равенства (`=`) в строке подключения будет считаться литералом, даже если значение заключено в кавычки.  
  
 В следующей таблице рассматриваются ключевые слова, которые можно использовать со свойством DBPROP_INIT_PROVIDERSTRING.  
  
|Ключевое слово|Свойство инициализации|Описание|  
|-------------|-----------------------------|-----------------|  
|**Addr**|SSPROP_INIT_NETWORKADDRESS|Синоним для **Address**.|  
|**Адрес**|SSPROP_INIT_NETWORKADDRESS|Сетевой адрес сервера, на котором выполняется экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **Address** обычно является сетевым именем сервера, но может быть и другим именем, например именем канала, IP-адресом или портом TCP/IP с адресом сокета.<br /><br /> Если указывается IP-адрес, убедитесь, что в диспетчере конфигурации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] включен протокол TCP/IP или протокол именованных каналов.<br /><br /> При использовании OLE DB Driver for SQL Server значение слова **Address** будет приоритетнее значения, передаваемого в ключевое слово **Server** в строках подключения. Также следует отметить, что строка `Address=;` соединяется с сервером, указанным в ключевом слове **Server**, а при использовании ключевых слов `Address= ;, Address=.;`, `Address=localhost;` и `Address=(local);`устанавливается соединение с локальным сервером.<br /><br /> Далее представлен полный синтаксис ключевого слова **Address**.<br /><br /> [_protocol_ **:** ]_Address_[ **,** _port &#124;\pipe\pipename_]<br /><br /> Параметр _протокол_ может иметь значение **tcp** (TCP/IP), **lpc** (общая память) или **np** (именованные каналы). Дополнительные сведения см. в статье [Настройка клиентских протоколов](../../../database-engine/configure-windows/configure-client-protocols.md).<br /><br /> Если не указан ни _протокол_, ни ключевое слово **Network**, OLE DB Driver for SQL Server будет использовать порядок протоколов, указанный в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager.<br /><br /> *port* — это порт указанного сервера, к которому выполняется подключение. По умолчанию [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использует порт 1433.|   
|**APP**|SSPROP_INIT_APPNAME|Строка, идентифицирующая приложение.|  
|**ApplicationIntent**|SSPROP_INIT_APPLICATIONINTENT|Объявляет тип рабочей нагрузки приложения при соединении с сервером. Возможные значения: `ReadOnly` и `ReadWrite`.<br /><br /> Значение по умолчанию — `ReadWrite`. Дополнительные сведения о поддержке OLE DB Driver for SQL Server для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в [этой статье](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**AttachDBFileName**|SSPROP_INIT_FILENAME|Имя первичного файла присоединяемой базы данных (включает полный путь). Чтобы использовать ключевое слово **AttachDBFileName**, необходимо также указать имя базы данных с помощью ключевого слова Database в строке поставщика. Если база данных уже присоединена, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не присоединяет ее повторно, а использует для соединения присоединенную базу данных по умолчанию.|  
|**Authentication**<a href="#table1_1"><sup id="table1_authmode">**1**</sup></a>|SSPROP_AUTH_MODE|Указывает используемый метод проверки подлинности SQL или Active Directory. Допустимые значения:<br/><ul><li>`(not set)`: режим проверки подлинности определяется другими ключевыми словами.</li><li>`ActiveDirectoryPassword:` проверка подлинности по идентификатору и паролю пользователя с использованием идентификатора Azure Active Directory.</li><li>`ActiveDirectoryIntegrated:` встроенная проверка подлинности с помощью идентификатора Azure Active Directory.</li><br/>**ПРИМЕЧАНИЕ.** Ключевое слово `ActiveDirectoryIntegrated` можно также использовать для проверки подлинности Windows в SQL Server. Оно заменяет ключевые слова проверки подлинности `Integrated Security` или `Trusted_Connection`. **Рекомендуется** присвоить ключевому слову `Authentication` (или его соответствующему свойству) значение `ActiveDirectoryIntegrated` в приложениях, использующих ключевые слова `Integrated Security` или `Trusted_Connection` (или их соответствующие свойства). Это позволит активировать новое поведение проверки сертификата и шифрования.<br/><br/><li>`ActiveDirectoryInteractive:` встроенная проверка подлинности с использованием идентификатора Azure Active Directory. Этот метод поддерживает многофакторную проверку подлинности Azure (MFA). </li><li>`ActiveDirectoryMSI:` проверка подлинности [управляемого удостоверения (MSI)](/azure/active-directory/managed-identities-azure-resources/overview). Для назначенного пользователем удостоверения в качестве идентификатора пользователя задается идентификатор объекта удостоверения пользователя.</li><li>`ActiveDirectoryServicePrincipal:` проверка подлинности с использованием субъекта-службы Azure Active Directory. В качестве идентификатора пользователя нужно указать идентификатор приложения (клиента). В качестве пароля нужно указать секрет приложения (клиента).</li><li>`SqlPassword:` проверка подлинности с помощью идентификатора пользователя и пароля.</li><br/>**ПРИМЕЧАНИЕ.** Мы **рекомендуем** присвоить ключевому слову `Authentication` (или его соответствующему свойству) значение `SqlPassword` в приложениях, использующих проверку подлинности `SQL Server`. Это позволит активировать [новое поведение проверки сертификата и шифрования](../features/using-azure-active-directory.md#encryption-and-certificate-validation).</ul>|
|**Автоматическое преобразование**|SSPROP_INIT_AUTOTRANSLATE|Синоним для **AutoTranslate**.|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|Настраивает перевод символов OEM/ANSI. Распознаются значения `yes` и `no`.|  
|**База данных**|DBPROP_INIT_CATALOG|Имя базы данных.|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|Указывает используемый режим обработки типов данных. Распознаются значения `0` (для типов данных поставщика) и `80` (для типов данных SQL Server 2000).|  
|**Encrypt**<a href="#table1_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|Указывает, необходимо ли шифрование данных перед их отправкой по сети. Возможные значения: `yes` и `no`. Значение по умолчанию — `no`.|  
|**FailoverPartner**|SSPROP_INIT_FAILOVERPARTNER|Имя сервера отработки отказа, используемого для зеркального отображения базы данных.|  
|**FailoverPartnerSPN**|SSPROP_INIT_FAILOVERPARTNERSPN|Имя участника-службы для партнера по обеспечению отработки отказа. Значение по умолчанию — пустая строка. Из-за этой пустой строки OLE DB Driver for SQL Server использует имя субъекта-службы по умолчанию, создаваемое поставщиком.|  
|**Язык**|SSPROPT_INIT_CURRENTLANGUAGE|Язык [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**MarsConn**|SSPROP_INIT_MARSCONNECTION|Включает или отключает режим MARS для соединения для сервера версии [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] или более поздней версии. Возможные значения: `yes` и `no`. Значение по умолчанию — `no`.|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|Всегда указывайте **MultiSubnetFailover=Yes** при соединении с прослушивателем группы доступности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или экземпляром отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover=Yes** настраивает драйвер OLE DB для SQL Server для ускоренного обнаружения активного (в данный момент) сервера и подключения к нему. Возможные значения: `Yes` и `No`. Значение по умолчанию — `No`. Пример:<br /><br /> `MultiSubnetFailover=Yes`<br /><br /> Дополнительные сведения о поддержке OLE DB Driver for SQL Server для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в [этой статье](../../oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Net**|SSPROP_INIT_NETWORKLIBRARY|Синоним для **Network**.|  
|**Network**|SSPROP_INIT_NETWORKLIBRARY|Сетевая библиотека, используемая для установления соединения с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в организации.|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|Синоним для **Network**.|  
|**PacketSize**|SSPROP_INIT_PACKETSIZE|Размер пакета потока табличных данных (TDS). Значением по умолчанию является 0 (фактическое значение будет определяться сервером).|  
|**PersistSensitive**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|Принимает в качестве значений строки `yes` и `no`. Если указано значение `no`, объекту источника данных не разрешено сохранять конфиденциальные сведения проверки подлинности.|  
|**PWD**|DBPROP_AUTH_PASSWORD|Пароль имени входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Server**|DBPROP_INIT_DATASOURCE|Имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Значением должно быть имя сервера в сети, IP-адрес или имя псевдонима диспетчера конфигурации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Если имя не задано, то производится соединение с экземпляром по умолчанию на локальном компьютере.<br /><br /> Ключевое слово **Address** переопределяет ключевое слово **Server**.<br /><br /> Чтобы подключиться к экземпляру по умолчанию на локальном сервере, можно указать одно из следующих ключевых слов:<br /><br /> **Server=;**<br /><br /> **Server=.;**<br /><br /> **Server=(local);**<br /><br /> **Server=(local);**<br /><br /> **Server=(localhost);**<br /><br /> **Server=(localdb)\\** *instancename* **;**<br /><br /> Дополнительные сведения о поддержке LocalDB см. в [этой статье](../../oledb/features/oledb-driver-for-sql-server-support-for-localdb.md).<br /><br /> Чтобы указать именованный экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], добавьте **\\** _InstanceName_.<br /><br /> Если сервер не указан, устанавливается соединение с экземпляром по умолчанию на локальном компьютере.<br /><br /> Если указывается IP-адрес, убедитесь, что в диспетчере конфигурации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] включен протокол TCP/IP или протокол именованных каналов.<br /><br /> Далее представлен полный синтаксис ключевого слова **Server**.<br /><br /> **Server=** [_protocol_ **:** ]*Server*[ **,** _port_]<br /><br /> Параметр _протокол_ может иметь значение **tcp** (TCP/IP), **lpc** (общая память) или **np** (именованные каналы).<br /><br /> Ниже приводится пример указания именованного канала:<br /><br /> `np:\\.\pipe\MSSQL$MYINST01\sql\query`<br /><br /> Эта строка указывает протокол именованных каналов (`np`), именованный канал на локальном компьютере (`\\.\pipe`), имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (`MSSQL$MYINST01`) и имя по умолчанию для именованного канала (`sql/query`).<br /><br /> Если не указан ни *протокол*, ни ключевое слово **Network**, OLE DB Driver for SQL Server будет использовать порядок протоколов, указанный в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager.<br /><br /> *port* — это порт указанного сервера, к которому выполняется подключение. По умолчанию [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использует порт 1433.<br /><br /> При использовании OLE DB Driver for SQL Server пробелы в начале значения, передаваемого в ключевое слово **Server** строки подключения, не учитываются.|   
|**ServerSPN**|SSPROP_INIT_SERVERSPN|Имя участника-службы для сервера. Значение по умолчанию — пустая строка. Из-за этой пустой строки OLE DB Driver for SQL Server использует имя субъекта-службы по умолчанию, создаваемое поставщиком.|  
|**Timeout**|DBPROP_INIT_TIMEOUT|Время (в секундах), в течение которого ожидается завершение инициализации источника данных.|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|Влияет на последовательность подключений, когда первый разрешенный IP-адрес имени узла не отвечает и имеется несколько IP-адресов, связанных с именем этого узла. TNIR взаимодействует с MultiSubnetFailover и поддерживает различные варианты последовательности подключений. Возможные значения: `Yes` и `No`. Значение по умолчанию — `Yes`. Дополнительные сведения см. в статье [Использование разрешения IP-адресов прозрачной сети](../../oledb/features/using-transparent-network-ip-resolution.md).|  
|**Trusted_Connection**|DBPROP_AUTH_INTEGRATED|Значение `yes` предписывает OLE DB Driver for SQL Server использовать проверку подлинности Windows для проверки имени входа. В противном случае для проверки имени входа драйвер OLE DB для SQL Server будет использовать имя пользователя и пароль [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]; необходимо также указать ключевые слова UID и PWD.|  
|**TrustServerCertificate**<a href="#table1_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|Принимает в качестве значений строки `yes` и `no`. По умолчанию имеет значение `no`, которое указывает, что сертификат сервера будет проверяться.|  
|**UID**|DBPROP_AUTH_USERID|Имя входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**UseFMTONLY**|SSPROP_INIT_USEFMTONLY|Определяет способ получения метаданных при подключении к [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] и более новым версиям. Возможные значения: `yes` и `no`. Значение по умолчанию — `no`.<br /><br />По умолчанию OLE DB Driver for SQL Server использует хранимые процедуры [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) и [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) для извлечения метаданных. Эти хранимые процедуры имеют некоторые ограничения (например, их выполнение при обработке временных таблиц закончится сбоем). Если для параметра **UseFMTONLY** установлено значение `yes`, драйвер должен использовать [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) для извлечения метаданных.|  
|**UseProcForPrepare**|SSPROP_INIT_USEPROCFORPREP|Это ключевое слово является устаревшим, и OLE DB Driver for SQL Server не обрабатывает его значение.|  
|**WSID**|SSPROP_INIT_WSID|Идентификатор рабочей станции.|  
  
<b id="table1_1">[1].</b> Для повышения безопасности поведение шифрования и проверки сертификатов изменяется при использовании свойств инициализации маркера доступа или проверки подлинности или их соответствующих ключевых слов строки подключения. Дополнительные сведения см. в разделе [Шифрование соединений и проверка сертификатов](../features/using-azure-active-directory.md#encryption-and-certificate-validation).

## <a name="using-idatainitializegetdatasource"></a>Использование IDataInitialize::GetDataSource

 Строки подключения, используемые приложениями OLE DB, в которых используется **IDataInitialize::GetDataSource**, имеют следующий синтаксис.  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=[quote]attribute-value[quote]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 - `quote ::= " | '`  
  
 Использование свойства должно соответствовать синтаксическим правилам, действующим в его области. Например, в **WSID** вместо кавычек используются фигурные скобки ( **{}** ), а в **Application Name** используются апострофы ( **'** ) или двойные кавычки ( **"** ). Заключать в кавычки можно только строковые свойства. Заключение в кавычки целочисленного или перечисляемого свойства приведет к ошибке `Connection String does not conform to OLE DB specification`.  
  
 Значения атрибутов можно при желании заключать в одинарные или двойные кавычки, а фактически это даже рекомендуется. Это позволяет избежать проблем, если значения содержат символы, отличные от буквенно-цифровых. Используемый символ кавычек может также использоваться в значении, если оно заключено в двойные кавычки.  
  
 Пробельный символ после знака равенства (=) в строке подключения будет считаться литералом, даже если значение заключено в кавычки.  
  
 Если строка подключения содержит несколько свойств из тех, которые приведены в следующей таблице, то будет использоваться значение последнего свойства.  
  
 В следующей таблице рассматриваются ключевые слова, которые можно использовать с **IDataInitialize::GetDataSource**.  
  
|Ключевое слово|Свойство инициализации|Описание|  
|-------------|-----------------------------|-----------------|  
|**Маркер доступа**<a href="#table2_1"><sup id="table2_accesstoken">**1**</sup></a>|SSPROP_AUTH_ACCESS_TOKEN|Маркер доступа, который используется для выполнения проверки подлинности в Azure Active Directory. <br/><br/>**ПРИМЕЧАНИЕ.** При указании этого ключевого слова, а также ключевых слов строк подключения `UID`, `PWD`, `Trusted_Connection` и `Authentication` или их соответствующих свойств или ключевых слов возникает ошибка.|
|**Имя приложения**|SSPROP_INIT_APPNAME|Строка, идентифицирующая приложение.|  
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|Объявляет тип рабочей нагрузки приложения при соединении с сервером. Возможные значения: `ReadOnly` и `ReadWrite`.<br /><br /> Значение по умолчанию — `ReadWrite`. Дополнительные сведения о поддержке OLE DB Driver for SQL Server для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в [этой статье](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Authentication**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_AUTH_MODE|Указывает используемый метод проверки подлинности SQL или Active Directory. Допустимые значения:<br/><ul><li>`(not set)`: режим проверки подлинности определяется другими ключевыми словами.</li><li>`ActiveDirectoryPassword:` проверка подлинности по идентификатору и паролю пользователя с использованием идентификатора Azure Active Directory.</li><li>`ActiveDirectoryIntegrated:` встроенная проверка подлинности с помощью идентификатора Azure Active Directory.</li><br/>**ПРИМЕЧАНИЕ.** Ключевое слово `ActiveDirectoryIntegrated` можно также использовать для проверки подлинности Windows в SQL Server. Оно заменяет ключевые слова проверки подлинности `Integrated Security` или `Trusted_Connection`. **Рекомендуется** присвоить ключевому слову `Authentication` (или его соответствующему свойству) значение `ActiveDirectoryIntegrated` в приложениях, использующих ключевые слова `Integrated Security` или `Trusted_Connection` (или их соответствующие свойства). Это позволит активировать новое поведение проверки сертификата и шифрования.<br/><br/><li>`ActiveDirectoryInteractive:` встроенная проверка подлинности с использованием идентификатора Azure Active Directory. Этот метод поддерживает многофакторную проверку подлинности Azure (MFA). </li><li>`ActiveDirectoryMSI:` проверка подлинности [управляемого удостоверения (MSI)](/azure/active-directory/managed-identities-azure-resources/overview). Для назначенного пользователем удостоверения в качестве идентификатора пользователя задается идентификатор объекта удостоверения пользователя.</li><li>`ActiveDirectoryServicePrincipal:` проверка подлинности с использованием субъекта-службы Azure Active Directory. В качестве идентификатора пользователя нужно указать идентификатор приложения (клиента). В качестве пароля нужно указать секрет приложения (клиента).</li><li>`SqlPassword:` проверка подлинности с помощью идентификатора пользователя и пароля.</li><br/>**ПРИМЕЧАНИЕ.** Мы **рекомендуем** присвоить ключевому слову `Authentication` (или его соответствующему свойству) значение `SqlPassword` в приложениях, использующих проверку подлинности `SQL Server`. Это позволит активировать [новое поведение проверки сертификата и шифрования](../features/using-azure-active-directory.md#encryption-and-certificate-validation).</ul>|
|**Автоматическое преобразование**|SSPROP_INIT_AUTOTRANSLATE|Синоним для **AutoTranslate**.|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|Настраивает перевод символов OEM/ANSI. Распознаются значения `true` и `false`.|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|Время (в секундах), в течение которого ожидается завершение инициализации источника данных.|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|Язык [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Источник данных**|DBPROP_INIT_DATASOURCE|Имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в организации.<br /><br /> Если имя не задано, то производится соединение с экземпляром по умолчанию на локальном компьютере.<br /><br /> Дополнительные сведения о допустимом синтаксисе адресов см. в описании ключевого слова **Server** в этом разделе.|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|Указывает используемый режим обработки типов данных. Распознаются значения `0` (для типов данных поставщика) и `80` (для типов данных [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]).|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|Имя сервера отработки отказа, используемого для зеркального отображения базы данных.|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|Имя участника-службы для партнера по обеспечению отработки отказа. Значение по умолчанию — пустая строка. Из-за этой пустой строки OLE DB Driver for SQL Server использует имя субъекта-службы по умолчанию, создаваемое поставщиком.|  
|**Исходный каталог**|DBPROP_INIT_CATALOG|Имя базы данных.|  
|**Исходное имя файла**|SSPROP_INIT_FILENAME|Имя первичного файла присоединяемой базы данных (включает полный путь). Чтобы использовать ключевое слово **AttachDBFileName**, необходимо также указать имя базы данных с помощью ключевого слова DATABASE в строке поставщика. Если база данных уже присоединена, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не присоединяет ее повторно, а использует для соединения присоединенную базу данных по умолчанию.|  
|**Встроенные функции безопасности**|DBPROP_AUTH_INTEGRATED|Принимает значение `SSPI` для проверки подлинности Windows.|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|Включает или отключает режим MARS для соединения. Распознаются значения `true` и `false`. Значение по умолчанию — `false`.|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|Всегда указывайте **MultiSubnetFailover=True** при соединении с прослушивателем группы доступности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или экземпляром отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover=True** настраивает драйвер OLE DB для SQL Server для ускоренного обнаружения активного (в данный момент) сервера и подключения к нему. Возможные значения: `True` и `False`. Значение по умолчанию — `False`. Пример:<br /><br /> `MultiSubnetFailover=True`<br /><br /> Дополнительные сведения о поддержке OLE DB Driver for SQL Server для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в [этой статье](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|Сетевой адрес экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в организации.<br /><br /> Дополнительные сведения о допустимом синтаксисе адресов см. в описании ключевого слова **Address** в этом разделе.|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|Сетевая библиотека, используемая для установления соединения с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в организации.|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|Размер пакета потока табличных данных (TDS). Значением по умолчанию является 0 (фактическое значение будет определяться сервером).|  
|**Пароль**|DBPROP_AUTH_PASSWORD|Пароль имени входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|Принимает в качестве значений строки `true` и `false`. Если используется значение `false`, объекту источника данных запрещено хранить конфиденциальные данные проверки подлинности.|  
|**Поставщик**||Для OLE DB Driver for SQL Server необходимо указать MSOLEDBSQL.|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|Имя участника-службы для сервера. Значение по умолчанию — пустая строка. Из-за этой пустой строки OLE DB Driver for SQL Server использует имя субъекта-службы по умолчанию, создаваемое поставщиком.|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|Влияет на последовательность подключений, когда первый разрешенный IP-адрес имени узла не отвечает и имеется несколько IP-адресов, связанных с именем этого узла. TNIR взаимодействует с MultiSubnetFailover и поддерживает различные варианты последовательности подключений. Возможные значения: `True` и `False`. Значение по умолчанию — `True`. Дополнительные сведения см. в статье [Использование разрешения IP-адресов прозрачной сети](../../oledb/features/using-transparent-network-ip-resolution.md).|  
|**Надежный сертификат сервера**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|Принимает в качестве значений строки `true` и `false`. По умолчанию имеет значение `false`, которое указывает, что сертификат сервера будет проверяться.|  
|**Использование шифрования данных**<a href="#table2_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|Указывает, необходимо ли шифрование данных перед их отправкой по сети. Возможные значения: `true` и `false`. Значение по умолчанию — `false`.|  
|**Use FMTONLY**|SSPROP_INIT_USEFMTONLY|Определяет способ получения метаданных при подключении к [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] и более новым версиям. Возможные значения: `true` и `false`. Значение по умолчанию — `false`.<br /><br />По умолчанию OLE DB Driver for SQL Server использует хранимые процедуры [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) и [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) для извлечения метаданных. Эти хранимые процедуры имеют некоторые ограничения (например, их выполнение при обработке временных таблиц закончится сбоем). Если для параметра **UseFMTONLY** установлено значение `true`, драйвер должен использовать [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) для извлечения метаданных.|  
|**Идентификатор пользователя**|DBPROP_AUTH_USERID|Имя входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Workstation ID**|SSPROP_INIT_WSID|Идентификатор рабочей станции.|  
  
<b id="table2_1">[1].</b> Для повышения безопасности поведение шифрования и проверки сертификатов изменяется при использовании свойств инициализации маркера доступа и проверки подлинности или их соответствующих ключевых слов строки подключения. Дополнительные сведения см. в разделе о [шифровании и проверке сертификатов](../features/using-azure-active-directory.md#encryption-and-certificate-validation).

 > [!NOTE]
 > В строке подключения свойство `Old Password` устанавливает значение свойства SSPROP_AUTH_OLD_PASSWORD, являющееся текущим паролем (возможно, с истекшим сроком действия), недоступным через свойство строки поставщика.  
  
## <a name="activex-data-objects-ado-connection-string-keywords"></a>Ключевые слова строки подключения объектов данных ActiveX (ADO)  

 Приложения ADO устанавливают свойство **ConnectionString** объектов **ADODBConnection** или указывают строку подключения как параметр метода **Open** объектов **ADODBConnection**.  
  
 В приложениях ADO также могут применяться ключевые слова, используемые методом OLE DB **IDBInitialize::Initialize**, но только для свойств, у которых отсутствует значение по умолчанию. Если в строке инициализации приложения содержатся ключевые слова ADO и ключевые слова **IDBInitialize::Initialize**, используется параметр ключевого слова ADO. Рекомендуется, чтобы приложение использовало для строк соединения только ключевые слова ADO.  
  
 Строки подключения, используемые ADO, имеют следующий синтаксис:  
  
 - `connection-string ::= empty-string[;] | attribute[;] | attribute; connection-string`  
  
 - `empty-string ::=`  
  
 - `attribute ::= attribute-keyword=["]attribute-value["]`  
  
 - `attribute-value ::= character-string`  
  
 - `attribute-keyword ::= identifier`  
  
 Значения атрибутов можно (и рекомендуется) дополнительно заключить в двойные кавычки. Это позволяет избежать проблем, если значения содержат символы, отличные от буквенно-цифровых. Значения атрибутов не могут содержать двойные кавычки.  
  
 В следующей таблице описываются ключевые слова, которые можно использовать со строками соединения ADO.  
  
|Ключевое слово|Свойство инициализации|Описание|  
|-------------|-----------------------------|-----------------|  
|**Маркер доступа**<a href="#table3_1"><sup id="table3_accesstoken">**1**</sup></a>|SSPROP_AUTH_ACCESS_TOKEN|Маркер доступа, который используется для выполнения проверки подлинности в Azure Active Directory.<br/><br/>**ПРИМЕЧАНИЕ.** При указании этого ключевого слова, а также ключевых слов строк подключения `UID`, `PWD`, `Trusted_Connection` и `Authentication` или их соответствующих свойств или ключевых слов возникает ошибка.|
|**Application Intent**|SSPROP_INIT_APPLICATIONINTENT|Объявляет тип рабочей нагрузки приложения при соединении с сервером. Возможные значения: `ReadOnly` и `ReadWrite`.<br /><br /> Значение по умолчанию — `ReadWrite`. Дополнительные сведения о поддержке OLE DB Driver for SQL Server для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в [этой статье](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Имя приложения**|SSPROP_INIT_APPNAME|Строка, идентифицирующая приложение.|  
|**Authentication**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_AUTH_MODE|Указывает используемый метод проверки подлинности SQL или Active Directory. Допустимые значения:<br/><ul><li>`(not set)`: режим проверки подлинности определяется другими ключевыми словами.</li><li>`ActiveDirectoryPassword:` проверка подлинности по идентификатору и паролю пользователя с использованием идентификатора Azure Active Directory.</li><li>`ActiveDirectoryIntegrated:` встроенная проверка подлинности с помощью идентификатора Azure Active Directory.</li><br/>**ПРИМЕЧАНИЕ.** Ключевое слово `ActiveDirectoryIntegrated` можно также использовать для проверки подлинности Windows в SQL Server. Оно заменяет ключевые слова проверки подлинности `Integrated Security` или `Trusted_Connection`. **Рекомендуется** присвоить ключевому слову `Authentication` (или его соответствующему свойству) значение `ActiveDirectoryIntegrated` в приложениях, использующих ключевые слова `Integrated Security` или `Trusted_Connection` (или их соответствующие свойства). Это позволит активировать новое поведение проверки сертификата и шифрования.<br/><br/><li>`ActiveDirectoryInteractive:` встроенная проверка подлинности с использованием идентификатора Azure Active Directory. Этот метод поддерживает многофакторную проверку подлинности Azure (MFA). </li><li>`ActiveDirectoryMSI:` проверка подлинности [управляемого удостоверения (MSI)](/azure/active-directory/managed-identities-azure-resources/overview). Для назначенного пользователем удостоверения в качестве идентификатора пользователя задается идентификатор объекта удостоверения пользователя.</li><li>`ActiveDirectoryServicePrincipal:` проверка подлинности с использованием субъекта-службы Azure Active Directory. В качестве идентификатора пользователя нужно указать идентификатор приложения (клиента). В качестве пароля нужно указать секрет приложения (клиента).</li><li>`SqlPassword:` проверка подлинности с помощью идентификатора пользователя и пароля.</li><br/>**ПРИМЕЧАНИЕ.** Мы **рекомендуем** присвоить ключевому слову `Authentication` (или его соответствующему свойству) значение `SqlPassword` в приложениях, использующих проверку подлинности `SQL Server`. Это позволит активировать [новое поведение проверки сертификата и шифрования](../features/using-azure-active-directory.md#encryption-and-certificate-validation).</ul>|
|**Автоматическое преобразование**|SSPROP_INIT_AUTOTRANSLATE|Синоним для **AutoTranslate**.|  
|**AutoTranslate**|SSPROP_INIT_AUTOTRANSLATE|Настраивает перевод символов OEM/ANSI. Распознаются значения `true` и `false`.|  
|**Connect Timeout**|DBPROP_INIT_TIMEOUT|Время (в секундах), в течение которого ожидается завершение инициализации источника данных.|  
|**Current Language**|SSPROPT_INIT_CURRENTLANGUAGE|Язык [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Источник данных**|DBPROP_INIT_DATASOURCE|Имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в организации.<br /><br /> Если имя не задано, то производится соединение с экземпляром по умолчанию на локальном компьютере.<br /><br /> Дополнительные сведения о допустимом синтаксисе адресов см. в описании ключевого слова **Server** в этом разделе.|  
|**DataTypeCompatibility**|SSPROP_INIT_DATATYPECOMPATIBILITY|Задает используемый режим обработки типов данных. Распознаются значения `0` (для типов данных поставщика) и `80` (для типов данных SQL Server 2000).|  
|**Failover Partner**|SSPROP_INIT_FAILOVERPARTNER|Имя сервера отработки отказа, используемого для зеркального отображения базы данных.|  
|**Failover Partner SPN**|SSPROP_INIT_FAILOVERPARTNERSPN|Имя участника-службы для партнера по обеспечению отработки отказа. Значение по умолчанию — пустая строка. Из-за этой пустой строки OLE DB Driver for SQL Server использует имя субъекта-службы по умолчанию, создаваемое поставщиком.|  
|**Исходный каталог**|DBPROP_INIT_CATALOG|Имя базы данных.|  
|**Исходное имя файла**|SSPROP_INIT_FILENAME|Имя первичного файла присоединяемой базы данных (включает полный путь). Чтобы использовать ключевое слово **AttachDBFileName**, необходимо также указать имя базы данных с помощью ключевого слова **DATABASE** в строке поставщика. Если база данных уже присоединена, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не присоединяет ее повторно, а использует для соединения присоединенную базу данных по умолчанию.|  
|**Встроенные функции безопасности**|DBPROP_AUTH_INTEGRATED|Принимает значение `SSPI` для проверки подлинности Windows.|  
|**MARS Connection**|SSPROP_INIT_MARSCONNECTION|Включает или отключает режим MARS для соединения для сервера версии [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] или более поздней версии. Распознаются значения `true` и `false` (значение по умолчанию — `false`).|  
|**MultiSubnetFailover**|SSPROP_INIT_MULTISUBNETFAILOVER|Всегда указывайте **MultiSubnetFailover=True** при соединении с прослушивателем группы доступности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или экземпляром отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. **MultiSubnetFailover=True** настраивает драйвер OLE DB для SQL Server для ускоренного обнаружения активного (в данный момент) сервера и подключения к нему. Возможные значения: `True` и `False`. Значение по умолчанию — `False`. Пример:<br /><br /> `MultiSubnetFailover=True`<br /><br /> Дополнительные сведения о поддержке OLE DB Driver for SQL Server для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в [этой статье](../features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).|  
|**Network Address**|SSPROP_INIT_NETWORKADDRESS|Сетевой адрес экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в организации.<br /><br /> Дополнительные сведения о допустимом синтаксисе адресов см. в описании ключевого слова **Address** в этом разделе.|  
|**Network Library**|SSPROP_INIT_NETWORKLIBRARY|Сетевая библиотека, используемая для установления соединения с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в организации.|  
|**Packet Size**|SSPROP_INIT_PACKETSIZE|Размер пакета потока табличных данных (TDS). Значением по умолчанию является 0 (фактическое значение будет определяться сервером).|  
|**Пароль**|DBPROP_AUTH_PASSWORD|Пароль имени входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Persist Security Info**|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|Принимает в качестве значений строки `true` и `false`. Если используется значение `false`, объекту источника данных запрещено хранить конфиденциальные данные проверки подлинности.|  
|**Поставщик**||В случае OLE DB Driver для SQL Server значение — `MSOLEDBSQL`.|  
|**Server SPN**|SSPROP_INIT_SERVERSPN|Имя участника-службы для сервера. Значение по умолчанию — пустая строка. Из-за этой пустой строки OLE DB Driver for SQL Server использует имя субъекта-службы по умолчанию, создаваемое поставщиком.|  
|**TransparentNetworkIPResolution**|SSPROP_INIT_TNIR|Влияет на последовательность подключений, когда первый разрешенный IP-адрес имени узла не отвечает и имеется несколько IP-адресов, связанных с именем этого узла. TNIR взаимодействует с MultiSubnetFailover и поддерживает различные варианты последовательности подключений. Возможные значения: `True` и `False`. Значение по умолчанию — `True`. Дополнительные сведения см. в статье [Использование разрешения IP-адресов прозрачной сети](../../oledb/features/using-transparent-network-ip-resolution.md).|  
|**Надежный сертификат сервера**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_INIT_TRUST_SERVER_CERTIFICATE|Принимает в качестве значений строки `true` и `false`. По умолчанию имеет значение `false`, которое указывает, что сертификат сервера будет проверяться.|  
|**Использование шифрования данных**<a href="#table3_1"><sup>**1**</sup></a>|SSPROP_INIT_ENCRYPT|Указывает, необходимо ли шифрование данных перед их отправкой по сети. Возможные значения: `true` и `false`. Значение по умолчанию — `false`.|  
|**Use FMTONLY**|SSPROP_INIT_USEFMTONLY|Определяет способ получения метаданных при подключении к [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] и более новым версиям. Возможные значения: `true` и `false`. Значение по умолчанию — `false`.<br /><br />По умолчанию OLE DB Driver for SQL Server использует хранимые процедуры [sp_describe_first_result_set](../../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) и [sp_describe_undeclared_parameters](../../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md) для извлечения метаданных. Эти хранимые процедуры имеют некоторые ограничения (например, их выполнение при обработке временных таблиц закончится сбоем). Если для параметра **UseFMTONLY** установлено значение `true`, драйвер должен использовать [SET FMTONLY](../../../t-sql/statements/set-fmtonly-transact-sql.md) для извлечения метаданных.|  
|**Идентификатор пользователя**|DBPROP_AUTH_USERID|Имя входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**Workstation ID**|SSPROP_INIT_WSID|Идентификатор рабочей станции.|  
  
<b id="table3_1">[1].</b> Для повышения безопасности поведение шифрования и проверки сертификатов изменяется при использовании свойств инициализации маркера доступа и проверки подлинности или их соответствующих ключевых слов строки подключения. Дополнительные сведения см. в разделе о [шифровании и проверке сертификатов](../features/using-azure-active-directory.md#encryption-and-certificate-validation).

 > [!NOTE] 
 > В строке подключения свойство «Old Password» устанавливает значение свойства SSPROP_AUTH_OLD_PASSWORD, являющееся текущим паролем (возможно, с истекшим сроком действия), недоступным через свойство строки поставщика.  
  
## <a name="see-also"></a>См. также раздел  

 [Создание приложений с помощью драйвера OLE DB для SQL Server](building-applications-with-oledb-driver-for-sql-server.md)
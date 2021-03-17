---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL)
ms.custom: ''
ms.date: 02/18/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0cd75d2eb6e613f36cecee8e79e3c8d6c99eed8d
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575348"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает набор всех клерков памяти, активных в данный момент в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_memory_clerks**. 
 
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary(8)**|Указывает уникальный адрес клерка памяти. Это первичный ключевой столбец. Не допускает значение NULL.|  
|**type**|**nvarchar(60)**|Указывает тип клерка памяти. Каждый клерк принадлежит к определенному типу, такому как CLR Clerks MEMORYCLERK_SQLCLR. Не допускает значение NULL.|  
|**name**|**nvarchar(256)**|Указывает внутреннее имя, назначенное данному клерку памяти. Компонент может иметь несколько клерков памяти определенного типа. Компонент может использовать определенные имена для идентификации клерков памяти одного и того же типа. Не допускает значение NULL.|  
|**memory_node_id**|**smallint**|Указывает идентификатор узла памяти. Не допускает значения NULL.|  
|**single_pages_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Дополнительные сведения см. в разделе [изменения в управлении памятью, начиная с SQL Server 2012 (11. x)](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).|  
|**pages_kb**|**bigint**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Устанавливает объем страничной памяти (в килобайтах), выделяемый для этого клерка памяти. Не допускает значение NULL.|  
|**multi_pages_kb**|**bigint**|**Применимо к**: с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Дополнительные сведения см. в разделе [изменения в управлении памятью, начиная с SQL Server 2012 (11. x)](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).<br /><br /> Объем выделенной многостраничной памяти в КБ. Это объем памяти, выделенной с помощью механизма распределения множества страниц узлов памяти. Эта память выделена вне буферного пула и использует преимущества виртуального блока распределения узлов памяти. Не допускает значение NULL.|  
|**virtual_memory_reserved_kb**|**bigint**|Указывает объем виртуальной памяти, зарезервированной клерком памяти. Не допускает значение NULL.|  
|**virtual_memory_committed_kb**|**bigint**|Указывает объем виртуальной памяти, зафиксированной клерком памяти. Объем зафиксированной памяти должен всегда быть меньше объема зарезервированной памяти. Не допускает значение NULL.|  
|**awe_allocated_kb**|**bigint**|Указывает объем физической памяти (в КБ), заблокированной в физической памяти и не выгруженной операционной системой. Не допускает значение NULL.|  
|**shared_memory_reserved_kb**|**bigint**|Указывает объем общей памяти, зарезервированной клерком памяти. Объем памяти, зарезервированной для использования при сопоставлении общей памяти и файлов. Не допускает значение NULL.|  
|**shared_memory_committed_kb**|**bigint**|Указывает объем общей памяти, зафиксированной клерком памяти. Не допускает значение NULL.|  
|**page_size_in_bytes**|**bigint**|Указывает гранулярность выделения страниц для этого клерка памяти. Не допускает значение NULL.|  
|**page_allocator_address**|**varbinary(8)**|Указывает адрес средства выделения страниц. Этот адрес уникален для клерка памяти и может использоваться в **sys.dm_os_memory_objects** для нахождение объектов памяти, привязанных к этому Клерку. Не допускает значение NULL.|  
|**host_address**|**varbinary(8)**|Указывает адрес памяти, по которому размещается данный клерк памяти. Дополнительные сведения см. в разделе [sys.dm_os_hosts &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md). Компоненты, такие как [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент, обращаются к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ресурсам памяти через интерфейс узла.<br /><br /> 0x00000000 = Клерк памяти принадлежит [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Не допускает значение NULL.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  

## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] основных целях служб, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] целей службы `VIEW DATABASE STATE` необходимо разрешение в базе данных.   
  
## <a name="remarks"></a>Комментарии

 Диспетчер памяти [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имеет трехуровневую иерархию. В нижней части иерархии располагаются узлы памяти. Средний уровень содержит клерки, кэш и пулы памяти. Верхний уровень содержит объекты памяти. Эти объекты используются для выделения памяти в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Узлы памяти обеспечивают интерфейс и реализацию низкоуровневых механизмов выделения. В пределах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] доступ к узлам памяти имеют только клерки памяти. Клерки памяти получают доступ к интерфейсам узлов памяти для ее выделения. Узлы памяти также ведут слежение за выделяемой клерками памятью в целях диагностики. Каждый компонент, выделяющий значительный объем памяти, должен создать свой клерк памяти и выделить необходимую ему память с помощью интерфейсов клерка. Часто компоненты создают соответствующие им клерки во время запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

### <a name="cachestore-and-userstore"></a>ЗАПИСАННЫХ и USERSTORE

ЗАПИСАННЫХ и USERSTORE — это клерки памяти, но они работают как реальные кэши. Обычно кэши сохраняют выделение памяти до тех пор, пока политика удаления кэша не освободит эти выделения. Чтобы избежать повторного создания, кэшированное выделение сохраняется в кэше как можно больше и обычно удаляется из кэша, когда он слишком старый, чтобы быть полезным, или когда пространство памяти требуется для новой информации (Дополнительные сведения см. в разделе " [Очистка часов](sys-dm-os-memory-cache-clock-hands-transact-sql.md#remarks))". Это один из двух основных элементов управления для управления жизненным циклом кэша и управления видимостью.

Хранилище кэша и хранилище пользователей отличаются тем, как они управляют временем существования выделений. В случае хранилища кэша все время существования записей полностью контролируется инфраструктурой кэширования SQLOS. При использовании хранилища пользователей время существования записей частично регулируется хранилищем. Реализация каждого пользовательского хранилища может быть характерна для природы выделения памяти, поэтому хранилища пользователей участвуют в контроле времени существования своих записей. 

Управление видимостью управляет видимостью записи. Запись в кэше может существовать, но может быть невидимой. Например, если запись кэша отмечена только для единственного использования, запись не будет видна после ее использования. Кроме того, запись кэша может быть помечена как «грязная»; Он будет продолжать работать в кэше, но не будет отображаться для поиска. Для обоих магазинов видимость записи контролируется структурой кэширования.

Дополнительные сведения см. в разделе [SQLOS Caching](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching).

### <a name="objectstore"></a>обжектсторе

Хранилище объектов — это простой пул. Он используется для кэширования однородных данных. Все записи в пулах рассматриваются как равные.  Хранилища объектов реализуют максимальное ограничение для управления размером относительно других кэшей.

Дополнительные сведения см. в разделе [SQLOS Caching](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching).

## <a name="types"></a>Типы

В следующей таблице перечислены типы клерков памяти.

|Тип  |Описание  |
|---------|---------|
|CACHESTORE_BROKERDSH     |     Это хранилище кэша используется для хранения распределений по [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) кэше заголовков безопасности диалогового окна    |
|CACHESTORE_BROKERKEK     |   Это хранилище кэша используется для хранения выделений путем [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  кэша ключей обмена ключами.    |
|CACHESTORE_BROKERREADONLY     |     Это хранилище кэша используется для хранения выделений в кэше только для чтения [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)    |
|CACHESTORE_BROKERRSB     |  Это хранилище кэша используется для хранения выделений путем [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) кэша [привязки удаленной службы](../../t-sql/statements/create-remote-service-binding-transact-sql.md) .    |
|CACHESTORE_BROKERTBLACS     |     Это хранилище кэша используется для хранения выделений путем [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) для структур доступа безопасности.    |
|CACHESTORE_BROKERTO     |  Это хранилище кэша используется для хранения выделений путем [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) кэша [объектов передачи](../performance-monitor/sql-server-broker-to-statistics-object.md) .   |
|CACHESTORE_BROKERUSERCERTLOOKUP     |    Это хранилище кэша используется для хранения выделений с помощью [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  кэша поиска сертификатов пользователей  |
|CACHESTORE_COLUMNSTOREOBJECTPOOL     |     Это хранилище кэша используется для выделения [индексов columnstore](../indexes/columnstore-indexes-overview.md) для [сегментов](../system-catalog-views/sys-column-store-segments-transact-sql.md) и [словарей](../system-catalog-views/sys-column-store-dictionaries-transact-sql.md) .   |
|CACHESTORE_CONVPRI     |  Это хранилище кэша используется для хранения выделений путем [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) для наблюдения за   [приоритетом диалогов](../system-catalog-views/sys-conversation-priorities-transact-sql.md)     |
|CACHESTORE_EVENTS     |     Это хранилище кэша используется для хранения выделений [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) [уведомлениях о событиях](../service-broker/event-notifications.md) . |
|CACHESTORE_FULLTEXTSTOPLIST     |    Этот клерк памяти используется для выделения Full-Text подсистеме для функций [списка стоп-слов](../search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) .       |
|CACHESTORE_NOTIF     |    Это хранилище кэша используется для выделения с помощью функций [уведомления о запросах](../../connect/ado-net/sql/query-notifications-sql-server.md) .    |
|CACHESTORE_OBJCP     |    Это хранилище кэша используется для кэширования объектов с помощью скомпилированных планов (CP): хранимых процедур, функций и триггеров. Чтобы продемонстрировать, после создания плана запроса для хранимой процедуры его план сохраняется в этом кэше.  |
|CACHESTORE_PHDR     |    Это хранилище кэша используется для временного кэширования памяти во время синтаксического анализа для представлений, ограничений и значений по умолчанию алгебризатор деревьев во время компиляции запроса. После синтаксического анализа запроса память должна быть освобождена. Некоторые примеры включают: множество инструкций в одном пакете — тысячи вставок или обновлений в один пакет, пакет T-SQL, содержащий большой динамически созданный запрос, большое количество значений в предложении IN.      |
|CACHESTORE_QDSRUNTIMESTATS     |   Это хранилище кэша используется для кэширования статистики времени выполнения [хранилища запросов](../performance/monitoring-performance-by-using-the-query-store.md)  . |
|CACHESTORE_SEARCHPROPERTYLIST     |     Это хранилище кэша используется для выделений обработчиком Full-Text для кэша [списка свойств](../search/search-document-properties-with-search-property-lists.md) .  |
|CACHESTORE_SEHOBTCOLUMNATTRIBUTE     |  Это хранилище кэша используется подсистемой хранилища для кэширования кучи или структуры метаданных столбцов сбалансированного дерева (HoBT).      |
|CACHESTORE_SQLCP     |    Это хранилище кэша используется для кэширования нерегламентированных запросов, подготовленных инструкций и курсоров на стороне сервера в кэше планов. Нерегламентированные запросы — это обычно инструкции языка T-SQL, передаваемые на сервер без явной параметризации. Подготовленные инструкции также используют это хранилище кэша. они отправляются приложением с помощью вызовов API, таких как [SQLPrepare ()](../../odbc/reference/syntax/sqlprepare-function.md) /  [SQLExecute](../../odbc/reference/syntax/sqlexecute-function.md) (ODBC) или [SqlCommand. Prepare](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.prepare) / [SqlCommand.Exeкутенонкуери](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.executenonquery) (ADO.NET) и будут отображаться на сервере как [sp_prepare](../system-stored-procedures/sp-prepare-transact-sql.md) / [sp_execute](../system-stored-procedures/sp-execute-transact-sql.md) или [sp_prepexec](../system-stored-procedures/sp-prepexec-transact-sql.md) выполнении системных процедур. Кроме того, курсоры на стороне сервера будут потребляться из этого хранилища кэша ([sp_cursoropen](../system-stored-procedures/sp-cursoropen-transact-sql.md), [sp_cursorfetch](../system-stored-procedures/sp-cursorfetch-transact-sql.md), [sp_cursorclose](../system-stored-procedures/sp-cursorclose-transact-sql.md)).    |
|CACHESTORE_STACKFRAMES     |    Это хранилище кэша используется для выделения внутренних структур ОС SQL, связанных с кадрами стека.     |
|CACHESTORE_SYSTEMROWSET     |   Это хранилище кэша используется для выделения внутренних структур, связанных с ведением журнала и восстановлением транзакций.      |
|CACHESTORE_TEMPTABLES     |     Это хранилище кэша используется для распределений, связанных с [временными таблицами и кэшированием табличных переменных](../databases/tempdb-database.md#performance-improvements-in-tempdb-for-sql-server) — частью кэша планов.    |
|CACHESTORE_VIEWDEFINITIONS     |     Это хранилище кэша используется для кэширования определений представлений в процессе оптимизации запроса.    |
|CACHESTORE_XML_SELECTIVE_DG     |     Это хранилище кэша используется для кэширования XML-структур при обработке XML.    |
|CACHESTORE_XMLDBATTRIBUTE     |     Это хранилище кэша используется для кэширования структур атрибутов XML для операций XML, таких как [XQuery](../../xquery/xquery-language-reference-sql-server.md).    |
|CACHESTORE_XMLDBELEMENT     |   Это хранилище кэша используется для кэширования структур XML-элементов для операций XML, таких как [XQuery](../../xquery/xquery-language-reference-sql-server.md).      |
|CACHESTORE_XMLDBTYPE     |    Это хранилище кэша используется для кэширования XML-структур для операций XML, таких как XQuery.     |
|CACHESTORE_XPROC     |   Это хранилище кэша используется для кэширования структур для [расширенных хранимых процедур (xproc)](../extended-stored-procedures-programming/database-engine-extended-stored-procedures-programming.md) в кэше планов.     |
|MEMORYCLERK_BACKUP     |    Этот клерк памяти используется для различных операций выделения с помощью функций [резервного копирования](../../t-sql/statements/backup-transact-sql.md)    |
|MEMORYCLERK_BHF     |      Этот клерк памяти используется для выделений для управления большими двоичными объектами (BLOB) во время выполнения запроса (поддержка обработки больших двоичных объектов).  |
|MEMORYCLERK_BITMAP     |     Этот клерк памяти используется для выделения функциональных возможностей ОС SQL для фильтрации по битовым картам    |
|MEMORYCLERK_CSILOBCOMPRESSION     |  Этот клерк памяти используется для выделения данных с помощью [сжатия больших двоичных объектов (BLOB) индекса columnstore](../data-compression/data-compression.md#columnstore-and-columnstore-archive-compression)    |
|MEMORYCLERK_DRTLHEAP     |    Этот клерк памяти используется для выделения функциональных возможностей ОС SQL   <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_EXPOOL     |    Этот клерк памяти используется для выделения функциональных возможностей ОС SQL   <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_EXTERNAL_EXTRACTORS     |   Этот клерк памяти используется для выделения подсистемой выполнения запросов для операций [пакетного режима](../query-processing-architecture-guide.md#batch-mode-execution) .   <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_FILETABLE     |      Этот клерк памяти используется для различных выделений с помощью функций таблиц [FileTable](../blob/filetables-sql-server.md) .   |
|MEMORYCLERK_FSAGENT     |      Этот клерк памяти используется для различных выделений с помощью функций [FileStream](../blob/filestream-sql-server.md) .    |
|MEMORYCLERK_FSCHUNKER     |      Этот клерк памяти используется для различных выделений с помощью функции [FileStream](../blob/filestream-sql-server.md) для создания блоков FILESTREAM.   |
|MEMORYCLERK_FULLTEXT     |     Этот клерк памяти используется для выделения структур механизма Full-Text.   |
|MEMORYCLERK_FULLTEXT_SHMEM     |   Этот клерк памяти используется для выделения структур механизмов Full-Text, связанных с подключением к общей памяти с помощью процесса полнотекстовой управляющей программы Full Text.      |
|MEMORYCLERK_HADR     |     Этот клерк памяти используется для выделения памяти функцией AlwaysOn     |
|MEMORYCLERK_HOST     |    Этот клерк памяти используется для выделения функциональных возможностей ОС SQL.     |
|MEMORYCLERK_LANGSVC     |     Этот клерк памяти используется для выделения инструкций и команд SQL T-SQL (средство синтаксического анализа, алгебризатор и т. д.).    |
|MEMORYCLERK_LWC     |   Этот клерк памяти используется для выделения Full-Text [семантической поисковой](../search/semantic-search-sql-server.md) подсистемой       |
|MEMORYCLERK_POLYBASE     |    Этот клерк памяти отслеживает выделение памяти для функций [polybase](../polybase/polybase-guide.md) в SQL Server.     |
|MEMORYCLERK_QSRANGEPREFETCH     |  Этот клерк памяти используется для выделений при выполнении запроса для предварительной выборки диапазона просмотра запросов.      |
|MEMORYCLERK_QUERYDISKSTORE     |     Этот клерк памяти используется выделениями памяти [хранилища запросов](../performance/monitoring-performance-by-using-the-query-store.md) в SQL Server.    |
|MEMORYCLERK_QUERYDISKSTORE_HASHMAP     |   Этот клерк памяти используется выделениями памяти [хранилища запросов](../performance/monitoring-performance-by-using-the-query-store.md) в SQL Server.      |
|MEMORYCLERK_QUERYDISKSTORE_STATS     |  Этот клерк памяти используется выделениями памяти [хранилища запросов](../performance/monitoring-performance-by-using-the-query-store.md) в SQL Server.      |
|MEMORYCLERK_QUERYPROFILE     |  Этот клерк памяти используется во время запуска сервера для включения профилирования запросов    <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_RTLHEAP     |    Этот клерк памяти используется для выделения функциональных возможностей ОС SQL.  <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_SECURITYAPI     |    Этот клерк памяти используется для выделения функциональных возможностей ОС SQL.  <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_SERIALIZATION     |     Только для внутреннего применения    |
|MEMORYCLERK_SLOG     |     Этот клерк памяти используется для выделения ресурсов sLog (дополнительный поток журнала в памяти) в [ускоренном восстановлении базы данных](../accelerated-database-recovery-concepts.md) . <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_SNI     |     Этот клерк памяти выделяет память для компонентов сетевого интерфейса сервера (SNI). SNI управляет подключением и пакеты [TDS](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/893fcc7e-8a39-4b3c-815a-773b7b982c50) для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  |
|MEMORYCLERK_SOSMEMMANAGER     |   Этот клерк памяти выделяет структуры для планирования потоков SQLOS (SOS) и управления памятью и вводом-выводом.     |
|MEMORYCLERK_SOSNODE     |     Этот клерк памяти выделяет структуры для планирования потоков SQLOS (SOS) и управления памятью и вводом-выводом.    |
|MEMORYCLERK_SOSOS     |     Этот клерк памяти выделяет структуры для планирования потоков SQLOS (SOS) и управления памятью и вводом-выводом.    |
|MEMORYCLERK_SPATIAL     |    Этот клерк памяти используется компонентами [пространственных данных](../spatial/spatial-data-sql-server.md) для выделения памяти.     |
|MEMORYCLERK_SQLBUFFERPOOL     |    Этот клерк памяти следит за наиболее крупными потребителями памяти, расположенными на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] страницах данных и индексов. Буферный пул или кэш данных хранит страницы данных и индексов, загруженные в память, чтобы обеспечить быстрый доступ к данным. Дополнительные сведения см. в разделе [Управление буферами](../memory-management-architecture-guide.md#buffer-management).     |
|MEMORYCLERK_SQLCLR     |     Этот клерк памяти используется для выделения средой [SQLCLR ](../clr-integration/clr-integration-overview.md).     |
|MEMORYCLERK_SQLCLRASSEMBLY     |     Этот клерк памяти используется для выделений для сборок [SQLCLR ](../clr-integration/clr-integration-overview.md) .     |
|MEMORYCLERK_SQLCONNECTIONPOOL     |     Этот клерк памяти кэширует сведения на сервере, который может потребоваться серверу клиентского приложения для наблюдения. Одним из примеров является приложение, которое создает дескрипторы подготовки с помощью  [sp_prepexecrpc](../system-stored-procedures/sp-prepexecrpc-transact-sql.md). Приложение должно надлежащим образом отменять подготовку (закрыть) эти дескрипторы после выполнения.  |
|MEMORYCLERK_SQLEXTENSIBILITY     |    Этот клерк памяти используется для выделения [средой расширяемости](../../machine-learning/concepts/extensibility-framework.md) для запуска внешних скриптов Python или R в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  <br /><br />**Область применения**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и более поздних версий   |
|MEMORYCLERK_SQLGENERAL     |   Этот клерк памяти может использоваться несколькими пользователями в ядре SQL. К примерам относятся память репликации, внутренняя Отладка или диагностика, некоторые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] функции запуска, некоторые функциональные возможности средства синтаксического анализа SQL, создание системных индексов, инициализация глобальных объектов памяти, создание подключения OLEDB внутри сервера и запросы к связанному серверу, трассировка профайлера на стороне сервера, создание данных Showplan, память для некоторых функциональных возможностей XML     |
|MEMORYCLERK_SQLHTTP     |    Не рекомендуется     |
|MEMORYCLERK_SQLLOGPOOL     |   Этот клерк памяти используется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пулом журналов.  Пул журналов — это кэш, используемый для повышения производительности при чтении журнала транзакций. В частности, это улучшает использование кэша журнала во время нескольких операций чтения журнала, уменьшая количество операций чтения журнала дискового ввода-вывода и позволяя совместно использовать операции просмотра журналов. Основными потребителями пула журналов являются функции AlwaysOn (изменение записи и отправка), диспетчер повторов, восстановление базы данных-анализ/повтор/Отмена, откат среды выполнения транзакций, репликация и CDC, резервное копирование и восстановление.    |
|MEMORYCLERK_SQLOPTIMIZER     |     Этот клерк памяти используется для выделения памяти на разных этапах компиляции запроса. К некоторым применениям относятся оптимизация запросов, диспетчер статистики индекса, компиляция определений представлений, Создание гистограммы.   |
|MEMORYCLERK_SQLQERESERVATIONS     |     Этот клерк памяти используется для выделений памяти, выделяемых для запросов для выполнения операций сортировки и хэширования во время выполнения запроса. Дополнительные сведения о резервировании выполнения запросов (предоставление памяти) см. в [этом блоге](https://techcommunity.microsoft.com/t5/sql-server-support/memory-grants-the-mysterious-sql-server-memory-consumer-with/ba-p/333994) .     |
|MEMORYCLERK_SQLQUERYCOMPILE     |    Этот клерк памяти используется оптимизатором запросов для выделения памяти во время компиляции запроса.   |
|MEMORYCLERK_SQLQUERYEXEC     |    Этот клерк памяти используется для выделения в следующих областях: [обработка пакетного режима](../query-processing-architecture-guide.md#batch-mode-execution), [параллельное выполнение запросов](../query-processing-architecture-guide.md#parallel-query-processing) , контекст выполнения запроса, [тесселяция индекса](../spatial/spatial-indexes-overview.md#tessellation), операции сортировки и хэширования (таблицы сортировки, хэш-таблицы), некоторые DVM обработки, выполнение [обновления статистики](../statistics/update-statistics.md)    |
|MEMORYCLERK_SQLQUERYPLAN     |     Этот клерк памяти используется для распределений по управлению страницами [кучи](../indexes/heaps-tables-without-clustered-indexes.md) , выделениям [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) и выделенным [sp_cursor * хранимым процедурам](../system-stored-procedures/cursor-stored-procedures-transact-sql.md) .   |
|MEMORYCLERK_SQLSERVICEBROKER     |   Этот клерк памяти используется [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) выделениями памяти.       |
|MEMORYCLERK_SQLSERVICEBROKERTRANSPORT     |     Этот клерк памяти используется [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) распределениями памяти.    |
|MEMORYCLERK_SQLSLO_OPERATIONS     |      Этот клерк памяти используется для сбора статистики производительности <br /><br />**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   |
|MEMORYCLERK_SQLSOAP     |     Не рекомендуется    |
|MEMORYCLERK_SQLSOAPSESSIONSTORE     |    Не рекомендуется     |
|MEMORYCLERK_SQLSTORENG     |   Этот клерк памяти используется для выделения нескольких компонентов подсистемы хранилища. К примерам компонентов относятся структуры для файлов базы данных, диспетчер файлов реплики моментальных снимков базы данных, монитор взаимоблокировок, структуры DBTABLE, структуры диспетчера журналов, некоторые структуры управления версиями tempdb, некоторые функции запуска сервера, контекст выполнения для дочерних потоков в параллельных запросах.      |
|MEMORYCLERK_SQLTRACE     |     Этот клерк памяти используется для выделения памяти [трассировки SQL](../sql-trace/sql-trace.md) на стороне сервера.     |
|MEMORYCLERK_SQLUTILITIES     |    Этот клерк памяти может использоваться несколькими распределителями в SQL Server. К примерам относятся резервное копирование и восстановление, Доставка журналов, зеркальное отображение базы данных, команды DBCC, код BCP на стороне сервера, работа некоторых параллелизма запросов, буферы просмотра журнала.    |
|MEMORYCLERK_SQLXML     |     Этот клерк памяти используется для выделения памяти при выполнении операций XML.    |
|MEMORYCLERK_SQLXP     |     Этот клерк памяти используется для выделения памяти при вызове [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [расширенных хранимых процедур](../extended-stored-procedures-reference/database-engine-extended-stored-procedures-reference.md).    |
|MEMORYCLERK_SVL     |    Этот клерк памяти используется для выделения внутренних структур ОС SQL |
|MEMORYCLERK_TEST     |    Только для внутреннего применения   |
|MEMORYCLERK_UNITTEST     |      Только для внутреннего применения  |
|MEMORYCLERK_WRITEPAGERECORDER     |    Этот клерк памяти используется для выделений с помощью записи страниц.   |
|MEMORYCLERK_XE     |    Этот клерк памяти используется для выделения памяти [расширенных событий](../extended-events/extended-events.md)      |
|MEMORYCLERK_XE_BUFFER     |      Этот клерк памяти используется для выделения памяти [расширенных событий](../extended-events/extended-events.md)   |
|MEMORYCLERK_XLOG_SERVER     |   Этот клерк памяти используется для выделения ресурсов xlog, используемых для управления файлами журнала в базе данных SQL Azure   <br /><br />**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |
|MEMORYCLERK_XTP     |    Этот клерк памяти используется для выделения памяти выполняющейся [в памяти OLTP](../in-memory-oltp/in-memory-oltp-in-memory-optimization.md) .     |
|OBJECTSTORE_LBSS     |    Это хранилище объектов используется для выделения временных LOB-переменных, параметров и промежуточных результатов для выражений. Пример, в котором используется это хранилище, — [возвращающие](../../connect/ado-net/sql/table-valued-parameters.md) табличное значение параметры (TVP). Дополнительные сведения об исправлениях в этой области см. в статье [4468102 базы знаний](https://support.microsoft.com/topic/kb4468102-fix-excessive-memory-usage-when-you-trace-rpc-events-that-involve-table-valued-parameters-in-sql-server-2016-and-2017-c68aa214-26f1-98de-6b4d-c7dcad82dbd4) и в  [базе знаний 4051359](https://support.microsoft.com/topic/kb4051359-fix-sql-server-runs-out-of-memory-when-table-valued-parameters-are-captured-in-extended-events-sessions-in-sql-server-2016-even-if-collecting-statement-or-data-stream-isn-t-enabled-a3639efa-0618-82a8-f6b1-8cdcba29ce6d) .     |
|OBJECTSTORE_LOCK_MANAGER     |      Этот клерк памяти отслеживает выделения памяти, сделанные [диспетчером блокировок](../sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine) в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .   |
|OBJECTSTORE_SECAUDIT_EVENT_BUFFER     |   Это хранилище объектов используется для SQL Server выделения памяти для [аудита](../security/auditing/sql-server-audit-database-engine.md) .        |
|OBJECTSTORE_SERVICE_BROKER     |     Это хранилище объектов используется [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)    |
|OBJECTSTORE_SNI_PACKET     |     Это хранилище объектов используется компонентами сетевого интерфейса сервера (SNI), которые управляют подключением.    |
|OBJECTSTORE_XACT_CACHE     |    Это хранилище объектов используется для кэширования сведений о транзакциях     |
|USERSTORE_DBMETADATA     |      Это хранилище объектов используется для структур метаданных     |
|USERSTORE_OBJPERM     |     Это хранилище используется для структур, отслеживающих безопасность и разрешения объектов.     |
|USERSTORE_QDSSTMT     |  Это хранилище кэша используется для кэширования инструкций [хранилища запросов](../performance/monitoring-performance-by-using-the-query-store.md)  .       |
|USERSTORE_SCHEMAMGR     |    Кэш диспетчера схемы хранит различные типы метаданных о объектах базы данных в памяти (например, таблицах). Обычным пользователем этого хранилища может быть база данных tempdb с такими объектами, как таблицы, временные процедуры, табличные переменные, возвращающие табличное значение параметры, рабочие таблицы, создано рабочих файлов, хранилище версий.  |
|USERSTORE_SXC     |    Это хранилище пользователя используется для выделений для хранения всех параметров [RPC](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/619c43b6-9495-4a58-9e49-a4950db245b3) .     |
|USERSTORE_TOKENPERM     |    TokenAndPermUserStore — это единое хранилище пользователей SOS, которое отслеживает записи безопасности для контекста безопасности, имени входа, пользователя, разрешения и аудита. Для хранения этих объектов выделяется несколько хэш-таблиц.    |

## <a name="see-also"></a>См. также  

 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  

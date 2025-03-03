---
description: sys.dm_exec_procedure_stats (Transact-SQL)
title: sys.dm_exec_procedure_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats_TSQL
- dm_exec_procedure_stats
- sys.dm_exec_procedure_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_procedure_stats dynamic management view
ms.assetid: ab8ddde8-1cea-4b41-a7e4-697e6ddd785a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0556a20a490aad3663144f8a34bab5ee54b65501
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751804"
---
# <a name="sysdm_exec_procedure_stats-transact-sql"></a>sys.dm_exec_procedure_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает суммарную статистику производительности для кэшированных хранимых процедур. Это представление содержит по одной строке для каждого кэшированного плана хранимой процедуры. Время существования строки равно времени пребывания хранимой процедуры в кэше. Когда хранимая процедура удаляется из кэша, соответствующая строка исключается из представления. В этот момент возникает событие трассировки SQL статистики производительности аналогично **sys.dm_exec_query_stats**.  
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.  
  
> [!NOTE]
> Результаты **sys.dm_exec_procedure_stats**  могут различаться при каждом выполнении, так как данные отражают только завершенные запросы, а не по-прежнему в полете.
> Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_exec_procedure_stats**. 

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------| 
|**database_id**|**int**|Идентификатор базы данных, в которой находится хранимая процедура.|  
|**object_id**|**int**|Идентификатор объекта хранимой процедуры.|  
|**type**|**char(2)**|Тип объекта:<br /><br /> P = хранимая процедура SQL<br /><br /> PC = хранимая процедура сборки (среда CLR)<br /><br /> X = расширенная хранимая процедура|  
|**type_desc**|**nvarchar(60)**|Описание типа объекта:<br /><br /> SQL_STORED_PROCEDURE<br /><br /> CLR_STORED_PROCEDURE<br /><br /> EXTENDED_STORED_PROCEDURE|  
|**sql_handle**|**varbinary (64)**|Это можно использовать для сопоставления с запросами в **sys.dm_exec_query_stats** , которые были выполнены в этой хранимой процедуре.|  
|**plan_handle**|**varbinary (64)**|Идентификатор плана в оперативной памяти. Этот идентификатор является временным и константным, только пока план сохраняется в кэше. Это значение можно использовать с динамическим административным представлением **sys.dm_exec_cached_plans** .<br /><br /> Значение всегда равно 0x000, если скомпилированная в собственном коде хранимая процедура запрашивает оптимизированную для памяти таблицу.|  
|**cached_time**|**datetime**|Время добавления хранимой процедуры в кэш.|  
|**last_execution_time**|**datetime**|Время последнего выполнения хранимой процедуры.|  
|**execution_count**|**bigint**|Количество выполнений хранимой процедуры со времени ее последней компиляции.|  
|**total_worker_time**|**bigint**|Общее количество времени ЦП, затраченное на выполнение хранимой процедуры с момента компиляции, в микросекундах.<br /><br /> Для скомпилированных в собственном коде хранимых процедур функция **total_worker_time** может быть неточной, если за время меньше миллисекунды выполняется большое количество хранимых процедур.|  
|**last_worker_time**|**bigint**|Время ЦП, затраченное на последнее выполнение хранимой процедуры, в микросекундах. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Минимальное время ЦП в микросекундах, когда эта хранимая процедура когда-либо использовалась во время одного выполнения. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Максимальное время ЦП (в микросекундах), которое эта хранимая процедура когда-либо использовала во время одного выполнения. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Общее число физических операций чтения, выполненных при выполнении хранимой процедуры с момента ее компиляции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**last_physical_reads**|**bigint**|Число операций физического считывания за время последнего выполнения хранимой процедуры.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**min_physical_reads**|**bigint**|Минимальное число физических операций чтения, выполненных этой хранимой процедурой во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**max_physical_reads**|**bigint**|Максимальное число физических операций чтения, выполненных этой хранимой процедурой во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**total_logical_writes**|**bigint**|Общее число операций логической записи, выполненных при выполнении хранимой процедуры с момента ее компиляции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**last_logical_writes**|**bigint**|Количество страниц буферного пула, изменялся за время последнего выполнения плана. Если страница уже является «грязной» (т. е. измененной), операции записи не учитываются.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**min_logical_writes**|**bigint**|Минимальное число операций логической записи, выполненных этой хранимой процедурой во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**max_logical_writes**|**bigint**|Максимальное число операций логической записи, выполненных этой хранимой процедурой во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**total_logical_reads**|**bigint**|Общее число логических операций чтения, выполненных при выполнении хранимой процедуры с момента ее компиляции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**last_logical_reads**|**bigint**|Число операций логического считывания за время последнего выполнения хранимой процедуры.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**min_logical_reads**|**bigint**|Минимальное число операций логического считывания, выполненных этой хранимой процедурой во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**max_logical_reads**|**bigint**|Максимальное число операций логического считывания, выполненных этой хранимой процедурой во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**total_elapsed_time**|**bigint**|Общее время, затраченное на выполнение хранимой процедуры, в микросекундах.|  
|**last_elapsed_time**|**bigint**|Затраченное время в микросекундах для последнего выполненного выполнения этой хранимой процедуры.|  
|**min_elapsed_time**|**bigint**|Минимальное время, затраченное на выполнение хранимой процедуры, в микросекундах.|  
|**max_elapsed_time**|**bigint**|Максимальное время, затраченное на выполнение хранимой процедуры, в микросекундах.|  
|**total_spills**|**bigint**|Общее число страниц, сброшенных при выполнении этой хранимой процедуры с момента ее компиляции.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**last_spills**|**bigint**|Количество страниц, сброшенных при последнем выполнении хранимой процедуры.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**min_spills**|**bigint**|Минимальное число страниц, которые эта хранимая процедура когда-либо заблокировала во время одного выполнения.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**max_spills**|**bigint**|Максимальное число страниц, которые когда-либо были сброшены этой хранимой процедурой во время одного выполнения.<br /><br /> Область **применения**: начиная с [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3|  
|**pdw_node_id**|**int**|Идентификатор узла, на котором находится данное распределение.<br /><br />**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
|**total_page_server_reads**|**bigint**|Общее число операций чтения на сервере страниц, выполненных с момента компиляции этой хранимой процедуры.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  
|**last_page_server_reads**|**bigint**|Число операций чтения сервера, выполненных при последнем выполнении хранимой процедуры.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  
|**min_page_server_reads**|**bigint**|Минимальное число серверных страниц считывает, что эта хранимая процедура когда-либо выполнялась во время одного выполнения.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  
|**max_page_server_reads**|**bigint**|Максимальное число серверных страниц считывает, что эта хранимая процедура когда-либо выполнялась во время одного выполнения.<br /><br /> Область **применения**: масштабирование базы данных SQL Azure|  
  
 <sup>1</sup> для хранимых процедур, скомпилированных в собственном режиме, когда сбор статистики включен, время рабочей роли собираются в миллисекундах. Если запрос выполняется за время меньше миллисекунды, это значение будет равно 0.  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
   
## <a name="remarks"></a>Remarks  
 Статистика в представлении обновляется после завершения выполнения хранимой процедуры.  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются сведения о 10 первых хранимых процедурах, идентифицируемых по среднему затраченному времени.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'proc name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_procedure_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>См. также:  
[Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
[sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
[sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)    
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)    
[sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)    
[sys.dm_exec_cached_plans (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)    
  

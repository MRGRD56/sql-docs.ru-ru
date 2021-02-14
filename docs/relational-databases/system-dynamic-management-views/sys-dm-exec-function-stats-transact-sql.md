---
description: sys.dm_exec_function_stats (Transact-SQL)
title: sys.dm_exec_function_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_function_stats
- sys.dm_exec_function_stats_tsql
- dm_exec_function_stats
- dm_exec_function_stats_tsql
helpviewer_keywords:
- sys.dm_exec_function_stats dynamic management view
ms.assetid: 4c3d6a02-08e4-414b-90be-36b89a0e5a3a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: be2d102b81bf5535354b99cc3405d50a0bf94915
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342979"
---
# <a name="sysdm_exec_function_stats-transact-sql"></a>sys.dm_exec_function_stats (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Возвращает статистическую статистику производительности для кэшированных функций. Представление возвращает одну строку для каждого кэшированного плана функции, а время существования строки — до тех пор, пока функция остается в кэше. При удалении функции из кэша соответствующая строка удаляется из этого представления. В этот момент возникает событие трассировки SQL статистики производительности аналогично **sys.dm_exec_query_stats**. Возвращает сведения о скалярных функциях, включая функции в памяти и скалярные функции CLR. Не возвращает сведения о функциях с табличным значением и о скалярных функциях, встроенных в [скалярное встраивание UDF](../../relational-databases/user-defined-functions/scalar-udf-inlining.md).
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.  
  
> [!NOTE]
> Результаты **sys.dm_exec_function_stats**  могут различаться при каждом выполнении, так как данные отражают только завершенные запросы, а не по-прежнему в полете. 

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор базы данных, в которой находится функция.|  
|**object_id**|**int**|Идентификационный номер объекта функции.|  
|**type**|**char(2)**|Тип объекта: FN = скалярные функции|  
|**type_desc**|**nvarchar(60)**|Описание типа объекта: SQL_SCALAR_FUNCTION|  
|**sql_handle**|**varbinary (64)**|Это можно использовать для сопоставления с запросами в **sys.dm_exec_query_stats** , которые были выполнены в этой функции.|  
|**plan_handle**|**varbinary (64)**|Идентификатор плана в оперативной памяти. Этот идентификатор является временным и константным, только пока план сохраняется в кэше. Это значение можно использовать с динамическим административным представлением **sys.dm_exec_cached_plans** .<br /><br /> Всегда будет 0x000, когда скомпилированная в собственном режиме функция запрашивает оптимизированную для памяти таблицу.|  
|**cached_time**|**datetime**|Время добавления функции в кэш.|  
|**last_execution_time**|**datetime**|Время последнего выполнения функции.|  
|**execution_count**|**bigint**|Количество раз, когда функция была выполнена с момента последней компиляции.|  
|**total_worker_time**|**bigint**|Общее количество времени ЦП, затраченное на выполнение этой функции с момента компиляции, в микросекундах.<br /><br /> Для функций, скомпилированных в собственном режиме, **total_worker_time** может быть неточным, если количество выполнений меньше 1 миллисекунды.|  
|**last_worker_time**|**bigint**|Время ЦП, затраченное на время последнего выполнения функции, в микросекундах. <sup>1</sup>|  
|**min_worker_time**|**bigint**|Минимальное время ЦП в микросекундах, которое эта функция когда-либо использовала во время одного выполнения. <sup>1</sup>|  
|**max_worker_time**|**bigint**|Максимальное время ЦП (в микросекундах), которое эта функция когда-либо использовала во время одного выполнения. <sup>1</sup>|  
|**total_physical_reads**|**bigint**|Общее число операций физического считывания, выполненных с момента компиляции этой функции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**last_physical_reads**|**bigint**|Число операций физического считывания, выполненных при последнем выполнении функции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**min_physical_reads**|**bigint**|Минимальное число физических операций чтения, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**max_physical_reads**|**bigint**|Максимальное число физических операций чтения, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**total_logical_writes**|**bigint**|Общее число логических операций записи, выполненных выполнением этой функции с момента ее компиляции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**last_logical_writes**|**bigint**|Количество страниц в буферном пуле, загрязненных во время последнего выполнения плана. Если страница уже является «грязной» (т. е. измененной), операции записи не учитываются.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**min_logical_writes**|**bigint**|Минимальное число операций логической записи, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**max_logical_writes**|**bigint**|Максимальное число операций логической записи, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**total_logical_reads**|**bigint**|Общее число операций логического считывания, выполненных с момента компиляции этой функции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**last_logical_reads**|**bigint**|Число логических операций чтения, выполненных при последнем выполнении функции.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**min_logical_reads**|**bigint**|Минимальное число операций логического считывания, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**max_logical_reads**|**bigint**|Максимальное число операций логического считывания, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> Значение всегда равно 0 при запросе оптимизированной для памяти таблицы.|  
|**total_elapsed_time**|**bigint**|Общее затраченное время в микросекундах для выполнения этой функции.|  
|**last_elapsed_time**|**bigint**|Затраченное время в микросекундах для последнего выполненного выполнения этой функции.|  
|**min_elapsed_time**|**bigint**|Минимальное затраченное время в микросекундах для любого завершенного выполнения этой функции.|  
|**max_elapsed_time**|**bigint**|Максимальное затраченное время в микросекундах для любого завершенного выполнения этой функции.|  
|**total_page_server_reads**|**bigint**|Общее число операций чтения сервера страниц, выполненных с момента компиляции этой функции.<br /><br /> **Применимо к:** Масштабирование базы данных SQL Azure.|  
|**last_page_server_reads**|**bigint**|Число операций чтения сервера страниц, выполненных при последнем выполнении функции.<br /><br /> **Применимо к:** Масштабирование базы данных SQL Azure.|  
|**min_page_server_reads**|**bigint**|Минимальное количество операций чтения сервером страниц, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> **Применимо к:** Масштабирование базы данных SQL Azure.|  
|**max_page_server_reads**|**bigint**|Максимальное количество операций чтения на сервере страниц, которые эта функция когда-либо выполняла во время одного выполнения.<br /><br /> **Применимо к:** Масштабирование базы данных SQL Azure.|
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются сведения о первых десяти функциях, идентифицируемых по среднему затраченному времени.  
  
```sql  
SELECT TOP 10 d.object_id, d.database_id, OBJECT_NAME(object_id, database_id) 'function name',   
    d.cached_time, d.last_execution_time, d.total_elapsed_time,  
    d.total_elapsed_time/d.execution_count AS [avg_elapsed_time],  
    d.last_elapsed_time, d.execution_count  
FROM sys.dm_exec_function_stats AS d  
ORDER BY [total_worker_time] DESC;  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции, связанные с выполнением &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 
 [sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)   
 [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  
  
  

---
description: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
title: sys.dm_db_missing_index_group_stats_query (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_group_stats_query_TSQL
- sys.dm_db_missing_index_group_stats_query
- dm_db_missing_index_group_stats_query_TSQL
- dm_db_missing_index_group_stats_query
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_group_stats_query dynamic management view
- missing indexes feature [SQL Server], sys.dm_db_missing_index_group_stats_query dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 01554b6abf8728ff317638f3e5e304b3823890ac
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551553"
---
# <a name="sysdm_db_missing_index_group_stats_query-transact-sql"></a>sys.dm_db_missing_index_group_stats_query (Transact-SQL)
[!INCLUDE [SQL Server 2019 SQL Database](../../includes/applies-to-version/sqlserver2019-asdb-asdbmi.md)]

  Возвращает сведения о запросах, которые требовали отсутствующего индекса из групп отсутствующих индексов, исключая пространственные индексы. Для каждой группы отсутствующих индексов может быть возвращено более одного запроса. Одна группа отсутствующих индексов может содержать несколько запросов, для которых необходим один и тот же индекс.
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.  
    
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**group_handle**|**int**|Идентифицирует группу отсутствующих индексов. Этот идентификатор уникален в пределах сервера.<br /><br /> Другие столбцы содержат сведения обо всех запросах, для которых индекс в группе считается отсутствующим.<br /><br /> Группа индексов содержит только один индекс.<BR><BR>Можно объединить `index_group_handle` в [sys.dm_db_missing_index_groups](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md).|  
|**query_hash**|**Binary (8)**|Двоичное хэш-значение рассчитывается для запроса и используется для идентификации запросов с аналогичной логикой. Можно использовать хэш запроса для определения использования статистических ресурсов для запросов, которые отличаются только своими литеральными значениями.|  
|**query_plan_hash**|**Binary (8)**|Двоичное хэш-значение рассчитывается для плана выполнения запроса и используется для идентификации аналогичных планов выполнения запросов. Можно использовать хэш плана запроса для нахождения совокупной стоимости запросов со схожими планами выполнения.<br /><br /> Значение всегда равно 0x000, если скомпилированная в собственном коде хранимая процедура запрашивает оптимизированную для памяти таблицу.|  
|**last_sql_handle**|**varbinary (64)**|Токен, однозначно определяющий пакет или хранимую процедуру последнего скомпилированного оператора, который требовал этого индекса.<BR><BR>`last_sql_handle` можно использовать для получения текста SQL запроса путем вызова функции динамического управления [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .|
|**last_statement_start_offset**|**int**|Указывает, что в байтах, начиная с 0, начальной позицией запроса, который описывает строка, в тексте пакета или сохраненном объекте для последнего скомпилированного оператора, который требовал этого индекса в своем пакете SQL.|
|**last_statement_end_offset**|**int**|Указывает, что в байтах, начиная с 0, конечной позиции запроса, который описывает строка, в тексте пакета или сохраненном объекте для последнего скомпилированного оператора, который требовал этого индекса в своем пакете SQL.|
|**last_statement_sql_handle**|**varbinary (64)**|Токен, однозначно определяющий пакет или хранимую процедуру последнего скомпилированного оператора, который требовал этого индекса.<BR><BR>`last_statement_sql_handle`Вместе с `last_statement_start_offset` и `last_statement_end_offset` можно использовать для получения текста SQL запроса путем вызова функции динамического управления [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md) .<BR><BR>Если хранилище запросов не было включено при компиляции запроса, возвращает 0.|
|**user_seeks**|**bigint**|Количество операций поиска по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**user_scans**|**bigint**|Количество операций просмотра по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_user_seek**|**datetime**|Дата и время последней операции поиска по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_user_scan**|**datetime**|Дата и время последней операции просмотра по запросам пользователя, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**avg_total_user_cost**|**float**|Средняя стоимость запросов пользователя, которая могла быть уменьшена с помощью индекса в группе.|  
|**avg_user_impact**|**float**|Средний процент выигрыша, который могли получить запросы пользователя, если создать эту группу отсутствующих индексов. Значение показывает, что стоимость запроса в среднем уменьшится на этот процент, если создать эту группу отсутствующих индексов.|  
|**system_seeks**|**bigint**|Количество операций поиска по запросам системы, таким как запросы auto stats, для которых мог бы использоваться рекомендованный индекс в группе. Дополнительные сведения см. в разделе [класс событий auto stats](../../relational-databases/event-classes/auto-stats-event-class.md).|  
|**system_scans**|**bigint**|Количество операций просмотра по запросам системы, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_system_seek**|**datetime**|Дата и время последней операции системного поиска по запросам системы, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**last_system_scan**|**datetime**|Дата и время последней операции системного просмотра по запросам системы, для которых мог бы использоваться рекомендованный индекс в группе.|  
|**avg_total_system_cost**|**float**|Средняя стоимость системных запросов, которая могла быть уменьшена с помощью индекса в группе.|  
|**avg_system_impact**|**float**|Средний процент выигрыша, который могли получить запросы системы, если создать эту группу отсутствующих индексов. Значение показывает, что стоимость запроса в среднем уменьшится на этот процент, если создать эту группу отсутствующих индексов.|  
  
## <a name="remarks"></a>Примечания  
 Сведения, возвращаемые, `sys.dm_db_missing_index_group_stats_query` обновляются при каждом выполнении запроса, а не при компиляции или перекомпиляции запроса. Статистика использования не сохраняется и сохраняется только до перезапуска ядра СУБД. Администраторы базы данных должны периодически делать резервные копии сведений об отсутствующих индексах, если необходимо сохранить статистику использования после перезагрузки сервера. Используйте `sqlserver_start_time` столбец в [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) , чтобы найти время последнего запуска ядра СУБД.   
 
  >[!NOTE]
  >Результирующий набор для этого динамического административного представления ограничен 600 строк. Каждая строка содержит один отсутствующий индекс. Если у вас больше 600 отсутствующих индексов, следует устранить существующие отсутствующие индексы, чтобы можно было просмотреть новые.

## <a name="permissions"></a>Разрешения  
 Для выполнения запроса к этому динамическому административному представлению пользователям должно быть предоставлено разрешение VIEW SERVER STATE или любое другое, подразумевающее разрешение VIEW SERVER STATE.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показано, как использовать `sys.dm_db_missing_index_group_stats_query` динамическое административное представление.  
  
  
### <a name="a-find-the-latest-query-text-for-the-top-10-highest-anticipated-improvements-for-user-queries"></a>A. Поиск последнего текста запроса с 10 наиболее высокими ожидаемыми улучшениями для пользовательских запросов 
 Следующий запрос возвращает последний записанный текст запроса для 10 отсутствующих индексов, которые выдают наибольшее ожидаемое совокупное улучшение в порядке убывания.  

```sql
SELECT TOP 10 
    SUBSTRING
    (
            sql_text.text,
            misq.last_statement_start_offset / 2 + 1,
            (
            CASE misq.last_statement_Start_offset
                WHEN -1 THEN datalength(sql_text.text)
                ELSE misq.last_statement_end_offset
            END - misq.last_statement_start_offset
            ) / 2 + 1
    ),
    misq.*
FROM sys.dm_db_missing_index_group_stats_query AS misq
CROSS APPLY sys.dm_exec_sql_text(last_sql_handle) AS sql_text
ORDER BY avg_total_user_cost * avg_user_impact * (user_seeks + user_scans) DESC; 
```
  
## <a name="see-also"></a>См. также:  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](sys-dm-os-sys-info-transact-sql.md)
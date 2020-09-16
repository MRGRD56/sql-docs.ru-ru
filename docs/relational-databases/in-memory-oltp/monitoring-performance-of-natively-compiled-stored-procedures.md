---
title: Отслеживание производительности скомпилированных в собственном коде хранимых процедур
description: Узнайте, как наблюдать за производительностью хранимых процедур, скомпилированных в собственном коде, а также других скомпилированных в собственном коде модулей T-SQL.
ms.custom: seo-dt-2019
ms.date: 04/03/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 55548cb2-77a8-4953-8b5a-f2778a4f13cf
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dbac8fa27b79b96a55f580983586141e14e27fc1
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545188"
---
# <a name="monitoring-performance-of-natively-compiled-stored-procedures"></a>Отслеживание производительности скомпилированных в собственном коде хранимых процедур

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В этой статье показано, как наблюдать за производительностью хранимых процедур, скомпилированных в собственном коде, а также других скомпилированных в собственном коде модулей T-SQL.  
  
## <a name="using-extended-events"></a>Использование расширенных событий  
 Для трассировки выполнения запроса используйте расширенное событие **sp_statement_completed** . Создайте сеанс с этим событием, при этом можно использовать фильтр в object_id для определенной хранимой процедуры, скомпилированной в собственном коде. Расширенное событие вызывается после выполнения каждого запроса. Время ЦП и время существования, указанные расширенным событием, показывают объем ресурсов ЦП, который потребовался на выполнение запроса, и время его выполнения. Скомпилированная в собственном коде хранимая процедура, которая потребляет значительное время ЦП, может сталкиваться с проблемами производительности.  
  
**line_number**вместе с **object_id** в расширенном событии можно использовать для анализа запросов. Следующий запрос может использоваться для получения определения процедуры. Номер строки можно использовать для поиска запроса в определении.  
  
```sql  
SELECT [definition]
FROM sys.sql_modules
WHERE object_id=object_id;
```  
    
## <a name="using-data-management-views-and-query-store"></a>Использование динамических административных представлений и хранилища запросов
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] поддерживают сбор статистики выполнения для скомпилированных в собственном коде хранимых процедур как на уровне процедуры, так и на уровне запроса. Из-за влияния на производительность сбор статистики выполнения по умолчанию не используется.  

Статистика выполнения отражается в системных представлениях [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md) и [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md), а также в [хранилище запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

## <a name="procedure-level-execution-statistics"></a>Статистика выполнения на уровне процедур

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : включите или отключите сбор статистики для скомпилированных в собственном коде хранимых процедур на уровне процедуры с помощью [sys.sp_xtp_control_proc_exec_stats (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).  Следующая инструкция включает сбор статистики выполнения на уровне процедуры для всех скомпилированных в собственном коде модулей T-SQL текущего экземпляра:

```sql
EXEC sys.sp_xtp_control_proc_exec_stats 1
```

**[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]** и **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : включите или отключите сбор статистики для скомпилированных в собственном коде хранимых процедур на уровне процедуры с помощью [database-scoped configuration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md), используя параметр `XTP_PROCEDURE_EXECUTION_STATISTICS`. Следующая инструкция включает сбор статистики выполнения на уровне процедуры для всех скомпилированных в собственном коде модулей T-SQL текущей базы данных:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_PROCEDURE_EXECUTION_STATISTICS = ON;
```

## <a name="query-level-execution-statistics"></a>Статистика выполнения на уровне запросов

**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : включите или отключите сбор статистики для скомпилированных в собственном коде хранимых процедур на уровне запроса с помощью [sys.sp_xtp_control_query_exec_stats (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).  Следующая инструкция включает сбор статистики выполнения на уровне запроса для всех скомпилированных в собственном коде модулей T-SQL текущего экземпляра:

```sql
EXEC sys.sp_xtp_control_query_exec_stats 1
```

**[!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]** и **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : включите или отключите сбор статистики для скомпилированных в собственном коде хранимых процедур на уровне инструкции с помощью [database-scoped configuration](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md), используя параметр `XTP_QUERY_EXECUTION_STATISTICS`. Следующая инструкция включает сбор статистики выполнения на уровне запроса для всех скомпилированных в собственном коде модулей T-SQL текущей базы данных:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET XTP_QUERY_EXECUTION_STATISTICS = ON;
```

## <a name="sample-queries"></a>Примеры запросов

 После того как статистика выполнения хранимых процедур, скомпилированных в собственном коде, будет собрана, запросить ее для определенной процедуры можно будет с помощью [sys.dm_exec_procedure_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md), а для запросов — с помощью [sys.dm_exec_query_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md).  
 
  
 После сбора статистики следующий запрос возвращает имена и статистику выполнения скомпилированных в собственном коде хранимых процедур в текущей базе данных.  

```sql
SELECT object_id, object_name(object_id) AS 'object name',
       cached_time, last_execution_time, execution_count,
       total_worker_time, last_worker_time,
       min_worker_time, max_worker_time,
       total_elapsed_time, last_elapsed_time,
       min_elapsed_time, max_elapsed_time
FROM sys.dm_exec_procedure_stats
WHERE database_id = DB_ID()
      AND object_id IN (SELECT object_id FROM sys.sql_modules WHERE uses_native_compilation = 1)
ORDER BY total_worker_time desc;
```

Следующий запрос возвращает текст запроса, а также статистику выполнения всех запросов из скомпилированных в собственном коде хранимых процедур в текущей базе данных, для которой были собраны статистические данные. Статистика при этом упорядочивается по общему времени рабочей роли в убывающем порядке.  

```sql
SELECT st.objectid,
        OBJECT_NAME(st.objectid) AS 'object name',
        SUBSTRING(
            st.text,
            (qs.statement_start_offset/2) + 1,
            ((qs.statement_end_offset-qs.statement_start_offset)/2) + 1
            ) AS 'query text',
        qs.creation_time, qs.last_execution_time, qs.execution_count,
        qs.total_worker_time, qs.last_worker_time, qs.min_worker_time, 
        qs.max_worker_time, qs.total_elapsed_time, qs.last_elapsed_time,
        qs.min_elapsed_time, qs.max_elapsed_time
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(sql_handle) st
WHERE database_id = DB_ID()
      AND object_id IN (SELECT object_id FROM sys.sql_modules WHERE uses_native_compilation = 1)
ORDER BY total_worker_time desc;
```

## <a name="query-execution-plans"></a>Планы выполнения запросов

 Скомпилированные в собственном коде хранимые процедуры поддерживают SHOWPLAN_XML (предполагаемый план выполнения). С помощью предполагаемого плана выполнения можно проверять план запроса с целью выявления проблем. Общие причины появления некачественных планов.  
  
-   Статистика не была обновлена перед созданием процедуры.  
  
-   Отсутствующие индексы  
  
 Для получения Showplan XML выполняется следующая инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
```sql  
SET SHOWPLAN_XML ON  
GO  
EXEC my_proc   
GO  
SET SHOWPLAN_XML OFF  
GO  
```  
  
 Кроме того, в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]выберите имя процедуры и щелкните **Показать предполагаемый план выполнения**.  
  
 Предполагаемый план выполнения для скомпилированных в собственном коде хранимых процедур показывает операторы и выражения для запросов из процедуры. [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] не поддерживает все атрибуты SHOWPLAN_XML для скомпилированных в собственном коде хранимых процедур. Например, атрибуты, связанные с оценкой затрат оптимизатора запросов, не являются частью SHOWPLAN_XML для процедуры.  
  
## <a name="see-also"></a>См. также:  
 [Скомпилированные в собственном коде хранимые процедуры](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)  
  
  

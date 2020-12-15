---
title: Мониторинг PolyBase и устранение неполадок | Документация Майкрософт
description: Чтобы устранить неполадки с PolyBase, используйте эти представления и динамические административные представления. Просмотр плана запроса PolyBase, мониторинг узлов в группе PolyBase и настройка высокой доступности узла имени Hadoop.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
ms.assetid: f119e819-c3ae-4e0b-a955-3948388a9cfe
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 4440573f9c9ae00cc293ff356127c1de65b272ff
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97406533"
---
# <a name="monitor-and-troubleshoot-polybase"></a>Мониторинг PolyBase и устранение неполадок

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

В этом разделе описаны методы по устранению неполадок с PolyBase.

## <a name="catalog-views"></a>Представления каталога

Ниже приведены представления каталога,с помощью которых можно управлять операциями PolyBase.

|Представление|Описание|  
|-|-|  
|[sys.external_tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-external-tables-transact-sql.md)|Определяет внешние таблицы|  
|[sys.external_data_sources (Transact-SQL)](../../relational-databases/system-catalog-views/sys-external-data-sources-transact-sql.md)|Определяет источники внешних таблиц|  
|[sys.external_file_formats (Transact-SQL)](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)|Определяет форматы внешних файлов|  

## <a name="dynamic-management-views"></a>Динамические административные представления  

:::row:::
    :::column:::
        [sys.dm_exec_compute_node_errors (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_compute_node_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_compute_nodes (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_distributed_request_steps (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-request-steps-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_distributed_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-requests-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_distributed_sql_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-distributed-sql-requests-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_dms_services (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-services-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_dms_workers (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [sys.dm_exec_external_operations (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-external-operations-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.dm_exec_external_work (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-external-work-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Запросы PolyBase разбиваются на серии шагов в sys.dm_exec_distributed_request_steps. В следующей таблице приводится сопоставление имен шагов и динамических административных представлений.

|Шаг PolyBase|Связанное динамическое административное представление|  
|-|-|
|HadoopJobOperation | sys.dm_exec_external_operations|
|RandomIdOperation | sys.dm_exec_distributed_request_steps|
|HadoopRoundRobinOperation | sys.dm_exec_dms_workers|
|StreamingReturnOperation | sys.dm_exec_dms_workers|
|OnOperation | sys.dm_exec_distributed_sql_requests |

## <a name="to-monitor-polybase-queries-using-dmvs"></a>Мониторинг запросов PolyBase с использованием динамических административных представлений

Здесь описаны динамические административные представления, которые можно использовать для мониторинга и устранения неполадок с запросами PolyBase.

1. **Поиск запросов с наиболее длительным временем выполнения.**  

   Запишите идентификатор выполнения запроса с наиболее длительным временем выполнения.

   ```sql
    -- Find the longest running query  
   SELECT execution_id, st.text, dr.total_elapsed_time  
   FROM sys.dm_exec_distributed_requests  dr  
         cross apply sys.dm_exec_sql_text(sql_handle) st  
   ORDER BY total_elapsed_time DESC;  
   ```

1. **Поиск действия распределенного запроса с наиболее длительным временем выполнения.**  

   Используйте идентификатор выполнения, записанный при выполнении предыдущего действия. Запишите индекс действия с наиболее длительным временем выполнения.

   Просмотрите значение параметра location_type действия с наиболее длительным временем выполнения:  

   - Головной или вычислительный: подразумевается операция SQL. Перейдите к шагу 3a.

      - DMS: подразумевается операция службы перемещения данных PolyBase. Перейдите к шагу 3b.

      ```sql  
      -- Find the longest running step of the distributed query plan  
      SELECT execution_id, step_index, operation_type, distribution_type,   
      location_type, status, total_elapsed_time, command   
      FROM sys.dm_exec_distributed_request_steps   
      WHERE execution_id = 'QID4547'   
      ORDER BY total_elapsed_time DESC;  
      ```  

1. **Поиск данных о ходе выполнения действия с наиболее длительным временем выполнения.**  

   1. **Поиск данных о ходе выполнения действия SQL.**  

      Используйте идентификатор выполнения и индекс действия, записанные при выполнении предыдущих действий. Используйте идентификатор выполнения и индекс действия, записанные при выполнении предыдущих действий.

      ```sql  
      -- Find the execution progress of SQL step    
      SELECT execution_id, step_index, distribution_id, status,   
      total_elapsed_time, row_count, command   
      FROM sys.dm_exec_distributed_sql_requests   
      WHERE execution_id = 'QID4547' and step_index = 1;  
      ```  

   1. **Поиск данных о ходе выполнения действия DMS.**

      Используйте идентификатор выполнения и индекс действия, записанные при выполнении предыдущих действий.

      ```sql  
      -- Find the execution progress of DMS step    
      SELECT execution_id, step_index, dms_step_index, status,   
      type, bytes_processed, total_elapsed_time  
      FROM sys.dm_exec_dms_workers   
      WHERE execution_id = 'QID4547'   
      ORDER BY total_elapsed_time DESC;
      ```  

1. **Поиск сведений о внешних операциях DMS.**  

   Используйте идентификатор выполнения и индекс действия, записанные при выполнении предыдущих действий.

   ```sql  
   SELECT execution_id, step_index, dms_step_index, compute_node_id,   
   type, input_name, length, total_elapsed_time, status   
   FROM sys.dm_exec_external_work   
   WHERE execution_id = 'QID4547' and step_index = 7   
   ORDER BY total_elapsed_time DESC;  
   ```  

## <a name="to-view-the--polybase-query-plan-to-be-changed"></a>Просмотр плана запроса PolyBase (для изменения) 

1. В SSMS включите параметр **Включить действительный план выполнения** (CTRL+M) и выполните запрос.

2. Перейдите на вкладку **План выполнения** .

   ![План запроса PolyBase](../../relational-databases/polybase/media/polybase-query-plan.png "План запроса PolyBase")  

3. Щелкните правой кнопкой мыши оператор **Удаленный запрос** и выберите пункт **Свойства**.

4. Скопируйте и вставьте значение оператора "Удаленный запрос" в текстовый редактор, чтобы просмотреть план удаленного запроса в формате XML. Ниже приведен пример такого файла.

   ```xml  

   <dsql_query number_nodes="1" number_distributions="8" number_distributions_per_node="8">  
      <sql>ExecuteMemo explain query</sql>  
      <dsql_operations total_cost="0" total_number_operations="6">  
        <dsql_operation operation_type="RND_ID">  
          <identifier>TEMP_ID_74</identifier>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">CREATE TABLE [tempdb].[dbo].[TEMP_ID_74] ([SensorKey] INT NOT NULL, [CustomerKey] INT NOT NULL, [GeographyKey] INT, [Speed] FLOAT(53) NOT NULL, [YearMeasured] INT NOT NULL ) WITH(DATA_COMPRESSION=PAGE);</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">EXEC [tempdb].[sys].[sp_addextendedproperty] @name=N'IS_EXTERNAL_STREAMING_TABLE', @value=N'true', @level0type=N'SCHEMA', @level0name=N'dbo', @level1type=N'TABLE', @level1name=N'TEMP_ID_74'</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">UPDATE STATISTICS [tempdb].[dbo].[TEMP_ID_74] WITH ROWCOUNT = 2401, PAGECOUNT = 7</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
        <dsql_operation operation_type="MULTI">  
          <dsql_operation operation_type="STREAMING_RETURN">  
            <operation_cost cost="1" accumulative_cost="1" average_rowsize="24" output_rows="5762.1" />  
            <location distribution="AllDistributions" />  
            <select>SELECT [T1_1].[SensorKey] AS [SensorKey],  
             [T1_1].[CustomerKey] AS [CustomerKey],  
             [T1_1].[GeographyKey] AS [GeographyKey],  
             [T1_1].[Speed] AS [Speed],  
             [T1_1].[YearMeasured] AS [YearMeasured]  
      FROM   (SELECT [T2_1].[SensorKey] AS [SensorKey],  
                     [T2_1].[CustomerKey] AS [CustomerKey],  
                     [T2_1].[GeographyKey] AS [GeographyKey],  
                     [T2_1].[Speed] AS [Speed],  
                     [T2_1].[YearMeasured] AS [YearMeasured]  
              FROM   [tempdb].[dbo].[TEMP_ID_74] AS T2_1  
              WHERE  ([T2_1].[Speed] > CAST (6.50000000000000000E+001 AS FLOAT))) AS T1_1</select>  
          </dsql_operation>  
          <dsql_operation operation_type="ExternalRoundRobinMove">  
            <operation_cost cost="16.594848" accumulative_cost="17.594848" average_rowsize="24" output_rows="19207" />  
            <external_uri>hdfs://10.193.26.177:8020/Demo/car_sensordata.tbl/</external_uri>  
            <destination_table>[TEMP_ID_74]</destination_table>  
          </dsql_operation>  
        </dsql_operation>  
        <dsql_operation operation_type="ON">  
          <location permanent="false" distribution="AllDistributions" />  
          <sql_operations>  
            <sql_operation type="statement">DROP TABLE [tempdb].[dbo].[TEMP_ID_74]</sql_operation>  
          </sql_operations>  
        </dsql_operation>  
      </dsql_operations>  
   </dsql_query>  
   ```  

## <a name="to-monitor-nodes-in-a-polybase-group"></a>Мониторинг узлов в группе PolyBase  

После настройки нескольких компьютеров в рамках группы горизонтального увеличения масштаба PolyBase можно отслеживать состояние компьютеров. Дополнительные сведения о создании группы горизонтального увеличения масштаба см. в разделе [Группы горизонтального увеличения масштаба PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).

1. Подключитесь к SQL Server на головном узле группы.

2. Запустите [sys.dm_exec_compute_nodes (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md) динамического административного представления, чтобы просмотреть все узлы в группе PolyBase.

3. Запустите [sys.dm_exec_compute_node_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md) динамического административного представления, чтобы просмотреть состояние всех узлов в группе PolyBase.

## <a name="hadoop-name-node-high-availability"></a>Высокий уровень доступности узла имени Hadoop

Сегодня PolyBase не взаимодействует со службами высокой доступности узла имени, такими как Zookeeper или Knox. Однако есть проверенное решение, которое можно использовать для обеспечения функциональности.

Решение проблемы: использование DNS-имени для перенаправления соединений на активный узел имени. Для этого необходимо, чтобы для взаимодействия с узлом внешний источник данных использовал DNS-имя. При возникновении отработки отказа следует изменить IP-адрес, связанный с DNS-именем, используемым в определении внешнего источника данных. В результате все новые соединения будут перенаправляться на соответствующий узел имени. В случае отработки существующие подключения завершатся ошибкой. Чтобы автоматизировать этот процесс, периодический сигнал может проверить связь с активным узлом имени. Если периодический сигнал завершается ошибкой, можно предположить, что возникла отработка отказа, и автоматически переключиться на IP-адреса дополнительных серверов.

## <a name="error-messages-and-possible-solutions"></a>Сообщения об ошибках и возможные решения

Сведения об устранении ошибок с внешними таблицами см. в блоге Муршеда Замана (Murshed Zaman) [https://blogs.msdn.microsoft.com/sqlcat/2016/06/21/polybase-setup-errors-and-possible-solutions/](/archive/blogs/sqlcat/polybase-setup-errors-and-possible-solutions "Ошибки настройки PolyBase и возможные решения").

## <a name="see-also"></a>См. также раздел

[Устранение неполадок с подключением PolyBase к Kerberos](polybase-troubleshoot-connectivity.md)
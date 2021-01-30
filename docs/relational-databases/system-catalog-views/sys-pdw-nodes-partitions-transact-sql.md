---
description: sys.pdw_nodes_partitions (Transact-SQL)
title: sys.pdw_nodes_partitions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: b4216752-4813-4b2c-b259-7d8ffc6cc190
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: c4c6e3ce34f1fbd73bf046b9fdeb178d82878f6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207842"
---
# <a name="syspdw_nodes_partitions-transact-sql"></a>sys.pdw_nodes_partitions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Содержит по одной строке для каждой секции всех таблиц и большинства типов индексов в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] базе данных. Все таблицы и индексы содержат по крайней мере одну секцию, независимо от того, были ли они явно секционированы.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|Идентификатор секции. Уникален в базе данных.|  
|object_id|**int**|Идентификатор объекта, которому принадлежит данная секция. Каждая таблица или представление содержит как минимум одну секцию.|  
|index_id|**int**|Идентификатор индекса в пределах объекта, которому принадлежит данная секция.|  
|partition_number|**int**|Номер секции (начиная с 1) в индексе владельца или куче. Для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] значение этого столбца равно 1.|  
|hobt_id|**bigint**|Идентификатор куча или сбалансированное дерево данных (HoBT), который содержит строки для этой секции.|  
|rows|**bigint**|Приблизительное количество строк в данной секции. |  
|data_compression|**int**|Указывает состояние сжатия для каждой секции.<br /><br /> 0 = нет<br /><br /> 1 = ROW<br /><br /> 2 = PAGE<br /><br /> 3 = COLUMNSTORE|  
|data_compression_desc|**nvarchar(60)**|Указывает состояние сжатия для каждой секции. Возможными значениями являются NONE, ROW и PAGE.|  
|pdw_node_id|**int**|Уникальный идентификатор [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] узла.|  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение `CONTROL SERVER`.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

### <a name="example-a-display-rows-in-each-partition-within-each-distribution"></a>Пример а. отображение строк в каждой секции в каждом распределении 

**Применимо к:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
 
Чтобы отобразить количество строк в каждой секции в каждом распределении, используйте [инструкцию DBCC PDW_SHOWPARTITIONSTATS (SQL Server PDW)](../../t-sql/database-console-commands/dbcc-pdw-showpartitionstats-transact-sql.md) .

### <a name="example-b-uses-system-views-to-view-rows-in-each-partition-of-each-distribution-of-a-table"></a>Пример б. Использование системных представлений для просмотра строк в каждой секции каждого распределения таблицы

**Применимо к:** [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]
 
Этот запрос возвращает количество строк в каждой секции каждого распределения таблицы `myTable` .  
 
```sql  
SELECT o.name, pnp.index_id, pnp.partition_id, pnp.rows,   
    pnp.data_compression_desc, pnp.pdw_node_id  
FROM sys.pdw_nodes_partitions AS pnp  
JOIN sys.pdw_nodes_tables AS NTables  
    ON pnp.object_id = NTables.object_id  
AND pnp.pdw_node_id = NTables.pdw_node_id  
JOIN sys.pdw_table_mappings AS TMap  
    ON NTables.name = TMap.physical_name 
    AND substring(TMap.physical_name,40, 10) = pnp.distribution_id 
JOIN sys.objects AS o  
    ON TMap.object_id = o.object_id  
WHERE o.name = 'myTable'  
ORDER BY o.name, pnp.index_id, pnp.partition_id;  
```    
  
## <a name="see-also"></a>См. также:  
 [Представления каталога Azure Synapse Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  


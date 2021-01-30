---
description: sys.pdw_nodes_column_store_segments (Transact-SQL)
title: sys.pdw_nodes_column_store_segments (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 03/28/2018
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: e2fdf8e9-1b74-4682-b2d4-c62aca053d7f
author: julieMSFT
ms.author: jrasnick
manager: jrj
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 08e99e71c4086f60b869b9d25c6b0d0f4deec35d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186568"
---
# <a name="syspdw_nodes_column_store_segments-transact-sql"></a>sys.pdw_nodes_column_store_segments (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Содержит по одной строке для каждого столбца в индексе columnstore.

| Имя столбца                 | Тип данных  | Описание                                                  |
| :-------------------------- | :--------- | :----------------------------------------------------------- |
| **partition_id**            | **bigint** | Указывает идентификатор секции. Уникален в базе данных.     |
| **hobt_id**                 | **bigint** | Идентификатор кучи или индекс сбалансированного дерева (hobt) для таблицы, в которой содержится индекс columnstore. |
| **column_id**               | **int**    | Идентификатор столбца columnstore.                                |
| **segment_id**              | **int**    | Идентификатор сегмента столбца. Для обеспечения обратной совместимости имя столбца будет по-segment_id, несмотря на то, что это идентификатор группы строк. Можно однозначно идентифицировать сегмент с помощью <hobt_id, partition_id column_id>, <SEGMENT_ID>. |
| **version**                 | **int**    | Версия формата сегмента столбца.                        |
| **encoding_type**           | **int**    | Тип кодировки, используемой для этого сегмента:<br /><br /> 1 = VALUE_BASED-нестроковый/двоичный без словаря (аналогично 4 с некоторыми внутренними вариациями)<br /><br /> 2 = VALUE_HASH_BASED — нестроковый или двоичный столбец с общими значениями в словаре<br /><br /> 3 = STRING_HASH_BASED-строка/двоичный столбец с общими значениями в словаре<br /><br /> 4 = STORE_BY_VALUE_BASED-не строковый/двоичный без словаря<br /><br /> 5 = STRING_STORE_BY_VALUE_BASED-String/binary без словаря<br /><br /> Все кодировки по возможности используют преимущества битовой упаковки и длины выполнения. |
| **row_count**               | **int**    | Число строк в группе строк.                             |
| **has_nulls**               | **int**    | Значение 1, если сегмент столбца содержит значения NULL.                     |
| **base_id**                 | **bigint** | Идентификатор базового значения, если используется тип кодировки 1.  Если тип кодировки 1 не используется, base_id устанавливается в значение 1. |
| **magnitude**               | **float**  | Величина, если используется тип кодировки 1.  Если тип кодировки 1 не используется, то величина устанавливается равным 1. |
| **primary__dictionary_id**  | **int**    | Идентификатор основного словаря. Ненулевое значение указывает на локальный словарь для этого столбца в текущем сегменте (т. е. группы строк). Значение-1 указывает на отсутствие локального словаря для этого сегмента. |
| **secondary_dictionary_id** | **int**    | Идентификатор дополнительного словаря. Ненулевое значение указывает на локальный словарь для этого столбца в текущем сегменте (т. е. группы строк). Значение-1 указывает на отсутствие локального словаря для этого сегмента. |
| **min_data_id**             | **bigint** | Минимальный идентификатор данных в сегменте столбца.                       |
| **max_data_id**             | **bigint** | Максимальный идентификатор данных в сегменте столбца.                       |
| **null_value**              | **bigint** | Значение, используемое для представления значений NULL.                               |
| **on_disk_size**            | **bigint** | Размер сегмента в байтах.                                    |
| **pdw_node_id**             | **int**    | Уникальный идентификатор [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] узла. |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Соединение sys.pdw_nodes_column_store_segments с другими системными таблицами для определения количества сегментов columnstore на логическую таблицу.

```sql
SELECT  sm.name           as schema_nm
,       tb.name           as table_nm
,       nc.name           as col_nm
,       nc.column_id
,       COUNT(*)          as segment_count
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb                   ON  sm.[schema_id]          = tb.[schema_id]
JOIN    sys.[pdw_table_mappings] mp       ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt         ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[pdw_nodes_partitions] np     ON  np.[object_id]          = nt.[object_id]
                                          AND np.[pdw_node_id]        = nt.[pdw_node_id]
                                          AND np.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_nodes_columns] nc        ON  np.[object_id]          = nc.[object_id]
                                          AND np.[pdw_node_id]        = nc.[pdw_node_id]
                                          AND np.[distribution_id]    = nc.[distribution_id]
JOIN    sys.[pdw_nodes_column_store_segments] rg  ON  rg.[partition_id]         = np.[partition_id]
                                                      AND rg.[pdw_node_id]      = np.[pdw_node_id]
                                                      AND rg.[distribution_id]  = np.[distribution_id]
                                                      AND rg.[column_id]        = nc.[column_id]
GROUP BY    sm.name
,           tb.name
,           nc.name
,           nc.column_id  
ORDER BY    table_nm
,           nc.column_id
,           sm.name ;
```

## <a name="permissions"></a>Разрешения

Необходимо разрешение **VIEW SERVER STATE**.

## <a name="see-also"></a>См. также:

[Представления каталога Azure Synapse Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
[CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)  
[sys.pdw_nodes_column_store_row_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-row-groups-transact-sql.md)  
[sys.pdw_nodes_column_store_dictionaries &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-nodes-column-store-dictionaries-transact-sql.md)

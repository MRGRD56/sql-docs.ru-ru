---
description: sys.pdw_materialized_view_mappings (Transact-SQL)
title: sys.pdw_materialized_view_mappings (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: d62b0e25-3226-4f87-a10a-b3a0d9555e19
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 13b058b198f48790a8a9359955141a8d1458a401
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210493"
---
# <a name="syspdw_materialized_view_mappings-transact-sql"></a>sys.pdw_materialized_view_mappings (Transact-SQL)  

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Привязывает материализованный представление к внутренним именам объектов, используя object_id.

Столбцы physical_name и object_id формируют ключ для этого представления каталога.
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|physical_name |**nvarchar (36)**|Физическое имя материализованных представлений.|  
|object_id  |**int**|Идентификатор объекта для материализованных представлений. См. раздел [sys. Objects (Transact-SQL)](./sys-objects-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).| 

## <a name="permissions"></a>Разрешения

Необходимо разрешение VIEW DATABASE STATE.
  
## <a name="see-also"></a>См. также раздел

[Настройка производительности с помощью материализованного представления](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[EXPLAIN &#40;Transact-SQL&#41;](../../t-sql/queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](./sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](./sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-pdw-showmaterializedviewoverhead-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Представления каталога Azure Synapse Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Системные представления, поддерживаемые в Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Инструкции T-SQL, поддерживаемые в Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)
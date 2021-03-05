---
title: sys.pdw_permanent_table_mappings (Transact-SQL)
description: Привязывает постоянные пользовательские таблицы к именам внутренних объектов с помощью **object_id**.
ms.custom: ''
ms.date: 07/24/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
author: mstehrani
ms.author: emtehran
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: ab6ec23c35f9766a82e9a0c07f31433b2cbbc2ce
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186594"
---
# <a name="syspdw_permanent_table_mappings-transact-sql"></a>sys.pdw_permanent_table_mappings (Transact-SQL)
[!INCLUDE [applies-to-version/asa](../../includes/applies-to-version/asa.md)]

Привязывает постоянные пользовательские таблицы к именам внутренних объектов с помощью **object_id**.  
  
> [!NOTE]
> **sys.pdw_permanent_table_mappings** содержит сопоставления с постоянными таблицами и не включает сопоставления временных или внешних таблиц.

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|physical_name|**nvarchar (36)**|Физическое имя таблицы.<br /><br /> **physical_name** и **object_id** формируют ключ для этого представления.||  
|object_id|**int**|Идентификатор объекта для таблицы. См. статью [sys. objects &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).<br /><br /> **physical_name** и **object_id** формируют ключ для этого представления.||  
  
## <a name="see-also"></a>См. также  
 [Представления каталога Azure Synapse Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.pdw_index_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-index-mappings-transact-sql.md)  
  
  

---
description: sys.workload_management_workload_groups (Transact-SQL)
title: sys.workload_management_workload_groups (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: system-objects
ms.prod_service: synapse-analytics
ms.reviewer: jrasnick
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 0a8f145be38259f5f3a600cb7a270ccf193b4672
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747754"
---
# <a name="sysworkload_management_workload_groups-transact-sql"></a>sys.workload_management_workload_groups (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

 Возвращает сведения о группах рабочей нагрузки.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|
|group_id|**int**|Уникальный идентификатор группы рабочей нагрузки. Не допускает значение NULL.||
|name|**sysname**|Имя группы рабочей нагрузки. Должен быть уникальным для экземпляра.  Не допускает значение NULL.||
|importance|**nvarchar(128)**|— Это относительная важность запроса в этой группе рабочей нагрузки и в группах рабочей нагрузки для общих ресурсов. Не допускает значение NULL.|Low, below_normal, обычная (по умолчанию), above_normal, High||
|min_percentage_resource|**tinyint**|Гарантированный объем ресурсов для запросов в группе рабочей нагрузки. Ресурсы не используются совместно с другими группами рабочей нагрузки. Не допускает значение NULL.||
|cap_percentage_resource|**tinyint**|Жесткое ограничение выделения ресурсов в процентах для запросов в группе рабочей нагрузки. Ограничивает максимальное количество ресурсов, выделенных для указанного уровня. Диапазон допустимых значений — от 1 до 100.||
|request_min_resource_grant_percent|**Decimal (5, 2)**|Указывает минимальный объем ресурсов, выделенных запросу. Допустимый диапазон значений — от 0,75 до 100.||
|request_max_resource_grant_percent |**Decimal (5, 2)**|Указывает максимальный объем ресурсов, выделенных для запроса.||
|query_execution_timeout_sec|**int**|Время выполнения в секундах, которое было разрешено до отмены запроса.  Запросы нельзя отменить после того, как они достигли фазы возврата выполнения.  query_execution_timeout_sec не включает время, затраченное на очередь.|
|query_wait_timeout_sec|**int**|INTERNAL||
|create_time|**datetime**|Время создания группы рабочей нагрузки. Не допускает значение NULL.||
modify_time|**datetime**|Время последнего изменения группы рабочей нагрузки. Не допускает значение NULL.||
|&nbsp;||||
  
## <a name="permissions"></a>Разрешения

Необходимо разрешение VIEW SERVER STATE.

## <a name="next-steps"></a>Следующие шаги

 Список всех представлений каталога для Azure синапсе Analytics и параллельного хранилища данных см. в статье [представления каталога Azure синапсе Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md). Сведения о создании группы рабочей нагрузки см. в разделе [Создание группы рабочей нагрузки](../../t-sql/statements/create-workload-group-transact-sql.md). Дополнительные сведения о классификации рабочей нагрузки см. в разделе [изоляция рабочей нагрузки](/azure/sql-data-warehouse/sql-data-warehouse-workload-isolation) .

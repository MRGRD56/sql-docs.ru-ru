---
description: sys.workload_management_workload_classifiers (Transact-SQL)
title: sys.workload_management_workload_classifiers (Transact-SQL) | Документация Майкрософт
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
ms.openlocfilehash: a466c431a422eee3a58bd62e8cb2a0d19947bd7b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747774"
---
# <a name="sysworkload_management_workload_classifiers-transact-sql"></a>sys.workload_management_workload_classifiers (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

 Возвращает сведения для классификаторов рабочей нагрузки.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|
|classifier_id|**int**|Уникальный идентификатор классификатора. Не допускает значение NULL.||
group_name|**sysname**|Имя группы рабочей нагрузки, которой назначен классификатор. Не допускает значение NULL. Присоединение к sys.workload_management_workload_groups ||
name|**sysname**|Имя классификатора. Должен быть уникальным для экземпляра. Не допускает значение NULL.||
|importance|**sysname**|— Это относительная важность запроса в этой группе рабочей нагрузки и в группах рабочей нагрузки для общих ресурсов.  Важность, указанная в классификаторе, переопределяет параметр важности группы рабочей нагрузки. Допускает значение NULL.  При значении NULL используется параметр важности группы рабочей нагрузки.|Low, below_normal, обычная (по умолчанию), above_normal, High |
|create_time|**datetime**|Время создания классификатора. Не допускает значение NULL.||
modify_time|**datetime**|Время последнего изменения классификатора. Не допускает значение NULL.||
is_enabled|**bit**|INTERNAL||
|&nbsp;||||
  
## <a name="permissions"></a>Разрешения

Необходимо разрешение VIEW SERVER STATE.

## <a name="next-steps"></a>Следующие шаги

 Список всех представлений каталога для Azure синапсе Analytics и параллельного хранилища данных см. в статье [представления каталога Azure синапсе Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md). Сведения о создании классификатора рабочей нагрузки см. в разделе [Создание классификатора рабочей нагрузки](../../t-sql/statements/create-workload-classifier-transact-sql.md). Дополнительные сведения о классификации рабочей нагрузки см. в разделе [классификация рабочей нагрузки](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification) .

---
description: sys.dm_pdw_component_health_active_alerts (Transact-SQL)
title: sys.dm_pdw_component_health_active_alerts (Transact-SQL
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c53e4a36-b841-424a-b8e2-255b1878deb6
author: markingmyname
ms.author: maghan
monikerRange: '>= aps-pdw-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 2bc90e6d3526b6b1727d8bc43263b21c34b88106
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92035447"
---
# <a name="sysdm_pdw_component_health_active_alerts-transact-sql"></a>sys.dm_pdw_component_health_active_alerts (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Хранит активные предупреждения о [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] компонентах.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Уникальный идентификатор [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] узла.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id и alert_instance_id образуют ключ для этого представления.|NOT NULL|  
|component_id|**int**|Идентификатор компонента. См. раздел [sys.pdw_health_components &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id и alert_instance_id образуют ключ для этого представления.|NOT NULL|  
|component_instance_id|**nvarchar(255)**|pdw_node_id, component_id, component_instance_id, alert_id и alert_instance_id образуют ключ для этого представления.|NOT NULL|  
|alert_id|**int**|Идентификатор для типа оповещения. См. раздел [sys.pdw_health_alerts &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md).<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id и alert_instance_id образуют ключ для этого представления.|NOT NULL|  
|alert_instance_id|**nvarchar (36)**|Определяет экземпляр данного предупреждения.<br /><br /> pdw_node_id, component_id, component_instance_id, alert_id и alert_instance_id образуют ключ для этого представления.|NOT NULL|  
|current_value|**nvarchar(255)**|Используется, если оповещение имеет тип StatusChange. Это текущее состояние компонента. Значение равно NULL для предупреждений типа threshold. Список типов оповещений см. в разделе [sys.pdw_health_alerts &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|previous_value|**nvarchar(255)**|Используется, если оповещение имеет тип StatusChange. Это состояние предыдущего компонента. Значение равно NULL для предупреждений типа threshold. Список типов оповещений см. в разделе [sys.pdw_health_alerts &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-alerts-transact-sql.md) .|NULL|  
|create_time|**datetime**|Время и Дата создания предупреждения.|NOT NULL|  
  
 Сведения о максимальном значении строк, хранящихся в этом представлении, см. в разделе "минимальное и максимальное значения" в [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления Azure синапсе Analytics и Параллельное хранилище данных &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

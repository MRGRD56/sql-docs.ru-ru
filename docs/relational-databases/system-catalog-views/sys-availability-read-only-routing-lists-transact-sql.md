---
description: sys.availability_read_only_routing_lists (Transact-SQL)
title: sys.availability_read_only_routing_lists (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_read_only_routing_lists_TSQL
- availability_read_only_routing_lists
- sys.availability_read_only_routing_lists
- sys.availability_read_only_routing_lists_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- read-only routing
- Availability Groups [SQL Server], readable secondary replicas
- Availability Groups [SQL Server], read-only routing
- readable secondary replicas
- sys.availability_read_only_routing_lists dynamic management view
ms.assetid: 0686bc5a-c206-41ef-b40a-79a8259d51d2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f38f8ea25b94097da9bcfd2fca272f9ad9030d77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183658"
---
# <a name="sysavailability_read_only_routing_lists-transact-sql"></a>sys.availability_read_only_routing_lists (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает строку для списка маршрутизации только для чтения для каждой реплики доступности в группе доступности AlwaysOn в отказоустойчивом кластере WSFC.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Уникальный идентификатор реплики доступности, которой принадлежит список маршрутизации.|  
|**routing_priority**|**int**|Порядок приоритетов для маршрутизации (1 — первый, 2 — второй и т. д.).|  
|**read_only_replica_id**|**uniqueidentifier**|Уникальный идентификатор реплики доступности, на которую будет направлена рабочая нагрузка только для чтения.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Always On динамические административные представления и функции групп доступности &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [Представления каталога групп доступности AlwaysOn (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [Мониторинг групп доступности &#40;&#41;Transact-SQL ](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  

---
description: sys.dm_pdw_os_performance_counters (Transact-SQL)
title: sys.dm_pdw_os_performance_counters (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0673a8f8-8bed-41eb-8959-a9e3e9e03a65
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: ea7840aa8514bd3bdbc03004e551c66ad01944fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99140140"
---
# <a name="sysdm_pdw_os_performance_counters-transact-sql"></a>sys.dm_pdw_os_performance_counters (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Содержит сведения о счетчиках производительности Windows для узлов в [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Идентификатор узла, содержащего счетчик.<br /><br /> pdw_node_id и counter_name формируют ключ для этого представления.|См. node_id в [sys.dm_pdw_nodes &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-transact-sql.md).|  
|counter_name|**nvarchar(255)**|Имя счетчика производительности Windows.||  
|counter_category|**nvarchar(255)**|Имя категории счетчика производительности Windows.||  
|имя_экземпляра|**nvarchar(255)**|Имя заданного экземпляра счетчика.||  
|counter_value|**Decimal (38, 10)**|Текущее значение счетчика.||  
|last_update_time|**Datetime2 (3)**|Метка времени последнего времени обновления значения.||  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления Azure синапсе Analytics и Параллельное хранилище данных &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

---
description: catalog.execution_component_phases
title: catalog.execution_component_phases | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 07a9a163-4787-40f7-b371-ac5c6cb4b095
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d3d9c223a4d415b9bfd28ed4c202570141c1f1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347372"
---
# <a name="catalogexecution_component_phases"></a>catalog.execution_component_phases 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Отображает время, затраченное компонентом потока данных на каждом этапе выполнения.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|phase_stats_id|**bigint**|Уникальный идентификатор (ID) этапа.|  
|execution_id|**bigint**|Уникальный идентификатор для экземпляра исполнения.|  
|package_name|**nvarchar(260)**|Имя первого пакета, запущенного во время выполнения.|  
|task_name|**nvarchar(4000)**|Имя задачи потока данных.|  
|subcomponent_name|**nvarchar(4000)**|Имя компонента потока данных.|  
|этап|**nvarchar(128)**|Имя этапа выполнения.|  
|start_time|**datetimeoffset(7)**|Время начала этапа.|  
|end_time|**datetimeoffset(7)**|Время окончания этапа.|  
|execution_path|**nvarchar(max)**|Путь выполнения задачи потока данных.|  
  
## <a name="remarks"></a>Remarks  
 Это представление отображает строку для каждого этапа выполнения компонента потока данных, такого как Validate, Pre-Execute, Post-Execute, PrimeOutput и ProcessInput. Каждая строка отображает время начала и окончания для конкретного этапа выполнения.  
  
## <a name="example"></a>Пример  
 В следующем примере используется представление catalog.execution_component_phases для нахождения общего времени, затраченного пакетом на выполнение всех фаз (**active_time**), и полное затраченное время на пакет (**total_time**).  
  
> [!WARNING]  
>  Представление catalog.execution_component_phases предоставляет эти сведения, если в качестве уровня ведения журнала выполнения пакетов задано значение Performance или Verbose. Дополнительные сведения см. в разделе [Включение ведения журналов при выполнении пакета на сервере служб SSIS](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
  
```sql
use SSISDB  
select package_name, task_name, subcomponent_name, execution_path,  
    SUM(DATEDIFF(ms,start_time,end_time)) as active_time,  
    DATEDIFF(ms,min(start_time), max(end_time)) as total_time  
from catalog.execution_component_phases  
where execution_id = 1841  
group by package_name, task_name, subcomponent_name, execution_path  
order by package_name, task_name, subcomponent_name, execution_path  
```  
  
## <a name="permissions"></a>Разрешения  
 Это представление требует применения одного из следующих разрешений:  
  
-   Разрешение READ на экземпляр выполнения  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
> [!NOTE]  
>  Наличие разрешения на выполнение операции на сервере подразумевает наличие разрешения на просмотр сведений об этой операции. Действует защита на уровне строки. Отображаются только строки, на которые у вас имеется разрешение.  
  
  

---
title: Реплике не назначена допустимая роль для группы доступности
description: Определение возможных причин, по которым реплике доступности может быть не назначена допустимая роль для группы доступности Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp1rolehealthy.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: ebb2c9f4-2097-4688-b4fb-8f0571047317
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9d5257ddbdb4e5506339fa2047fe42bf7c92a668
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584656"
---
# <a name="availability-replica-does-not-have-a-healthy-role-for-an-always-on-availability-group"></a>Реплике доступности не назначена допустимая роль для группы доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Введение  
  
|||  
|-|-|  
|**Имя политики**|Состояние роли реплики доступности|  
|**Проблема**|Реплике доступности не назначена допустимая роль.|  
|**Категория**|**Критическая**|  
|**Аспект**|Реплика доступности|  
  
## <a name="description"></a>Description  
 Эта политика проверяет состояние роли реплики доступности. Политика находится в состоянии неисправности, если роль реплики доступности не является первичной или вторичной. В остальном политика находится в рабочем состоянии.  
  
> [!NOTE]  
>  Для этого выпуска [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]сведения о возможных причинах проблемы и ее решении доступны в разделе [Реплика доступности не имеет исправной роли](https://go.microsoft.com/fwlink/p/?LinkId=220856) в TechNet Wiki.  
  
## <a name="possible-causes"></a>Возможные причины  
 Роль этой реплики доступности неисправна. Реплике не назначена роль первичной или вторичной.  
  
## <a name="possible-solution-information_still_to_come"></a>Возможное решение: Information_still_to_come  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

---
title: База данных-получатель не присоединена | Документы Майкрософт
description: Политика "Состояние присоединения базы данных доступности" проверяет состояние присоединения для базы данных-получателя в рамках основанной на политиках системы управления группами доступности Always On.
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.drp2joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 10817e5e-75fa-42dd-baa2-359bea3ad051
author: cawrites
ms.author: chadam
ms.openlocfilehash: 39c9f6852fd1a1cc15a014b67ee693a3cb315787
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344507"
---
# <a name="secondary-database-is-not-joined"></a>База данных-получатель не подключена
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Введение  
  
|||  
|-|-|  
|**Имя политики**|Состояние присоединения базы данных доступности|  
|**Проблема**|База данных-получатель не присоединена.|  
|**Категория**|**Предупреждение**|  
|**Аспект**|База данных доступности|  
  
## <a name="description"></a>Описание  
 Эта политика проверяет состояние соединения данных базы данных-получателя (которая также называется «реплика базы данных-получателя»). Эта политика находится в нерабочем состоянии, если реплика набора данных не присоединена. В остальном политика находится в рабочем состоянии.  

## <a name="possible-causes"></a>Возможные причины  
 Эта база данных-получателя не присоединена к группе доступности. Конфигурация этой базы данных-получателя является неполной.  
  
## <a name="possible-solution"></a>Возможное решение  
 Используйте Transact-SQL, PowerShell или среду SQL Server Management Studio для присоединения вторичной реплики к группе доступности. Дополнительные сведения о присоединении вторичных реплик к группам доступности приведены в разделе [Присоединение вторичной реплики к группе доступности (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx).  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

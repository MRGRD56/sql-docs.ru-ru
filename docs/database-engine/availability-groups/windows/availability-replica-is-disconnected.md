---
title: Реплика доступности не соединена с группой доступности
description: Определение возможных причин, по которым реплика не связана с группой доступности Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: troubleshooting
f1_keywords:
- sql13.swb.agdashboard.arp2connected.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 1a2162d3-54fb-4356-b349-effbdc15a5a4
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71324a6bfc03bbb75979391ac22bd0381938b54f
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633134"
---
# <a name="availability-replica-is-disconnected-within-an-always-on-availability-group"></a>Реплика доступности не соединена с группой доступности Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>Введение  
  
- **Имя политики** Состояние соединения реплики доступности
- **Проблема** Реплика доступности отключена.
- **Категория** **Критически важное**
- **Аспект** Реплика доступности  
  
## <a name="description"></a>Описание  
 Эта политика проверяет состояние соединения между репликами доступности. Эта политика находится в нерабочем состоянии, если подключение к реплике доступности имеет состояние DISCONNECTED. В остальном политика находится в рабочем состоянии.  
  
## <a name="possible-causes"></a>Возможные причины  
 Вторичная реплика не подключена к первичной реплике. Состояние соединения — DISCONNECTED. Возможны следующие причины этой проблемы.  
  
-   Порт подключения может конфликтовать с другим приложением.  
  
-   Несоответствие типа или алгоритма шифрования.  
  
-   Конечная точка подключения была удалена или не была запущена.  
  
-   Транспорт отключен.  
  
## <a name="possible-solutions"></a>Возможные решения  
 Ниже перечислены возможные решения этой проблемы:  
  
-   Проверьте конфигурацию конечной точки зеркального отображения базы данных для экземпляров первичной и вторичной реплики и обновите конфигурацию с несоответствием.  
  
-   Проверьте наличие конфликта портов и при его обнаружении измените номер порта.  
  
## <a name="see-also"></a>См. также:  
 [Обзор групп доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

---
title: SQL Server XTP Phantom Processor | Документация Майкрософт
description: Сведения об объекте производительности SQL Server XTP Phantom Processor, который содержит счетчики, относящиеся к подсистеме обработки фантомов механизма In-Memory OLTP.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 0f691b3d-a8fd-4459-ad21-2cfc8574a8c0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c0547f8f93b548e7dcf51271e7c796bad52f46ad
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505478"
---
# <a name="sql-server-xtp-phantom-processor"></a>SQL Server XTP Phantom Processor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Объект производительности SQL Server XTP Phantom Processor содержит счетчики, относящиеся к подсистеме обработки фантомов механизма In-Memory OLTP. Этот компонент отвечает за обнаружение фантомных строк в транзакциях, выполняемых на уровне изоляции SERIALIZABLE, а также за контроль соблюдения ограничений в параллельных сценариях.  
  
 В этой таблице описываются счетчики объекта **SQL Server XTP Phantom Processor** .  
  
|Счетчик|Описание|  
|-------------|-----------------|  
|**Число попыток сканирования «пыльных углов»/с (от Phantom)**|Число повторных попыток сканирования из-за конфликтов записи при обработке «пыльных углов», выданное Phantom Processor (в среднем), в секунду Это счетчик очень низкого уровня, не предназначенный для пользователей.|  
|**Удалено истекших фантомных строк/с**|Число просроченных строк, удаленных фантомными сканированиями (в среднем), в секунду.|  
|**Количество затронутых фантомных просроченных строк/с**|Число просроченных строк, затронутых фантомными сканированиями (в среднем), в секунду.|  
|**Количество затронутых фантомных строк с истекающим сроком действия /с**|Число строк с истекающим сроком действия, затронутых фантомными сканированиями (в среднем), в секунду.|  
|**Затронутых фантомных строк/с**|Число строк, затронутых фантомными сканированиями (в среднем), в секунду.|  
|**Запущено фантомных сканирований/с**|Число начатых фантомных сканирований (в среднем), в секунду.|  
  
## <a name="see-also"></a>См. также:  
 [Счетчики производительности XTP (In-Memory OLTP) для SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  

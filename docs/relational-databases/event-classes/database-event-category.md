---
description: Категория событий Database
title: Категория событий Database | Документация Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- event classes [SQL Server], Database event category
- Database event category [SQL Server]
- SQL Server event classes, Database event category
ms.assetid: b61af738-f144-4992-b0b2-d44cb7240991
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5f13acae75ca7bb45efd20b940435ccfd6d1a299
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178218"
---
# <a name="database-event-category"></a>Категория событий Database
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  В категории событий **База данных** содержатся классы событий для контроля компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
## <a name="in-this-section"></a>в этом разделе  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Класс событий Data File Auto Grow](../../relational-databases/event-classes/data-file-auto-grow-event-class.md)|Указывает на то, что размер файла данных увеличивается автоматически. Это событие не происходит при явном увеличении файла данных с помощью инструкции ALTER DATABASE.|  
|[Класс событий Data File Auto Shrink](../../relational-databases/event-classes/data-file-auto-shrink-event-class.md)|Указывает на то, что файл данных был сжат.|  
|[Соединение зеркального отображения базы данных, класс событий](../../relational-databases/event-classes/database-mirroring-connection-event-class.md)|Событие, формируемое для уведомления о состоянии транспортного соединения для зеркального отображения базы данных.|  
|[Класс событий Database Mirroring State Change](../../relational-databases/event-classes/database-mirroring-state-change-event-class.md)|Указывает, когда изменяется состояние зеркальной базы данных.|  
|[Database Suspect Data Page, класс событий](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)|Указывает, что страница добавлена в таблицу **suspect_pages** базы данных **msdb** .|  
|[Класс событий Log File Auto Grow](../../relational-databases/event-classes/log-file-auto-grow-event-class.md)|Указывает на то, что размер файла журнала был сжат автоматически. Это событие не происходит при явном увеличении файла журнала с помощью инструкции ALTER DATABASE.|  
|[Класс событий Log File Auto Shrink](../../relational-databases/event-classes/log-file-auto-shrink-event-class.md)|Указывает на то, что размер файла журнала был сжат автоматически. Это событие не происходит при явном сжатии файла журнала с помощью инструкции ALTER DATABASE.|  
  
## <a name="see-also"></a>См. также  
 [Расширенные события](../../relational-databases/extended-events/extended-events.md)  
  
  

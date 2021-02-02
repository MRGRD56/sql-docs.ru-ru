---
description: класс событий CPU Threshold Exceeded
title: Класс событий CPU Threshold Exceeded | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- CPU Threshold Exceeded Event Class
ms.assetid: eb106f7d-baa3-4a2b-96b2-f9fe0844057d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 35672e1d5a47fd3ed5a253e0c5b7e9a91043bed8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99110638"
---
# <a name="cpu-threshold-exceeded-event-class"></a>класс событий CPU Threshold Exceeded
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Класс событий CPU Threshold Exceeded указывает, что регулятор ресурсов обнаружил запрос, превышающий пороговое значение ЦП, заданное параметром REQUEST_MAX_CPU_TIME_SEC.  
  
> [!NOTE]  
>  Интервал обнаружения для этого события — пять секунд. Это событие гарантированно создается в том случае, если запрос превышает указанное ограничение в течение более чем пяти секунд. Однако, если запрос превышает указанное пороговое значение в течение меньшего времени, его обнаружение может быть пропущено в зависимости от времени поступления запроса и времени последнего прохода.  
  
## <a name="cpu-threshold-exceeded-data-columns"></a>Столбцы данных класса событий CPU Threshold Exceeded  
  
|Имя столбца данных|Тип данных|Описание|Идентификатор столбца|Фильтруемый|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ЦП|**int**|Использование ЦП в миллисекундах.|18|Да|  
|EventClass|**int**|214|27|Нет|  
|EventSubClass|**int**|Нарушение ограничения ЦП.|21|Да|  
|GroupID|**int**|Идентификатор группы, в которой произошло нарушение.|66|Да|  
|OwnerID|**int**|Идентификатор SPID процесса, который вызвал нарушение.|58|Да|  
|SPID|**int**|Идентификатор процесса сервера, запустившего данное событие.<br /><br /> Примечание. Он может отличаться от действительного пользовательского идентификатора SPID, если в потоке операционной системы проверка использования ЦП является фоновой задачей.|12|Да|  
|StartTime|**datetime**|Время срабатывания события.|14|Да|  
  
## <a name="see-also"></a>См. также:  
 [Хранимая процедура sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  

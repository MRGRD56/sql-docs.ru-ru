---
description: Server Memory Change, класс событий
title: Класс событий Server Memory Change | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Server Memory Change event class
ms.assetid: c9836484-39c5-4a89-b080-3567783b6fff
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 86f8e8a10a7207b75419a14b5b575a57297d9d81
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88329941"
---
# <a name="server-memory-change-event-class"></a>Server Memory Change, класс событий
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Событие класса **Server Memory Change** происходит, когда использование памяти  экземпляром [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] увеличивается или уменьшается на 1 МБ или на 5 % от максимального объема памяти сервера в зависимости от того, что больше.  
  
## <a name="server-memory-change-event-class-data-columns"></a>Столбцы данных класса событий Server Memory Change  
  
|Имя столбца данных|Тип данных|Описание|Идентификатор столбца|Да|  
|----------------------|---------------|-----------------|---------------|---------|  
|**EventClass**|**int**|Тип события = 81.|27|Нет|  
|**EventSequence**|**int**|Последовательность данного события в запросе.|51|Нет|  
|**EventSubClass**|**int**|Тип подкласса события.<br /><br /> 1 = увеличение памяти<br /><br /> 2 = уменьшение памяти|21|Да|  
|**IntegerData**|**int**|Новый объем памяти в мегабайтах (МБ).|25|Да|  
|**IsSystem**|**int**|Указывает, произошло событие в системном или в пользовательском процессе. 1 = системный, 0 = пользовательский.|60|Да|  
|**RequestID**|**int**|Идентификатор запроса, содержащего инструкцию.|49|Да|  
|**ServerName**|**nvarchar**|Имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , для которого производится трассировка.|26|Нет|  
|**SessionLoginName**|**nvarchar**|Имя входа пользователя, который инициировал сеанс. Например, при подключении к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по имени "Имя_входа1" и при выполнении инструкции под именем "Имя_входа2" **SessionLoginName** содержит значение "Имя_входа1", а **LoginName** — значение "Имя_входа2". В этом столбце отображаются как имена входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , так и имена входа Windows.|64|Да|  
|**SPID**|**int**|Идентификатор сеанса, в котором произошло событие.|12|Да|  
|**StartTime**|**datetime**|Время начала события, если оно известно.|14|Да|  
|**TransactionID**|**bigint**|Назначенный системой идентификатор транзакции.|4|Да|  
|**XactSequence**|**bigint**|Токен, который описывает текущую транзакцию.|50|Да|  
  
## <a name="see-also"></a>См. также:  
 [Расширенные события](../../relational-databases/extended-events/extended-events.md)   
 [Хранимая процедура sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Параметры конфигурации сервера «Server Memory»](../../database-engine/configure-windows/server-memory-server-configuration-options.md)  
  
  

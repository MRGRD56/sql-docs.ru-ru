---
title: Эквиваленты расширенных событий для классов событий трассировки SQL
description: В этой статье вы узнаете, как просматривать события и действия расширенных событий, аналогичных каждому событию трассировки SQL со связанными столбцами.
ms.date: 03/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- SQL Trace, extended events equivalents
- extended events [SQL Server], SQL Trace equivalents
- extended events [SQL Server], user configurable events
ms.assetid: 7f24104c-201d-4361-9759-f78a27936011
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4a139d6b8720d77b766ba317b08c08e9b16e830d
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489963"
---
# <a name="view-the-extended-events-equivalents-to-sql-trace-event-classes"></a>Просмотр эквивалентов расширенных событий для классов событий трассировки SQL

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/applies-to-version/sql-asdb.md)]

  Если требуется с помощью расширенных событий выполнять сбор данных о событиях, эквивалентных классам и столбцам событий трассировки SQL, желательно иметь представление о том, как события SQL-трассировки сопоставляются с событиями и действиями расширенных событий.  
  
 С помощью приведенной ниже процедуры можно просматривать события и действия расширенных событий, аналогичных каждому событию трассировки SQL со связанными столбцами.  
  
## <a name="to-view-the-extended-events-equivalents-to-sql-trace-events-using-query-editor"></a>Просмотр эквивалентов расширенных событий для событий трассировки SQL с помощью редактора запросов

- В редакторе запросов среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]выполните следующий запрос:

   ```sql
   USE MASTER;
   GO
   SELECT DISTINCT
      tb.trace_event_id,
      te.name            AS 'Event Class',
      em.package_name    AS 'Package',
      em.xe_event_name   AS 'XEvent Name',
      tb.trace_column_id,
      tc.name            AS 'SQL Trace Column',
      am.xe_action_name  AS 'Extended Events action'
   FROM
                sys.trace_events         te
      LEFT JOIN sys.trace_xe_event_map   em ON te.trace_event_id  = em.trace_event_id
      LEFT JOIN sys.trace_event_bindings tb ON em.trace_event_id  = tb.trace_event_id
      LEFT JOIN sys.trace_columns        tc ON tb.trace_column_id = tc.trace_column_id
      LEFT JOIN sys.trace_xe_action_map  am ON tc.trace_column_id = am.trace_column_id
   ORDER BY te.name, tc.name
   ```

При просмотре результатов обратите внимание на следующее.  

- Если все столбцы, кроме столбца класса событий, возвращают значение NULL, значит, данный класс событий не был перенесен из трассировки SQL.  
  
-   Если единственным значением, содержащимся в столбце действий расширенных событий, является NULL, значит, выполняется одно из следующих условий.  
  
    -   Столбец трассировки SQL сопоставлен с одним из полей данных, связанных с событием расширенных событий.  
  
        > [!NOTE]  
        > Каждое событие расширенных событий имеет набор полей данных по умолчанию, автоматически включаемых в результирующий набор.  
  
    -   Столбец действий не имеет эквивалента в расширенных событиях. Примером этого является столбец класса событий в трассировке SQL. Этот столбец не нужен в расширенных событиях, поскольку достаточно имени события.  
  
-   Что касается настраиваемых пользователем классов событий трассировки SQL (с UserConfigurable:1 по UserConfigurable:9), в расширенных событиях вместо них используется одно событие. Имя события — user_event. Это событие вызывается с помощью хранимой процедуры sp_trace_generateevent, которая используется и в трассировке SQL. Событие user_event возвращается независимо от того, какой идентификатор события передается хранимой процедуре. Однако поле event_id возвращается вместе с другими данными о событии. Это позволяет составлять предикат на основе идентификатора события. Например, если в коде используется UserConfigurable:0 (идентификатор события = 82), то можно добавить событие user_event в сеанс и указать предикат "event_id = 82". Таким образом, нет необходимости менять код, поскольку хранимая процедура sp_trace_generateevent формирует событие расширенных событий user_event и эквивалентный класс событий трассировки SQL.  
  
## <a name="see-also"></a>См. также  
 [Хранимая процедура sp_trace_generateevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)  
  
  

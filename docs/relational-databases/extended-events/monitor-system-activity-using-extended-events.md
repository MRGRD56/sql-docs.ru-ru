---
title: отслеживать активность системы с помощью расширенных событий
description: Используйте расширенные события совместно со средством трассировки событий для Windows (ETW) для наблюдения за активностью системы. Узнайте больше об инструкциях CREATE EVENT SESSION, ALTER EVENT SESSION и DROP EVENT SESSION.
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- xe
- extended events [SQL Server], monitoring system activity
ms.assetid: d83ad88f-818c-49fe-a9a9-299f704fca53
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 89277789f725bd78ea62ac0c91fd880d6e635950
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489980"
---
# <a name="monitor-system-activity-using-extended-events"></a>отслеживать активность системы с помощью расширенных событий

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Эта процедура показывает, как использовать расширенные события совместно со средством отслеживания событий для Windows (ETW) в целях мониторинга активности системы. Процедура показывает также варианты использования инструкций CREATE EVENT SESSION, ALTER EVENT SESSION и DROP EVENT SESSION.  
  
 Решение этих задач предполагает использование редактора запросов в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] для выполнения следующей процедуры. Процедура требует также использования командной строки для выполнения команд ETW.  
  
### <a name="to-monitor-system-activity-using-extended-events"></a>Мониторинг активности системы с помощью расширенных событий  
  
1.  В редакторе запросов выполните следующие инструкции, чтобы создать сеанс событий и добавить два события. Эти события (checkpoint_begin и checkpoint_end) запускаются в начале и в конце контрольной точки базы данных.  
  
    ```  
    CREATE EVENT SESSION test0  
    ON SERVER  
    ADD EVENT sqlserver.checkpoint_begin,  
    ADD EVENT sqlserver.checkpoint_end  
    WITH (MAX_DISPATCH_LATENCY = 1 SECONDS)  
    go  
    ```  
  
2.  Добавьте цель сегментирования с 32 сегментами для подсчета числа контрольных точек по идентификатору базы данных.  
  
    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    ADD TARGET package0.histogram  
    (  
          SET slots = 32, filtering_event_name = 'sqlserver.checkpoint_end', source_type = 0, source = 'database_id'  
    )  
    go  
    ```  
  
3.  Выполните следующие инструкции, чтобы добавить цель ETW. Это позволит видеть события начала и конца и использовать это для определения продолжительности обработки контрольной точки.  
  
    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    ADD TARGET package0.etw_classic_sync_target  
    go  
    ```  
  
4.  Введите следующие инструкции, чтобы запустить сеанс и начать сбор событий.  
  
    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    STATE = start  
    go  
    ```  
  
5.  Введите следующие инструкции для запуска трех событий.  
  
    ```  
    USE tempdb  
          checkpoint  
    go  
    USE master  
          checkpoint  
          checkpoint  
    go  
    ```  
  
6.  Введите следующие инструкции для просмотра счетчиков событий.  
  
    ```  
    SELECT CAST(xest.target_data AS xml) Bucketizer_Target_Data_in_XML  
    FROM sys.dm_xe_session_targets xest  
    JOIN sys.dm_xe_sessions xes ON xes.address = xest.event_session_address  
    JOIN sys.server_event_sessions ses ON xes.name = ses.name  
    WHERE xest.target_name = 'histogram' AND xes.name = 'test0'  
    go  
    ```  
  
7.  В командной строке введите следующие команды для просмотра данных EWT.  
  
    > [!NOTE]  
    >   Чтобы получить справку по команде **tracerpt** , введите в командной строке команду `tracerpt /?`.  
  
    ```  
    logman query -ets --- List the ETW sessions. This is optional.  
    logman update XE_DEFAULT_ETW_SESSION -fd -ets --- Flush the ETW log.  
    tracerpt %temp%\xeetw.etl -o xeetw.txt --- Dump the events so they can be seen.  
    ```  
  
8.  Введите следующие инструкции, чтобы прекратить сеанс и удалить его с сервера.  

    ```  
    ALTER EVENT SESSION test0  
    ON SERVER  
    STATE = STOP  
    go  
  
    DROP EVENT SESSION test0  
    ON SERVER  
    go  
    ```  
  
## <a name="see-also"></a>См. также:  
 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)   
 [DROP EVENT SESSION (Transact-SQL)](../../t-sql/statements/drop-event-session-transact-sql.md)   
 [Представления каталога расширенных событий (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Динамические административные представления расширенных событий](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Цели расширенных событий SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))  
  

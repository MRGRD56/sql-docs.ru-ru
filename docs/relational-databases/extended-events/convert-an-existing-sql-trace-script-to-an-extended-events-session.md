---
title: Преобразование скрипта трассировки SQL в сеанс расширенных событий
description: Используйте эти процедуры для создания сеанса расширенных событий, эквивалентного существующему скрипту трассировки SQL, который требуется преобразовать.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
helpviewer_keywords:
- SQL Trace, convert script to extended events
- extended events [SQL Server], convert SQL Trace script
ms.assetid: 4c8f29e6-0a37-490f-88b3-33493871b3f9
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4549a86841410a4aa5c68c80cd9480abf858d1d8
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492446"
---
# <a name="convert-an-existing-sql-trace-script-to-an-extended-events-session"></a>Преобразование существующего скрипта трассировки SQL в сеанс расширенных событий

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  При наличии существующего скрипта трассировки SQL, который требуется преобразовать для сеанса расширенных событий, можно использовать процедуры из данного раздела для создания эквивалентного сеанса расширенных событий. С помощью системных таблиц trace_xe_action_map и trace_xe_event_map можно собрать сведения, необходимые для выполнения преобразования.  
  
 Эти действия включают следующие шаги.  
  
1.  Создание сеанса трассировки SQL и получение идентификатора трассировки с помощью существующего скрипта.  
  
2.  Выполнение запроса, использующего функцию fn_trace_geteventinfo для поиска эквивалентных событий и действий расширенных событий для каждого класса событий трассировки SQL и связанных с ним столбцов.  
  
3.  Функция fn_trace_getfilterinfo позволяет получить список фильтров и эквивалентных действий расширенных событий для использования.  
  
4.  Создайте сеанс расширенных событий вручную с помощью эквивалентных событий, действий и предикатов (фильтров) расширенных событий.  

## <a name="to-obtain-the-trace-id"></a>Получение идентификатора трассировки  
  
1.  Откройте скрипт трассировки SQL в редакторе запросов и выполните его, чтобы создать сеанс трассировки. Обратите внимание, что для выполнения этой процедуры не нужно запускать сеанс трассировки.  
  
2.  Получите идентификатор трассировки. Для этого выполните следующий запрос:  
  
    ```sql
    SELECT * FROM sys.traces;  
    GO  
    ```  
  
    > [!NOTE]  
    >  Идентификатор трассировки 1 обычно соответствует трассировке по умолчанию.  
  
## <a name="to-determine-the-extended-events-equivalents"></a>Определение эквивалентов расширенных событий  
  
1.  Чтобы определить эквивалентные события и действия расширенных событий, выполните приведенный ниже запрос, в котором параметру *trace_id* присвоено значение идентификатора трассировки, полученное в предыдущей процедуре.  
  
    > [!NOTE]  
    >  В этом примере используется код идентификатор для трассировки по умолчанию (1).  
  
    ```sql
    USE MASTER;  
    GO  
    DECLARE @trace_id int;  
    SET @trace_id = 1;  
    SELECT DISTINCT el.eventid, em.package_name, em.xe_event_name AS 'event'  
       , el.columnid, ec.xe_action_name AS 'action'  
    FROM (sys.fn_trace_geteventinfo(@trace_id) AS el  
       LEFT OUTER JOIN sys.trace_xe_event_map AS em  
          ON el.eventid = em.trace_event_id)  
    LEFT OUTER JOIN sys.trace_xe_action_map AS ec  
       ON el.columnid = ec.trace_column_id  
    WHERE em.xe_event_name IS NOT NULL AND ec.xe_action_name IS NOT NULL;  
    ```  
  
     Возвращаются эквивалентные идентификатор события, имя пакета, имя события, идентификатор столбца и имя действия расширенных событий. Эти выходные данные будут использоваться в процедуре «Создание сеанса расширенных событий» далее в этом разделе.  
  
     В некоторых случаях отфильтрованный столбец сопоставляется с полем данных события, включенным по умолчанию в событие расширенных событий. Поэтому столбец «Extended_Events_action_name» будет иметь значение NULL. В этом случае необходимо выполнить следующие действия, чтобы определить, какое поле данных эквивалентно отфильтрованному столбцу.  
  
    1.  Для действий, возвращающих значение NULL, определите в скрипте классы событий трассировки SQL, содержащие фильтруемый столбец.  
  
         Например, при использовании класса событий SP:StmtCompleted мог быть указан фильтр по имени столбца трассировки Duration (идентификатор класса событий трассировки SQL — 45, идентификатор столбца трассировки SQL — 13). В этом случае имя действия будет отображаться в результатах запроса как значение NULL.  
  
    2.  Для каждого класса событий трассировки SQL, определенных в предыдущем шаге, найдите эквивалентное имя события расширенных событий. (Если вы не уверены в правильности имени эквивалентного события, используйте запрос, приведенный в разделе [Просмотр эквивалентов расширенных событий для классов событий трассировки SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md).)  
  
    3.  Следующий запрос используется для определения правильных полей данных для событий, определенных в предыдущем шаге. Запрос отображает поля данных расширенных событий в столбце «event_field». В запросе замените *<имя_события>* именем события, указанным в предыдущем шаге.  
  
        ```sql
        SELECT xp.name package_name, xe.name event_name  
           ,xc.name event_field, xc.description  
        FROM sys.trace_xe_event_map AS em  
        INNER JOIN sys.dm_xe_objects AS xe  
           ON em.xe_event_name = xe.name  
        INNER JOIN sys.dm_xe_packages AS xp  
           ON xe.package_guid = xp.guid AND em.package_name = xp.name  
        INNER JOIN sys.dm_xe_object_columns AS xc  
           ON xe.name = xc.object_name  
        WHERE xe.object_type = 'event' AND xc.column_type <> 'readonly'  
           AND em.xe_event_name = '<event_name>';  
        ```  
  
         Например, класс событий SP:StmtCompleted сопоставляется с событием sp_statement_completed расширенных событий. Если указать в запросе в качестве имени события sp_statement_completed, то в полях, которые по умолчанию включаются в событие, появится столбец "event_field". Среди полей есть поле «duration». Чтобы создать фильтр в эквивалентном сеансе расширенных событий, необходимо добавить предикат, например "WHERE duration > 0". В качестве примера см. процедуру «Создание сеанса расширенных событий» в этом разделе.  
  
## <a name="to-create-the-extended-events-session"></a>Создание сеанса расширенных событий  
 Воспользуйтесь редактором запросов для создания сеанса расширенных событий и записи выходных данных в целевой файл. Следующие шаги описывают один запрос, там же объясняется, как строить запрос. Полный пример запроса см. в подразделе «Пример» этого раздела.  
  
1.  Добавьте инструкции для создания сеанса событий, заменив *имя_сеанса* нужным именем сеанса расширенных событий.  
  
    ```sql
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [Session_Name] ON SERVER;  
    CREATE EVENT SESSION [Session_Name]  
    ON SERVER;  
    ```  
  
2.  Добавьте события и действия расширенных событий, возвращенные в качестве выходных данных в процедуре «Определение эквивалентов расширенных событий», и предикаты (фильтры), определенные в процедуре «Определение фильтров, использованных в скрипте».  
  
     В приведенном ниже примере используется скрипт трассировки SQL с классами событий SQL:StmtStarting и SP:StmtCompleted и фильтрами по полям session_id и duration. Образец выходных данных для запроса процедуры «Определение эквивалентов расширенных событий» представляет собой следующий результирующий набор:  
  
    ```  
    Eventid  package_name  event                   columnid  action  
    44       sqlserver     sp_statement_starting   6         nt_username  
    44       sqlserver     sp_statement_starting   9         client_pid  
    44       sqlserver     sp_statement_starting   10        client_app_name  
    44       sqlserver     sp_statement_starting   11        server_principal_name  
    44       sqlserver     sp_statement_starting   12        session_id  
    45       sqlserver     sp_statement_completed  6         nt_username  
    45       sqlserver     sp_statement_completed  9         client_pid  
    45       sqlserver     sp_statement_completed  10        client_app_name  
    45       sqlserver     sp_statement_completed  11        server_principal_name  
    45       sqlserver     sp_statement_completed  12        session_id  
    ```  
  
     Для преобразования его в эквивалент расширенных событий добавляются события sqlserver.sp_statement_starting и sqlserver.sp_statement_completed со списком действий. Инструкции-предикаты включены в качестве предложений WHERE.  
  
    ```sql
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
          (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
          )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       )  
    ```  
  
3.  Добавьте целевой асинхронный файл, заменив путь к файлу расположением, в котором требуется сохранить выходные данные. При использовании целевого файла следует включить путь к файлу журнала и метаданных.  
  
    ```sql
    ADD TARGET package0.asynchronous_file_target(  
       SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="to-view-the-results"></a>Просмотр результатов  
  
1.  Просмотреть выходные данные можно с помощью функции sys.fn_xe_file_target_read_file. Для этого запустите следующий запрос, заменив пути к файлам указанными путями:  
  
    ```sql
    SELECT *, CAST(event_data as XML) AS 'event_data_XML'  
    FROM sys.fn_xe_file_target_read_file('c:\temp\ExtendedEventsStoredProcs*.xel', 'c:\temp\ExtendedEventsStoredProcs*.xem', NULL, NULL);  
  
    ```  
  
    > [!NOTE]  
    >  Приведение данных события к XML является необязательным.  
  
     Дополнительные сведения о функции sys.fn_xe_file_target_read_file см. в разделе [sys.fn_xe_file_target_read_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md).  
  
    ```sql
    IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
       DROP EVENT SESSION [session_name] ON SERVER;  
    CREATE EVENT SESSION [session_name]  
    ON SERVER  
  
    ADD EVENT sqlserver.sp_statement_starting  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59   
       ),  
  
    ADD EVENT sqlserver.sp_statement_completed  
       (ACTION  
       (  
          sqlserver.nt_username,  
          sqlserver.client_pid,  
          sqlserver.client_app_name,  
          sqlserver.server_principal_name,  
          sqlserver.session_id  
       )  
       WHERE sqlserver.session_id = 59 AND duration > 0  
       );  
  
    ADD TARGET package0.asynchronous_file_target  
       (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
    ```  
  
## <a name="example"></a>Пример  
  
```sql
IF EXISTS(SELECT * FROM sys.server_event_sessions WHERE name='session_name')  
   DROP EVENT SESSION [session_name] ON SERVER;  
CREATE EVENT SESSION [session_name]  
ON SERVER  
  
ADD EVENT sqlserver.sp_statement_starting  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59   
   ),  
  
ADD EVENT sqlserver.sp_statement_completed  
   (ACTION  
   (  
      sqlserver.nt_username,  
      sqlserver.client_pid,  
      sqlserver.client_app_name,  
      sqlserver.server_principal_name,  
      sqlserver.session_id  
   )  
   WHERE sqlserver.session_id = 59 AND duration > 0  
   )  
  
ADD TARGET package0.asynchronous_file_target  
   (SET filename='c:\temp\ExtendedEventsStoredProcs.xel', metadatafile='c:\temp\ExtendedEventsStoredProcs.xem');  
```  
  
## <a name="see-also"></a>См. также:  
 [Просмотр эквивалентов расширенных событий для классов событий трассировки SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)  
  
  

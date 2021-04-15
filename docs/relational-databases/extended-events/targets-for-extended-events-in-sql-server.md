---
title: Целевые объекты для расширенных событий в SQL Server
description: В этой статье описываются целевые объекты package0 для расширенных событий в SQL Server. Сведения о возможностях целевых объектов при сборе и отправке данных и целевых параметрах.
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
ms.assetid: 47c64144-4432-4778-93b5-00496749665b
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: bb2ea11d395ab996adcd892a366cbf41575a78d4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489848"
---
# <a name="targets-for-extended-events-in-sql-server"></a>Целевые объекты для расширенных событий в SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


В этой статье объясняется, когда и как использовать целевые объекты package0 для расширенных событий в SQL Server. Для каждого целевого объекта в этой статье описываются следующие аспекты:

- Его способность сбора данных, отправляемых событиями, и включения их в отчеты.
- Его параметры, за исключением случаев, когда назначение параметра очевидно из его названия.


#### <a name="xquery-example"></a>Пример XQuery


[Раздел Ring_buffer](#h2_target_ring_buffer) содержит пример использования [XQuery в Transact-SQL](../../xquery/xquery-language-reference-sql-server.md) для копирования строки XML-кода в реляционный набор строк.


### <a name="prerequisites"></a>Предварительные требования


- Общее знакомство с основами расширенных событий, как описано в статье [Краткое руководство. Расширенные события в SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).


- Установка последней версии часто обновляемой служебной программы для SQL Server Management Studio (SSMS.exe). Дополнительные сведения см. в разделе:
    - [Скачивание SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)


- Умение использовать **обозреватель объектов** в SSMS.exe, чтобы щелкнуть правой кнопкой мыши целевой узел в сеансе событий для [удобного просмотра выходных данных](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md).
    - Данные события записываются в виде XML-строки. Однако в этой статье данные отображаются в виде реляционных строк. Среда SSMS использовалась для просмотра данных, а затем была скопирована и вставлена в эту статью.
    - Альтернативный способ T-SQL для создания наборов строк из XML-кода описан в [разделе ring_buffer](#h2_target_ring_buffer). В нем используется XQuery.



## <a name="parameters-actions-and-fields"></a>Параметры, действия и поля


В языке Transact-SQL инструкция [CREATE EVENT SESSION](~/t-sql/statements/create-event-session-transact-sql.md) является центральным элементом для расширенных событий. Для записи инструкции часто требуется список и описание из следующих элементов:

- Поля, связанные с выбранным событием.
- Параметры, связанные с выбранным целевым объектом.

Инструкции SELECT, которые возвращают такие списки из системных представлений, можно скопировать из следующей статьи в разделе C:

- [Использование SELECT и JOIN в системных представлениях для расширенных событий в SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)
    - [C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) Поля SELECT для события.
    - [C.6](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_6_parameters_targets) Параметры SELECT для целевого объекта.
    - [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) Действия SELECT.


Вы можете просмотреть параметры, поля и действия в контексте инструкции CREATE EVENT SESSION по [этой ссылке](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_B_2_TSQL_perspective).



<a name="h2_target_etw_classic_sync_target"></a>

## <a name="etw_classic_sync_target-target"></a>Целевой объект etw_classic_sync_target


Расширенные события SQL Server могут взаимодействовать с трассировкой событий Windows (ETW) для отслеживания активности системы. Дополнительные сведения см. в разделе:

- [Цель "Средство трассировки событий для Windows"](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [Мониторинг активности системы с помощью расширенных событий](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)


Этот целевой объект трассировки событий Windows обрабатывает получаемые данные *синхронно* , тогда как большинство целевых объектов делают это *асинхронно*.

> [!NOTE]
> База данных SQL Azure не поддерживает `etw_classic_sync_target target`.

<!-- After OPS Versioning is live, the above !NOTE could be converted into a "3colon ZONE".  GeneMi = MightyPen. -->

<a name="h2_target_event_counter"></a>

## <a name="event_counter-target"></a>Целевой объект event_counter


Целевой объект event_counter просто подсчитывает, сколько раз возникает каждое указанное событие.


В отличие от большинства других целевых объектов:

- Целевой объект event_counter не имеет параметров.


- Целевой объект event_counter обрабатывает получаемые данные *синхронно* .
    - Синхронная работа приемлема для простого event_counter, так как event_counter требует лишь небольшой объем обработки.
    - Ядро СУБД отключится от любого целевого объекта, который выполняется слишком медленно и может, таким образом, снизить производительность ядра СУБД. Это одна из причин того, что большинство целевых объектов работают *асинхронно*.


#### <a name="example-output-captured-by-event_counter"></a>Пример выходных данных, захваченных event_counter


```
package_name   event_name         count
------------   ----------         -----
sqlserver      checkpoint_begin   4
```


Далее идет CREATE EVENT SESSION, который привел к предыдущим результатам. Для этого теста использовалось поле **package0.counter** в предложении EVENT...WHERE, чтобы прекратить подсчета после достижения значения 4.


```sql
CREATE EVENT SESSION [event_counter_1]
    ON SERVER 
    ADD EVENT sqlserver.checkpoint_begin   -- Test by issuing CHECKPOINT; statements.
    (
        WHERE ([package0].[counter] <= (4))   -- A predicate filter.
    )
    ADD TARGET package0.event_counter
    WITH
    (
        MAX_MEMORY = 4096 KB,
        MAX_DISPATCH_LATENCY = 3 SECONDS
    );
```



<a name="h2_target_event_file"></a>

## <a name="event_file-target"></a>Целевой объект event_file


Целевой объект **event_file** записывает выходные данные сеанса событий из буфера в файл на диске:


- Укажите параметр *filename =* в предложении ADD TARGET.
    - Файл должен иметь расширение **XEL** .


- Выбранное вами имя файла используется системой в качестве префикса, к которому добавляется длинное целое число на основе даты и времени, за которым следует расширение XEL.

::: moniker range="= azuresqldb-current || = azuresqldb-mi-current "

> [!NOTE]
> База данных SQL Azure поддерживает хранение файлов `xel` только в Хранилище BLOB-объектов Azure. 
>
> Пример кода **event_file** для базы данных SQL (и Управляемого экземпляра SQL) см. в разделе [Код целевого файла событий для расширенных событий в базе данных SQL](/azure/sql-database/sql-database-xevent-code-event-file).

::: moniker-end


#### <a name="create-event-session-with-event_file-target"></a>CREATE EVENT SESSION с целевым объектом **event_file**


Далее идет CREATE EVENT SESSION, который мы использовали для тестирования. Одно из предложений ADD TARGET указывает event_file.


```sql
CREATE EVENT SESSION [locks_acq_rel_eventfile_22]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION (sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION(sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.event_file
    (
        SET     filename=N'C:\Junk\locks_acq_rel_eventfile_22-.xel'
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=10 SECONDS
    );
```


#### <a name="sysfn_xe_file_target_read_file-function"></a>функция sys.fn_xe_file_target_read_file


Целевой объект event_file хранит полученные данные в двоичном формате, который непригоден для чтения человеком. Transact-SQL может вывести содержимое XEL-файла, использовав SELECT FROM для функции [**sys.fn_xe_file_target_read_file**](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md) .


Для SQL Server **2016** и более поздней версии, следующая инструкция SELECT T-SQL выводит данные. Суффикс *.xel в 


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            null, null, null)  AS f;
```


Для SQL Server **2014** данные выводит инструкция SELECT, аналогичная приведенной ниже. После SQL Server 2014 XEM-файлы больше не используются.


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            'C:\junk\metafile.xem',
            null, null)  AS f;
```


Конечно, можно вручную использовать интерфейс SSMS для просмотра данных в XEL-файлах:


#### <a name="data-stored-in-the-event_file-target"></a>Данные, хранящиеся в целевом объекте event_file


Далее приведен отчет по результатам использования инструкции SELECT для **sys.fn_xe_file_target_read_file** в SQL Server 2016.


```
module_guid                            package_guid                           object_name     event_data                                                                                                                                                                                                                                                                                          file_name                                                      file_offset
-----------                            ------------                           -----------     ----------                                                                                                                                                                                                                                                                                          ---------                                                      -----------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_acquired   <event name="lock_acquired" package="sqlserver" timestamp="2016-08-07T20:13:35.827Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_released   <event name="lock_released" package="sqlserver" timestamp="2016-08-07T20:13:35.832Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
```



<a name="h2_target_histogram"></a>

## <a name="histogram-target"></a>целевой объект histogram


Целевой объект **histogram** разнообразнее целевого объекта event_counter. Объект histogram позволяет сделать следующее:

- подсчитывать вхождения для нескольких элементов отдельно;
- подсчитывать вхождения для разных типов элементов:
    - Поля событий.
    - Действия.


Параметр **source_type** является основой для управления целевым объектом histogram:

- **source_type=0** — означает сбор данных для *полей событий*).
- **source_type=1** — означает сбор данных для *действий*).
    - 1 используется по умолчанию.


По умолчанию параметр slots равен 256. Если назначить другое значение, оно округляется вверх до следующей степени 2.

- Например, slots=59 округляется до 64.


### <a name="action-example-for-histogram"></a>Пример *действия* для histogram


В предложении TARGET...SET следующая инструкция CREATE EVENT SESSION Transact-указывает назначение целевого параметра **source_type=1**. 1 означает, что целевой объект histogram отслеживает действие.

В текущем примере предложение EVENT...ACTION предложение предлагает на выбор целевому объекту всего одно действие, а именно **sqlos.system_thread_id**. В предложении TARGET...SET мы видим, назначение **source=N'sqlos.system_thread_id'** .

- Для отслеживания более одного исходного действия можно добавить второй целевой объект histogram в инструкцию CREATE EVENT SESSION.


```sql
CREATE EVENT SESSION [histogram_lockacquired]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
        (
        ACTION
            (
            sqlos.system_thread_id
            )
        )
    ADD TARGET package0.histogram
        (
        SET
            filtering_event_name=N'sqlserver.lock_acquired',
            slots=(16),
            source=N'sqlos.system_thread_id',
            source_type=1
        )
    WITH
        (
        <.... (For brevity, numerous parameter assignments generated by SSMS.exe are not shown here.) ....>
        );
```


 Были собраны указанные ниже данные. Значения в столбце **value** были значениями system_thread_id. Например, в потоке 6540 общее число блокировок составило 236.


```
value   count
-----   -----
 6540     236
 9308      91
 9668      74
10144      49
 5244      44
 2396      28
```


#### <a name="select-to-discover-available-actions"></a>Использование инструкции SELECT для обнаружения доступных действий


Инструкция [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) SELECT позволяет найти действия, доступные в системе для указания в инструкции CREATE EVENT SESSION. В предложении WHERE сначала измените фильтр **o.name LIKE** в соответствии с интересующими вас действиями.


Далее инструкция C.3 SELECT возвращает выборку набора строк. Действие **system_thread_id** указано во второй строке.


```
Package-Name   Action-Name                 Action-Description
------------   -----------                 ------------------
package0       collect_current_thread_id   Collect the current Windows thread ID
sqlos          system_thread_id            Collect current system thread ID
sqlserver      create_dump_all_threads     Create mini dump including all threads
sqlserver      create_dump_single_thread   Create mini dump for the current thread
```


### <a name="event-field-example-for-histogram"></a>Пример *поля* события для histogram


Следующий пример задает **source_type=0**. Значение, присваиваемое **source=** , представляет собой поле события (а не действие).



```sql
CREATE EVENT SESSION [histogram_checkpoint_dbid]
    ON SERVER 
    ADD EVENT  sqlserver.checkpoint_begin
    ADD TARGET package0.histogram
    (
    SET
        filtering_event_name = N'sqlserver.checkpoint_begin',
        source               = N'database_id',
        source_type          = (0)
    )
    WITH
    ( <....> );
```


Целевым объектом histogram были собраны указанные ниже данные. Данные показывают, что в базе данных, у которой ID=5, произошло 7 событий checkpoint_begin.


```
value   count
-----   -----
5       7
7       4
6       3
```


#### <a name="select-to-discover-available-fields-on-your-chosen-event"></a>Использование инструкции SELECT для обнаружения доступных полей для выбранного события


Инструкция [C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) SELECT отображает поля событий, доступные для выбора. Сначала измените фильтр **o.name LIKE** на имя выбранного события.


Приведенный ниже набор строк возвращен инструкцией C.4 SELECT. Этот набор строк показывает, что database_id является единственным полем в событии checkpoint_begin, которое может предоставить значения для целевого объекта histogram.


```
Package-Name   Event-Name         Field-Name   Field-Description
------------   ----------         ----------   -----------------
sqlserver      checkpoint_begin   database_id  NULL
sqlserver      checkpoint_end     database_id  NULL
```


<a name="h2_target_pair_matching"></a>

## <a name="pair_matching-target"></a>Целевой объект pair_matching


Целевой объект pair_matching позволяет обнаруживать начальные события без соответствующего конечного события. Например, если событие lock_acquired возникает без своевременной выдачи события lock_released, может возникнуть проблема.


Система не сопоставляет начальные и конечные события автоматически. Вместо этого вы поясняете системе такое сопоставление в инструкции CREATE EVENT SESSION. При совпадении начального и конечного событий эта пара отбрасывается, чтобы можно было сконцентрироваться на начальных событиях, не имеющих соответствия.


#### <a name="finding-matchable-fields-for-the-start-and-end-event-pair"></a>Поиск соответствующих полей для пары для начального и конечного событий


Благодаря инструкции [C.4 SELECT](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields)мы видим, что в следующем наборе строк присутствует около 16 полей для события lock_acquired. Приведенный здесь набор строк был вручную разделен, чтобы показать, каким полям соответствует наш пример. Сопоставление некоторых полей, например **duration** для обоих событий, не имеет смысла.


```
Package-Name   Event-Name   Field-Name               Field-Description
------------   ----------   ----------               -----------------
sqlserver   lock_acquired   database_name            NULL
sqlserver   lock_acquired   mode                     NULL
sqlserver   lock_acquired   resource_0               The ID of the locked object, when lock_resource_type is OBJECT.
sqlserver   lock_acquired   resource_1               NULL
sqlserver   lock_acquired   resource_2               The ID of the lock partition, when lock_resource_type is OBJECT, and resource_1 is 0.
sqlserver   lock_acquired   transaction_id           NULL

sqlserver   lock_acquired   associated_object_id     The ID of the object that requested the lock that was acquired.
sqlserver   lock_acquired   database_id              NULL
sqlserver   lock_acquired   duration                 The time (in microseconds) between when the lock was requested and when it was canceled.
sqlserver   lock_acquired   lockspace_nest_id        NULL
sqlserver   lock_acquired   lockspace_sub_id         NULL
sqlserver   lock_acquired   lockspace_workspace_id   NULL
sqlserver   lock_acquired   object_id                The ID of the locked object, when lock_resource_type is OBJECT. For other lock resource types it will be 0
sqlserver   lock_acquired   owner_type               NULL
sqlserver   lock_acquired   resource_description     The description of the lock resource. The description depends on the type of lock. This is the same value as the resource_description column in the sys.dm_tran_locks view.
sqlserver   lock_acquired   resource_type            NULL
```


### <a name="example-of-pair_matching"></a>Пример pair_matching


Следующая инструкция CREATE EVENT SESSION указывает два события и два целевых объекта. Целевой объект pair_matching указывает два набора полей для сопоставления событий по парам. Порядок полей с разделителями-запятыми, назначенный **begin_matching_columns =** и **end_matching_columns =** , должен совпадать. Между полями, указанными в значении с разделителями-запятыми, нельзя использовать символы табуляции и новой строки, но можно использовать пробелы.

Чтобы сократить число результатов, мы сначала использовали инструкцию SELECT для sys.objects, чтобы найти object_id нашей тестовой таблицы. Мы добавили фильтр для этого идентификатора в предложение EVENT...WHERE.


```sql
CREATE EVENT SESSION [pair_matching_lock_a_r_33]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.pair_matching
    (
        SET
            begin_event = N'sqlserver.lock_acquired',
            begin_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            end_event = N'sqlserver.lock_released',
            end_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            respond_to_memory_pressure = (1)
    )
    WITH
    (
        MAX_MEMORY = 8192 KB,
        MAX_DISPATCH_LATENCY = 15 SECONDS
    );
```


Чтобы протестировать сеанса событий, мы намеренно запретили снятие полученных блокировок. Для этого мы использовали следующие шаги в коде T-SQL:

1. BEGIN TRANSACTION.
2. UPDATE MyTable...
3. Намеренное отсутствие COMMIT TRANSACTION, до изучения целевых объектов.
4. Выполнение COMMIT TRANSACTION после тестирования.

Простой целевой объект **event_counter** предоставил указанные ниже выходные строки. Поскольку 52-50 = 2, выходные данные указывают на то, что мы должны видеть 2 непарных события lock_acquired при рассмотрении результата целевого объекта pair_matching.


```
package_name   event_name      count
------------   ----------      -----
sqlserver      lock_acquired   52
sqlserver      lock_released   50
```


Целевой объект **pair_matching** предоставил указанные ниже выходные данные. Мы действительно видим 2 строки lock_acquired, на что и указывали выходные данные event_counter. Отображение этих строк означает, что эти два события lock_acquired являются непарными.


```
package_name   event_name      timestamp                     database_name   duration   mode   object_id   owner_type   resource_0   resource_1   resource_2   resource_description   resource_type   transaction_id
------------   ----------      ---------                     -------------   --------   ----   ---------   ----------   ----------   ----------   ----------   --------------------   -------------   --------------
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          S      370100359   Transaction  370100359    3            0            [INDEX_OPERATION]      OBJECT          34126
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          IX     370100359   Transaction  370100359    0            0                                   OBJECT          34126
```


Строки для непарных событий lock_acquired могли включать в себя текст T-SQL или **sqlserver.sql_text**, который принял блокировки. Но мы не хотели слишком сильно раздувать отображаемые данные.


<a name="h2_target_ring_buffer"></a>

## <a name="ring_buffer-target"></a>Целевой объект ring_buffer


Целевой объект ring_buffer удобен для быстрого и простого тестирования событий. При остановке сеанса событий хранимые выходные данные удаляются.

В этом разделе ring_buffer также показано, как можно использовать реализацию XQuery на языке Transact-SQL для копирования XML-содержимого ring_buffer в более удобочитаемый реляционный набор строк.


#### <a name="create-event-session-with-ring_buffer"></a>CREATE EVENT SESSION с ring_buffer


В этой инструкции CREATE EVENT SESSION, которая использует целевой объект ring_buffer, нет ничего особенного.


```sql
CREATE EVENT SESSION [ring_buffer_lock_acquired_4]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET collect_resource_description=(1)

        ACTION(sqlserver.database_name)

        WHERE
        (
            [object_id]=(370100359)  -- ID of MyTable
            AND
            sqlserver.database_name='InMemTest2'
        )
    )
    ADD TARGET package0.ring_buffer
    (
        SET max_events_limit=(98)
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=3 SECONDS
    );
```


### <a name="xml-output-received-for-lock_acquired-by-ring_buffer"></a>Выходные данные XML, полученные ring_buffer для lock_acquired


При извлечении инструкцией SELECT содержимое находится в виде XML-строки. Далее показана XML-строка, хранимая целевым объектом ring_buffer в тестировании. Однако для краткости последующего XML-кода были удалены все элементы &#x3c;event&#x3e;, кроме двух. Кроме того, внутри каждого &#x3c;event&#x3e; было удалено несколько лишних элементов &#x3c;data&#x3e;.


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="6" eventCount="6" droppedCount="0" memoryUsed="1032">
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:53.987Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111030</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:56.012Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111039</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
</RingBufferTarget>
```


Чтобы просмотреть предыдущий XML-код, вы можете использовать приведенную ниже инструкцию SELECT, пока активен сеанс событий. Активные XML-данные извлекаются из системного представления **sys.dm_xe_session_targets**.


```sql
SELECT
        CAST(LocksAcquired.TargetXml AS XML)  AS RBufXml
    INTO
        #XmlAsTable
    FROM
        (
        SELECT
                CAST(t.target_data AS XML)  AS TargetXml
            FROM
                     sys.dm_xe_session_targets  AS t
                JOIN sys.dm_xe_sessions         AS s

                    ON s.address = t.event_session_address
            WHERE
                t.target_name = 'ring_buffer'
                AND
                s.name        = 'ring_buffer_lock_acquired_4'
        )
            AS LocksAcquired;


SELECT * FROM #XmlAsTable;
```


### <a name="xquery-to-see-the-xml-as-a-rowset"></a>XQuery для просмотра XML-данных в виде набора строк


Чтобы просмотреть предыдущий XML-кода в виде реляционного набора строк, после указанной выше инструкции SELECT выполните приведенный ниже код T-SQL. Каждое использование XQuery поясняется с помощью закомментированных строк.


```sql
SELECT
         -- (A)
         ObjectLocks.value('(@timestamp)[1]',
            'datetime'     )  AS [OccurredDtTm]

        -- (B)
        ,ObjectLocks.value('(data[@name="mode"]/text)[1]',
            'nvarchar(32)' )  AS [Mode]

        -- (C)
        ,ObjectLocks.value('(data[@name="transaction_id"]/value)[1]',
            'bigint' )  AS [TxnId]

        -- (D)
        ,ObjectLocks.value('(action[@name="database_name" and @package="sqlserver"]/value)[1]',
            'nvarchar(128)')  AS [DatabaseName]
    FROM
        #TableXmlCell
    CROSS APPLY
        -- (E)
        TargetDateAsXml.nodes('/RingBufferTarget/event[@name="lock_acquired"]')  AS T(ObjectLocks);
```


#### <a name="xquery-notes-from-preceding-select"></a>Заметки о XQuery из предыдущей инструкции SELECT


(А)
- timestamp= значению атрибута в элементе &#x3c;event&#x3e;.
- Конструкция "(...)[1]" обеспечивает возвращение только одного значения в каждой итерации, так как это обязательно ограничение метода .value() XQuery для столбцов и переменной типа XML-данных.


(Б)
- Внутреннее значение элемента &#x3c;text&#x3e; внутри элемента &#x3c;data&#x3e;, атрибут name= которого равен mode.


(В)
- Внутреннее значение элемента &#x3c;value&#x3e; внутри элемента &#x3c;data&#x3e;, атрибут name= которого равен transaction_id.


(Г)
- &#x3c;event&#x3e; содержит &#x3c;action&#x3e;.
- Элемент &#x3c;action&#x3e;, у которого атрибут name= равен database_name, а атрибут package= равен sqlserver (не package0), получает внутреннее значение элемента &#x3c;value&#x3e;.


(Д)
- C.A. вызывает повтор обработки для каждого отдельного элемента &#x3c;event&#x3e;, атрибут name= которого равен lock_acquired.
- Это относится к XML-данным, возвращаемым предыдущим предложением FROM.


#### <a name="output-from-xquery-select"></a>Выходные данные XQuery SELECT


Ниже приведен набор строк, созданный описанным выше кодом T-SQL, который включает в себя XQuery.


```
OccurredDtTm              Mode    DatabaseName
------------              ----    ------------
2016-08-05 23:59:53.987   SCH_S   InMemTest2
2016-08-05 23:59:56.013   SCH_S   InMemTest2
```



## <a name="xevent-net-namespaces-and-cx23"></a>Пространства имен XEvent .NET и C&#x23;


Package0 имеет еще два целевых объекта, но их нельзя использовать в Transact-SQL:

- compressed_history
- event_stream


Одним из признаков того, что эти целевые объекты нельзя использовать в T-SQL, является то, что их отличные от NULL значения в столбце *sys.dm_xe_objects.capabilities* не содержат бит 0x1.


Целевой объект event_stream можно использовать в программах платформы .NET, написанных на таких языках, как C#. Разработчики на C# и других языках платформы .NET могут получить доступ к потоку событий через класс .NET Framework, например в пространстве имен Microsoft.SqlServer.XEvents.Linq.

Если произошла ошибка **25726** , значит поток событий заполнился данными быстрее, чем клиент смог их обработать. В результате ядро СУБД отключилось от потока событий, чтобы избежать снижения производительности сервера.


### <a name="xevent-namespaces"></a>Пространства имен XEvent


- [Пространство имен Microsoft.SqlServer.Management.XEvent](/dotnet/api/microsoft.sqlserver.management.xevent)

- [Пространство имен Microsoft.SqlServer.XEvent.Linq](/dotnet/api/microsoft.sqlserver.xevent.linq)

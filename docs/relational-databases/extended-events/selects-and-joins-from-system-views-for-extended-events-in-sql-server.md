---
title: Использование SELECT и JOIN в системных представлениях для расширенных событий
description: В SQL Server и базе данных SQL Azure имеются системные представления расширенных событий. Узнайте, как сведения о сеансе событий представлены в разных перспективах.
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
ms.assetid: 04521d7f-588c-4259-abc2-1a2857eb05ec
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da83f31fde5d1234a4c6dc1402a439782c4bdda1
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489942"
---
# <a name="selects-and-joins-from-system-views-for-extended-events-in-sql-server"></a>Использование SELECT и JOIN в системных представлениях для расширенных событий в SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


В этой статье описываются два набора системных представлений, связанных с расширенными событиями в SQL Server и в Базе данных SQL Azure. В статье рассматриваются следующие вопросы:

- Присоединение (JOIN) различных системных представлений.
- Выбор (SELECT) определенного рода сведений из системных представлений.
- Представление одной и той же информации сеанса событий с точки зрения различных технологий для более глубокого понимания каждого варианта.


Большая часть примеров написана для SQL Server. Однако после внесения незначительных изменений их можно выполнить и в базе данных SQL.



## <a name="a-foundational-information"></a>A. Основные сведения


Существует два набора системных представлений для расширенных событий.


#### <a name="catalog-views"></a>Представления каталога

- В этих представлениях хранятся сведения об *определении* каждого сеанса событий, созданного с помощью [CREATE EVENT SESSION](../../t-sql/statements/create-event-session-transact-sql.md)или эквивалента в пользовательском интерфейсе среды SSMS. Однако в этих представлениях нет никаких данных о том, запускались ли вообще эти сеансы.
    - Например, если в **обозревателе объектов** SSMS показано, что не определено ни одного сеанса событий, то при использовании инструкции SELECT в представлении *sys.server_event_session_targets* не будет возвращено ни одной строки.


- Префикс имени:
    - *sys.server\_event\_session\** — префикс имени на сервере SQL Server;
    - *sys.database\_event\_session\** — префикс имени в базе данных SQL Server.


#### <a name="dynamic-management-views-dmvs"></a>Динамические административные представления (DMV)

- Хранение сведений о *текущей активности* запущенных сеансов событий. Однако динамические административные представления содержат минимум сведений об определении сеансов.
    - Даже если в настоящее время остановлены все сеансы событий, инструкция SELECT в представлении *sys.dm_xe_packages* будет по-прежнему возвращать строки, так как при запуске сервера в активную память загружаются разные пакеты.
    - По этой же причине *sys.dm_xe_objects* *sys.dm_xe_object_columns* будут по-прежнему возвращать строки.


- Префикс имени для динамических административных представлений расширенных событий:
    - *sys.dm\_xe\_\** — префикс имени на сервере SQL Server;
    - *sys.dm\_xe\_database\_\** — префикс имени в базе данных SQL.


#### <a name="permissions"></a>Разрешения:


Для выполнения SELECT в представлениях системы необходимо следующее разрешение:

- VIEW SERVER STATE — если на сервере Microsoft SQL Server;
- VIEW DATABASE STATE — если в базе данных Azure SQL.


<a name="section_B_catalog_views"></a>

## <a name="b-catalog-views"></a>Б. Представления каталога


В этом разделе сопоставляются три разных технологических подхода для одного и того же определенного сеанса событий. Сеанс был определен и отображается в **обозревателе объектов** среды SQL Server Management Studio (SSMS.exe), но сеанс в данный момент не запущен.

Чтобы избежать непредвиденных сбоев, рекомендуется ежемесячно [устанавливать последнее обновление SSMS](../../ssms/download-sql-server-management-studio-ssms.md).


Справочную документацию по представлениям каталога для расширенных событий можно найти в разделе [Представления каталога расширенных событий (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md).


&nbsp;



#### <a name="the-sequence-in-this-section-b"></a>Содержание раздела Б


- [Б.1. Обзор с использованием пользовательского интерфейса среды SSMS](#section_B_1_SSMS_UI_perspective)
    - Создание определения сеанса событий с помощью пользовательского интерфейса среды SSMS. Пошаговые снимки экрана.


- [Б.2. Обзор с использованием Transact-SQL](#section_B_2_TSQL_perspective)
    - Использование контекстного меню среды SSMS для реконструирования определенного сеанса событий в эквивалентную инструкцию Transact-SQL **CREATE EVENT SESSION** . T-SQL демонстрирует точное совпадение с данными на снимках экрана SSMS.


- [Б.3. Обзор с использованием конструкции SELECT JOIN UNION представления каталога](#section_B_3_Catalog_view_S_J_UNION)
    - Выполнение инструкции T-SQL SELECT в представлениях системного каталога для сеанса событий. Результаты совпадают со спецификациями инструкции **CREATE EVENT SESSION** .


&nbsp;



<a name="section_B_1_SSMS_UI_perspective"></a>

### <a name="b1-ssms-ui-perspective"></a>Б.1. Обзор с использованием пользовательского интерфейса среды SSMS


В среде SSMS в **обозревателе объектов** откройте диалоговое окно **Новый сеанс** , развернув узлы **Управление** > **Расширенные события** и щелкнув правой кнопкой мыши **Сеансы** > **Создать сеанс**.

В большом диалоговом окне **Новый сеанс** в его первом разделе **Общие** был выбран параметр **Запускать сеанс событий при запуске сервера**.

![Новый сеанс > Общие, Запускать сеанс событий при запуске сервера.](../../relational-databases/extended-events/media/xevents-ssms-ac105-eventname-startup.png)


Далее в разделе **События** видно, что было выбрано событие **lock_deadlock**. Для этого события были выбраны три **действия** . Это означает, что была нажата кнопка **Настроить** , которая становится серой после нажатия.

![Новый сеанс > События, Глобальные поля (действия)](../../relational-databases/extended-events/media/xevents-ssms-ac110-actions-global.png)


<a name="resource_type_PAGE_cat_view"></a>

В разделе **События** > **Настройка** видно, что полю [**resource_type** было задано значение **PAGE**](#resource_type_dmv_actual_row). Это означает, что данные события не будут отправляться от обработчика событий в целевой объект, если значение поля **resource_type** отлично от **PAGE**.

Здесь также показаны дополнительные фильтры предикатов для имени базы данных и счетчика.

![Новый сеанс > События, Фильтр (предикат), Поля (действия)](../../relational-databases/extended-events/media/xevents-ssms-ac115-predicate-db.png)


Затем в разделе **Хранилище данных** показано, что в качестве целевого объекта выбран **event_file**. Кроме того, здесь видно, что был выбран параметр **Включить переключение файлов**.

![Новый сеанс > Хранилище данных, eventfile_enablefileroleover](../../relational-databases/extended-events/media/xevents-ssms-ac120-target-eventfile.png)


И, наконец, в разделе **Дополнительно** показано, что значение параметра **Максимальная задержка диспетчера** было уменьшено до 4-х секунд.

![Новый сеанс > Дополнительно, Максимальная задержка диспетчера](../../relational-databases/extended-events/media/xevents-ssms-ac125-latency4.png)


На этом обзор определения сеанса событий с использованием пользовательского интерфейса среды SSMS завершен.


<a name="section_B_2_TSQL_perspective"></a>

### <a name="b2-transact-sql-perspective"></a>Б.2. Обзор с использованием Transact-SQL


Независимо от способа создания определения сеанса событий сеанс из пользовательского интерфейса среды SSMS можно реконструировать в точно совпадающий скрипт Transact-SQL. Можно просмотреть предыдущие снимки экрана нового сеанса и сравнить отображаемые данные с предложениями в следующем созданном скрипте T-SQL **CREATE EVENT SESSION** .

Чтобы реконструировать сеанс событий, в **обозревателе объектов** щелкните правой кнопкой мыши узел сеанса и затем выберите **Создать скрипт сеанса как** > **Создать в** > **Буфер обмена**.

В результате реконструирования с помощью среды был создан следующий скрипт T-SQL. Затем он был вручную доработан с использованием только пробелов.


```sql
CREATE EVENT SESSION [event_session_test3]
    ON SERVER  -- Or, if on Azure SQL Database, ON DATABASE.

    ADD EVENT sqlserver.lock_deadlock
    (
        SET
            collect_database_name = (1)
        ACTION
        (
            package0  .collect_system_time,
            package0  .event_sequence,
            sqlserver .client_hostname
        )
        WHERE
        (
            [database_name]           = N'InMemTest2'
            AND [package0].[counter] <= (16)
            AND [resource_type]       = (6)
        )
    )

    ADD TARGET package0.event_file
    (
        SET
            filename           = N'C:\Junk\event_session_test3_EF.xel',
            max_file_size      = (20),
            max_rollover_files = (2)
    )

    WITH
    (
        MAX_MEMORY            = 4096 KB,
        EVENT_RETENTION_MODE  = ALLOW_SINGLE_EVENT_LOSS,
        MAX_DISPATCH_LATENCY  = 4 SECONDS,
        MAX_EVENT_SIZE        = 0 KB,
        MEMORY_PARTITION_MODE = NONE,
        TRACK_CAUSALITY       = OFF,
        STARTUP_STATE         = ON
    );
```


На этом обзор с использованием T-SQL завершен.


<a name="section_B_3_Catalog_view_S_J_UNION"></a>

### <a name="b3-catalog-view-select-join-union-perspective"></a>Б.3. Обзор с использованием конструкции SELECT JOIN UNION представления каталога


Не пугайтесь! Следующая инструкция T-SQL SELECT слишком длинна только потому, что она объединяет (UNION) несколько небольших инструкций SELECT. Любая из этих небольших инструкций SELECT может выполняться самостоятельно. Небольшие инструкции SELECT демонстрируют варианты присоединения (JOIN) различных системных представлений каталогизации.


```sql
SELECT
        s.name        AS [Session-Name],
        '1_EVENT'     AS [Clause-Type],
        'Event-Name'  AS [Parameter-Name],
        e.name        AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name         AS [Session-Name],
        '2_EVENT_SET'  AS [Clause-Type],
        f.name         AS [Parameter-Name],
        f.value        AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_fields   As f

            ON  f.event_session_id = s.event_session_id
            AND f.object_id        = e.event_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name              AS [Session-Name],
        '3_EVENT_ACTION'    AS [Clause-Type],

        a.package + '.' + a.name
                            AS [Parameter-Name],

        '(Not_Applicable)'  AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_actions  As a

            ON  a.event_session_id = s.event_session_id
            AND a.event_id         = e.event_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name                AS [Session-Name],
        '4_EVENT_PREDICATES'  AS [Clause-Type],
        e.predicate           AS [Parameter-Name],
        '(Not_Applicable)'    AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_events   AS e

            ON  e.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name              AS [Session-Name],
        '5_TARGET'          AS [Clause-Type],
        t.name              AS [Parameter-Name],
        '(Not_Applicable)'  AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_targets  AS t

            ON  t.event_session_id = s.event_session_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name          AS [Session-Name],
        '6_TARGET_SET'  AS [Clause-Type],
        f.name          AS [Parameter-Name],
        f.value         AS [Parameter-Value]
    FROM
              sys.server_event_sessions         AS s
        JOIN  sys.server_event_session_targets  AS t

            ON  t.event_session_id = s.event_session_id

        JOIN  sys.server_event_session_fields   As f

            ON  f.event_session_id = s.event_session_id
            AND f.object_id        = t.target_id
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name               AS [Session-Name],
        '7_WITH_MAX_MEMORY'  AS [Clause-Type],
        'max_memory'         AS [Parameter-Name],
        s.max_memory         AS [Parameter-Value]
    FROM
              sys.server_event_sessions  AS s
    WHERE
        s.name = 'event_session_test3'

UNION ALL
SELECT
        s.name                  AS [Session-Name],
        '7_WITH_STARTUP_STATE'  AS [Clause-Type],
        'startup_state'         AS [Parameter-Name],
        s.startup_state         AS [Parameter-Value]
    FROM
              sys.server_event_sessions  AS s
    WHERE
        s.name = 'event_session_test3'

ORDER BY
    [Session-Name],
    [Clause-Type],
    [Parameter-Name]
;
```


#### <a name="output"></a>Выходные данные


В следующей таблице приведены выходные данные выполнения предыдущей конструкции SELECT JOIN UNION. Имена и значения выходных параметров соответствуют результату предыдущей инструкции CREATE EVENT SESSION.

| Session-Name | Clause-Type | Parameter-Name | Parameter-Value |
|---|---|---|---|
|event_session_test3  | 1_EVENT |                Event-Name |                       lock_deadlock |
|event_session_test3  |  2_EVENT_SET |             collect_database_name |            1 |
|event_session_test3  |  3_EVENT_ACTION |          sqlserver.client_hostname |       (Not_Applicable) |
|event_session_test3  |  3_EVENT_ACTION |         sqlserver.collect_system_time |   (Not_Applicable) |
|event_session_test3  |  3_EVENT_ACTION |         sqlserver.event_sequence |        (Not_Applicable) |
|event_session_test3  |  4_EVENT_PREDICATES |     (\[sqlserver\].\[equal_i_sql_unicode_string\]\(\[database_name\],N'InMemTest2'\) AND \[package0\].\[counter\]<=\(16\)\) |   (Not_Applicable) |
|event_session_test3  |  5_TARGET |               event_file |                      (Not_Applicable) |
|event_session_test3  |  6_TARGET_SET |           filename  |                       C:\Junk\event_session_test3_EF.xel |
|event_session_test3  |  6_TARGET_SET |           max_file_size |                   20 |
|event_session_test3  |  6_TARGET_SET |           max_rollover_files |              2 |
|event_session_test3  |  7_WITH_MAX_MEMORY |      max_memory |                      4096 |
|event_session_test3  |  7_WITH_STARTUP_STATE |   startup_state |                   1 |

На этом раздел с представлениями каталога завершен.



<a name="section_C_DMVs"></a>

## <a name="c-dynamic-management-views-dmvs"></a>В. Динамические административные представления (DMV)


Перейдем к динамическим административным представлениям. Этот раздел содержит несколько инструкций Transact-SQL SELECT, каждая из которых служит конкретной цели. Кроме того, инструкции SELECT демонстрируют, как можно применять JOIN, чтобы объединить представления DMV для использования в новых целях.


Справочная документация по DMV доступна в статье [Динамические административные представления расширенных событий](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md).


В этой статье все фактические строки выходных данных из следующих инструкций SELECT относятся к SQL Server 2016, если не указано иное.


Ниже приведен список инструкций SELECT в разделе В, посвященном представлениям DMV.

- [В.1. Список всех пакетов](#section_C_1_list_packages)
- [В.2. Количество каждого типа объекта](#section_C_2_count_object_type)
- [В.3. SELECT для всех доступных элементов, отсортированных по типу](#section_C_3_select_all_available_objects)
- [В.4. Поля данных, доступные для события](#section_C_4_data_fields)
- [В.5. *sys.dm_xe_map_values* и поля событий](#section_C_5_map_values_fields)
- [В.6. Параметры для целевых объектов](#section_C_6_parameters_targets)
- [В.7. Инструкция SELECT DMV, приводящая столбец target_data к формату XML](#section_C_7_dmv_select_target_data_column)
- [В.8. Использование SELECT в функции для получения данных event_file с диска](#section_C_8_select_function_disk)



<a name="section_C_1_list_packages"></a>

### <a name="c1-list-of-all-packages"></a>В.1. Список всех пакетов


Все объекты, которые можно использовать в области расширенных событий, взяты из пакетов, которые загружены в систему. В этом разделе перечислены все пакеты и даны их описания.


```sql
SELECT  --C.1
        p.name         AS [Package],
        p.description  AS [Package-Description]
    FROM
        sys.dm_xe_packages  AS p
    ORDER BY
        p.name;
```


#### <a name="output"></a>Выходные данные

Ниже приведен список пакетов.

| Пакет        |Package-Description|
|---|---|
|файловый поток|     Расширенные события для FILESTREAM и FileTable в SQL Server |
|package0   |    Пакет по умолчанию. Содержит все стандартные типы, карты, операторы сравнения, действия и целевые объекты |
|qds         |   Расширенные события для хранилища запросов |
|SecAudit     |  События аудита безопасности |
|sqlclr        | Расширенные события для SQL CLR |
|sqlos         | Расширенные события для операционной системы SQL |
|SQLSatellite |  Расширенные события для SQL Satellite |
|sqlserver   |   Расширенные события для Microsoft SQL Server |
|sqlserver  |    Расширенные события для Microsoft SQL Server |
|sqlserver  |    Расширенные события для Microsoft SQL Server |
|sqlsni     |    Расширенные события для Microsoft SQL Server |
|ucs        |    Расширенные события для стека объединенных коммуникаций |
|XtpCompile |    Расширенные события для компиляции XTP |
|XtpEngine  |    Расширенные события для механизма XTP |
|XtpRuntime |    Расширенные события для среды выполнения XTP |


*Определения приведенных выше сокращений:*

- clr — среда выполнения .NET
- qds — хранилище данных запросов
- sni — сетевой интерфейс сервера
- ucs — стек единой системы связи
- xtp — экстремальная транзакционная обработка


<a name="section_C_2_count_object_type"></a>

### <a name="c2-count-of-every-object-type"></a>В.2. Количество каждого типа объекта


Здесь содержатся сведения о типах объектов, содержащихся в пакетах событий. Отображается полный список всех типов объектов, которые находятся в *sys.dm\_xe\_objects*, а также количество по каждому типу.


```sql
SELECT  --C.2
        Count(*)  AS [Count-of-Type],
        o.object_type
    FROM
        sys.dm_xe_objects  AS o
    GROUP BY
        o.object_type
    ORDER BY
        1  DESC;
```


#### <a name="output"></a>Выходные данные

Ниже приведено количество объектов по типу объекта. Здесь около 1915 объектов.

|Count-of-Type |   object_type |
|---|---|
|1303|            event |
|351  |           карта |
|84    |          message |
|77     |         pred_compare; |
|53     |        action |
|46     |         pred_source; |
|28     |         type |
|17     |         target |

<a name="section_C_3_select_all_available_objects"></a>

### <a name="c3-select-all-available-items-sorted-by-type"></a>В.3. SELECT для всех доступных элементов, отсортированных по типу


Следующая инструкция SELECT возвращает около 1915 строк, по одной для каждого объекта.



```sql
SELECT  --C.3
        o.object_type  AS [Type-of-Item],
        p.name         AS [Package],
        o.name         AS [Item],
        o.description  AS [Item-Description]
    FROM
             sys.dm_xe_objects  AS o
        JOIN sys.dm_xe_packages AS p  ON o.package_guid = p.guid
    WHERE
        o.object_type IN ('action' , 'target' , 'pred_source')
        AND
        (
            (o.capabilities & 1) = 0
            OR
            o.capabilities IS NULL
        )
    ORDER BY
        [Type-of-Item],
        [Package],
        [Item];
```


#### <a name="output"></a>Выходные данные

Чтобы удовлетворить ваш интерес, далее приводится произвольная выборка объектов, возвращенных предыдущей инструкцией SELECT.


|Type-of-Item|   Пакет|        Item|                          Item-Description|
|---|---|---|---|
|action|         package0  |     callstack                     |Получение текущего стека вызова|
|action |        package0  |     debug_break                   |Прерывание процесса в отладчике по умолчанию|
|action |       sqlos      |    task_time                     |Получение времени выполнения текущей задачи|
|action |        sqlserver |     sql_text                     | Получение текста SQL|
|event  |        qds       |     query_store_aprc_regression  | Возникает, когда хранилище запросов обнаруживает ухудшение производительности плана запроса|
|event  |        SQLSatellite |  connection_accept            | Происходит при принятии нового подключения. Это событие регистрирует все попытки подключения.|
|event  |        XtpCompile  |   cgen                          |Возникает в начале формирования кода C.|
|карта    |        qds         |   aprc_state                    |Состояние автоматического исправления снижения эффективности плана в хранилище запросов|
|message |       package0    |   histogram_event_required      |Если исходный тип равен 0, то параметру filtering_event_name необходимо задать значение.|
|pred_compare; |  package0   |    equal_ansi_string             |Оператор проверки на равенство для двух строковых значений ANSI|
|pred_compare; |  sqlserver  |    equal_i_sql_ansi_string       |Оператор равенства для двух строковых значений в формате SQL ANSI|
|pred_source; |   sqlos      |    task_execution_time           |Получение времени выполнения текущей задачи|
|pred_source; |   sqlserver  |    client_app_name               |Получение имени текущего клиентского приложения|
|target |        package0   |    etw_classic_sync_target       |Синхронная цель средства трассировки событий для Windows (ETW)|
|target |        package0   |    event_counter                 |Использование целевого параметра event_counter для подсчета экземпляров каждого события в сеансе событий.|
|target  |       package0  |     event_file                    |Используйте целевой объект event_file, чтобы сохранить данные события в XEL-файл, который можно заархивировать и использовать для последующего анализа и проверки. Можно выполнить слияние нескольких XEL-файлов для просмотра объединенных данных из отдельных сеансов событий.|
|target  |       package0   |    histogram                     |Используйте целевой объект гистограммы для объединения данных событий на основе определенного поля данных события или действия, связанного с событием. Гистограмма позволяет анализировать распределение данных событий за период сеанса событий.|
|target   |      package0  |     pair_matching                 |Целевой объект связывания|
|target   |      package0  |     ring_buffer                   |Целевой объект асинхронного кольцевого буфера|
|type     |      package0  |     Xml                           |XML-фрагмент правильного формата|

<a name="section_C_4_data_fields"></a>

### <a name="c4-data-fields-available-for-your-event"></a>В.4. Поля данных, доступные для события


Следующая инструкция SELECT возвращает все поля данных, относящиеся к вашему событию.

- Обратите внимание на элемент предложения WHERE: *column_type = 'data'* .
- Кроме того, потребуется изменить значение предложения WHERE для *o.name =* .


```sql
SELECT  -- C.4
        p.name         AS [Package],
        c.object_name  AS [Event],
        c.name         AS [Column-for-Predicate-Data],
        c.description  AS [Column-Description]
    FROM
              sys.dm_xe_object_columns  AS c
        JOIN  sys.dm_xe_objects         AS o

            ON  o.name = c.object_name

        JOIN  sys.dm_xe_packages        AS p

            ON  p.guid = o.package_guid
    WHERE
        c.column_type = 'data'
        AND
        o.object_type = 'event'
        AND
        o.name        = '\<EVENT-NAME-HERE!>'  --'lock_deadlock'
    ORDER BY
        [Package],
        [Event],
        [Column-for-Predicate-Data];
```


#### <a name="output"></a>Выходные данные

Предыдущая инструкция SELECT, WHERE `o.name = 'lock_deadlock'`возвратила следующие строки параметров:

- Каждая строка представляет дополнительный фильтр для события *sqlserver.lock_deadlock* .
- В следующем фрагменте отсутствует столбец *\[Column-Description\]* . Он часто имеет значение NULL.
- Это фактические выходные данные, за исключением опущенного столбца с описанием, который часто имеет значение NULL.
- В этих строках object_type = 'lock_deadlock'.

|Пакет|     Событие|           Column-for-Predicate-Data|
|---|---|---|
|sqlserver|   lock_deadlock|   associated_object_id|
|sqlserver|  lock_deadlock |  database_id|
|sqlserver|  lock_deadlock |  database_name|
|sqlserver|   lock_deadlock|   deadlock_id|
|sqlserver|   lock_deadlock|   длительность|
|sqlserver|   lock_deadlock|   lockspace_nest_id|
|sqlserver|   lock_deadlock|   lockspace_sub_id|
|sqlserver|   lock_deadlock|   lockspace_workspace_id|
|sqlserver|   lock_deadlock|   mode|
|sqlserver|   lock_deadlock|   object_id|
|sqlserver|   lock_deadlock|   owner_type|
|sqlserver|   lock_deadlock|   resource_0|
|sqlserver|   lock_deadlock|  resource_1|
|sqlserver|   lock_deadlock|   resource_2|
|sqlserver|   lock_deadlock|   resource_description|
|sqlserver|   lock_deadlock|   resource_type|
|sqlserver|   lock_deadlock|   transaction_id|




<a name="section_C_5_map_values_fields"></a>

### <a name="c5-sysdm_xe_map_values-and-event-fields"></a>В.5. *sys.dm_xe_map_values* и поля событий


Следующая инструкция SELECT содержит оператор JOIN для сложного представления с именем *sys.dm_xe_map_values*.

Назначение SELECT заключается в отображении множества полей, из которых можно выбрать необходимые для сеанса событий. Поля событий можно использовать двумя способами:

- для выбора значений полей, которые будут записываться в целевой объект для каждого вхождения события;
- для фильтрации вхождений событий, которые будут отправляться из целевого объекта или оставаться в нем.


```sql
SELECT  --C.5
        dp.name         AS [Package],
        do.name         AS [Object],
        do.object_type  AS [Object-Type],
        'o--c'     AS [O--C],
        dc.name         AS [Column],
        dc.type_name    AS [Column-Type-Name],
        dc.column_type  AS [Column-Type],
        dc.column_value AS [Column-Value],
        'c--m'     AS [C--M],
        dm.map_value    AS [Map-Value],
        dm.map_key      AS [Map-Key]
    FROM
              sys.dm_xe_objects         AS do
        JOIN  sys.dm_xe_object_columns  AS dc

            ON  dc.object_name = do.name

        JOIN  sys.dm_xe_map_values      AS dm

            ON  dm.name = dc.type_name

        JOIN  sys.dm_xe_packages        AS dp

            ON  dp.guid = do.package_guid
    WHERE
        do.object_type = 'event'
        AND
        do.name        = '\<YOUR-EVENT-NAME-HERE!>'  --'lock_deadlock'
    ORDER BY
        [Package],
        [Object],
        [Column],
        [Map-Value];
```


#### <a name="output"></a>Выходные данные

<a name="resource_type_dmv_actual_row"></a>

Далее приведена выборка фактических 153 строк выходных данных из предыдущего результата выполнения T-SQL SELECT. Строка для **resource_type** — [соответствующая](#resource_type_PAGE_cat_view) предикату фильтрации, используемый в **event_session_test3** пример в другом месте в этой статье.


```
/***  5 sampled rows from the actual 153 rows returned.
    NOTE:  'resource_type' under 'Column'.

Package     Object          Object-Type   O--C   Column          Column-Type-Name     Column-Type   Column-Value   C--M   Map-Value        Map-Key
-------     ------          -----------   ----   ------          ----------------     -----------   ------------   ----   ---------        -------
sqlserver   lock_deadlock   event         o--c   CHANNEL         etw_channel          readonly      2              c--m   Operational      4
sqlserver   lock_deadlock   event         o--c   KEYWORD         keyword_map          readonly      16             c--m   access_methods   1024
sqlserver   lock_deadlock   event         o--c   mode            lock_mode            data          NULL           c--m   IX               8
sqlserver   lock_deadlock   event         o--c   owner_type      lock_owner_type      data          NULL           c--m   Cursor           2
sqlserver   lock_deadlock   event         o--c   resource_type   lock_resource_type   data          NULL           c--m   PAGE             6

Therefore, on your CREATE EVENT SESSION statement, in its ADD EVENT WHERE clause,
you could put:
    WHERE( ... resource_type = 6 ...)  -- Meaning:  6 = PAGE.
***/
```


<a name="section_C_6_parameters_targets"></a>

### <a name="c6-parameters-for-targets"></a>В.6. Параметры для целевых объектов


Следующая инструкция SELECT возвращает каждый параметр для целевого объекта. Каждый параметр помечается, поэтому можно определить, является он обязательным или нет. Значения, назначаемые параметрам, влияют на поведение целевого объекта.

- Обратите внимание на элемент предложения WHERE: *object_type = 'customizable'* .
- Кроме того, потребуется изменить значение предложения WHERE для *o.name =* .


```sql
SELECT  --C.6
        p.name        AS [Package],
        o.name        AS [Target],
        c.name        AS [Parameter],
        c.type_name   AS [Parameter-Type],

        CASE c.capabilities_desc
            WHEN 'mandatory' THEN 'YES_Mandatory'
            ELSE 'Not_mandatory'
        END  AS [IsMandatoryYN],

        c.description AS [Parameter-Description]
    FROM
              sys.dm_xe_objects   AS o
        JOIN  sys.dm_xe_packages  AS p

            ON  o.package_guid = p.guid

        LEFT OUTER JOIN  sys.dm_xe_object_columns  AS c

            ON  o.name        = c.object_name
            AND c.column_type = 'customizable'  -- !
    WHERE
        o.object_type = 'target'
        AND
        o.name     LIKE '%'    -- Or '\<YOUR-TARGET-NAME-HERE!>'.
    ORDER BY
        [Package],
        [Target],
        [IsMandatoryYN]  DESC,
        [Parameter];
```


#### <a name="output"></a>Выходные данные

Приведенные ниже строки параметров являются подмножеством данных, возвращенных предыдущей инструкцией SELECT в SQL Server 2016.


```
/***  Actual output, all rows, where target name = 'event_file'.
Package    Target       Parameter            Parameter-Type       IsMandatoryYN   Parameter-Description
-------    ------       ---------            --------------       -------------   ---------------------
package0   event_file   filename             unicode_string_ptr   YES_Mandatory   Specifies the location and file name of the log
package0   event_file   increment            uint64               Not_mandatory   Size in MB to grow the file
package0   event_file   lazy_create_blob     boolean              Not_mandatory   Create blob upon publishing of first event buffer, not before.
package0   event_file   max_file_size        uint64               Not_mandatory   Maximum file size in MB
package0   event_file   max_rollover_files   uint32               Not_mandatory   Maximum number of files to retain
package0   event_file   metadatafile         unicode_string_ptr   Not_mandatory   Not used
***/
```


<a name="section_C_7_dmv_select_target_data_column"></a>

### <a name="c7-dmv-select-casting-target_data-column-to-xml"></a>В.7. Инструкция SELECT DMV, приводящая столбец target_data к формату XML


Эта инструкция DMV SELECT возвращает строки данных из целевого объекта открытого сеанса событий. Данные приведены к формату XML, поэтому возвращенную ячейку можно активировать щелчком мыши для простоты отображения в среде.

- Если сеанс событий остановлен, SELECT не возвратит ни одной строки.
- Потребуется изменить значение предложения WHERE для *s.name =* .


```sql
SELECT  --C.7
        s.name,
        t.target_name,
        CAST(t.target_data AS XML)  AS [XML-Cast]
    FROM
              sys.dm_xe_session_targets  AS t
        JOIN  sys.dm_xe_sessions         AS s

            ON s.address = t.event_session_address
    WHERE
        s.name = '\<Your-Session-Name-Here!>';
```


#### <a name="output-the-only-row-including-its-xml-cell"></a>Выходные данные в виде единственной строки, включая ячейку XML

Здесь приведена единственная строка, являющаяся выходными данными выполнения предыдущей инструкции SELECT. Столбец *XML-Cast* содержит строку XML-данных, которую среда SSMS считает XML. Поэтому SSMS понимает, что ячейку XML-Cast необходимо сделать активной по щелчку.


Для данного запуска:

- *S.name =* значение, заданное для сеанса событий для события *checkpoint_begin* .
- Целевой объект — *ring_buffer*.


```XML
name                              target_name   XML-Cast
----                              -----------   --------
checkpoint_session_ring_buffer2   ring_buffer   <RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="104"><event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:23.508Z"><data name="database_id"><type name="uint32" package="package0" /><value>5</value></data></event><event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:26.975Z"><data name="database_id"><type name="uint32" package="package0" /><value>5</value></data></event></RingBufferTarget>
```


#### <a name="output-xml-displayed-pretty-when-cell-is-clicked"></a>Выходные данные, XML-данные, отображаемые после щелчка ячейки


После щелчка ячейки XML-Cast отображаются следующие данные.


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="104">
  <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:23.508Z">
    <data name="database_id">
      <type name="uint32" package="package0" />
      <value>5</value>
    </data>
  </event>
  <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T01:28:26.975Z">
    <data name="database_id">
      <type name="uint32" package="package0" />
      <value>5</value>
    </data>
  </event>
</RingBufferTarget>
```


<a name="section_C_8_select_function_disk"></a>

### <a name="c8-select-from-a-function-to-retrieve-event_file-data-from-disk-drive"></a>В.8. Использование SELECT в функции для получения данных event_file с диска


Предположим, что сеанс событий собрал некоторые данные и позднее был остановлен. Если сеанс был определен для использования целевого объекта event_file, вы по-прежнему можете получать данные путем вызова функции *sys.fn_xe_target_read_file*.

- Перед запуском этой инструкции SELECT необходимо изменить путь и имя файла в параметре вызова функции.
    - Не обращайте внимание на дополнительные цифры, которые система SQL будет внедрять в реальные имена XEL-файлов при каждом перезапуске сеанса. Просто укажите обычное корневое имя и расширение.


```sql
SELECT  --C.8
        f.module_guid,
        f.package_guid,
        f.object_name,
        f.file_name,
        f.file_offset,
        CAST(f.event_data AS XML)  AS [Event-Data-As-XML]
    FROM
        sys.fn_xe_file_target_read_file(

            '\<YOUR-PATH-FILE-NAME-ROOT-HERE!>*.xel',
            --'C:\Junk\Checkpoint_Begins_ES*.xel',  -- Example.

            NULL, NULL, NULL
        )  AS f;
```


#### <a name="output-rows-returned-by-select-from-the-function"></a>Выходные данные, строки, возвращенные SELECT FROM (выбор из функции)


Далее приведены строки, возвращенные предыдущим запросом SELECT FROM. Крайний правый столбец XML содержит данные, касающиеся вхождения события.


```
module_guid                            package_guid                           object_name        file_name                                                           file_offset   Event-Data-As-XML
-----------                            ------------                           -----------        ---------                                                           -----------   -----------------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_begin   C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5120          <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:14.023Z"><data name="database_id"><value>5</value></data><action name="session_id" package="sqlserver"><value>60</value></action><action name="database_id" package="sqlserver"><value>5</value></action></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_end     C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5120          <event name="checkpoint_end" package="sqlserver" timestamp="2016-07-09T03:30:14.025Z"><data name="database_id"><value>5</value></data></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_begin   C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5632          <event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:17.704Z"><data name="database_id"><value>5</value></data><action name="session_id" package="sqlserver"><value>60</value></action><action name="database_id" package="sqlserver"><value>5</value></action></event>
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   checkpoint_end     C:\Junk\Checkpoint_Begins_ES_20160615bb-_0_131125086091700000.xel   5632          <event name="checkpoint_end" package="sqlserver" timestamp="2016-07-09T03:30:17.709Z"><data name="database_id"><value>5</value></data></event>
```


#### <a name="output-one-xml-cell"></a>Выходные данные, одна ячейка XML


Далее приведено содержимое первой ячейки XML из предыдущего возвращенного набора строк.


```xml
<event name="checkpoint_begin" package="sqlserver" timestamp="2016-07-09T03:30:14.023Z">
  <data name="database_id">
    <value>5</value>
  </data>
  <action name="session_id" package="sqlserver">
    <value>60</value>
  </action>
  <action name="database_id" package="sqlserver">
    <value>5</value>
  </action>
</event>
```
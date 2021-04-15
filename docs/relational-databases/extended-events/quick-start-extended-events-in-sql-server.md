---
title: Краткое руководство. Расширенные события в SQL Server
description: Это краткое руководство поможет вам использовать расширенные события, которые являются упрощенной системой мониторинга производительности, позволяющей получать данные, необходимые для отслеживания и устранения неполадок в SQL Server.
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xevents
ms.topic: quickstart
f1_keywords:
- sql11.ssms.XeNewEventSession.General.f1
- sql11.ssms.XeNewEventSession.Events.f1
- sql11.ssms.XeNewEventSession.Targets.f1
- sql11.ssms.XeNewEventSession.Advanced.f1
ms.assetid: 7bb78b25-3433-4edb-a2ec-c8b2fa58dea1
author: rothja
ms.author: jroth
ms.reviewer: maghan
ms.date: 04/16/2020
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9459b2a2b01670a0c6201586597ba02eb1218853
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492401"
---
# <a name="quickstart-extended-events-in-sql-server"></a>Краткое руководство. Расширенные события в SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Расширенные события — это упрощенная система мониторинга производительности, позволяющая пользователям получать данные, необходимые для отслеживания и устранения неполадок в SQL Server. Дополнительные сведения об архитектуре расширенных событий см. в разделе [Общие сведения о расширенных событиях](extended-events.md).  Эта статья призвана помочь разработчикам SQL, которые незнакомы с расширенными событиями, создать сеанс событий за считанные минуты. С помощью расширенных событий можно получать подробные сведения о внутренних процессах, происходящих в системе SQL и приложении. При создании сеанса расширенных событий вы сообщаете системе:

- какие события вас интересуют;
- как система должна предоставлять вам данные.

В этой статье приводятся следующие сведения:

- С помощью снимков экрана иллюстрируются действия в SSMS.exe по созданию сеанса событий.
- Снимки экрана сопоставляются с эквивалентными инструкциями Transact-SQL.
- Подробно описываются термины и понятия, связанные с интерфейсом и инструкциями T-SQL для сеансов событий.
- Показано, как протестировать сеанс событий.
- Описываются варианты обработки результатов:
  - регистрация и сохранение результатов;
  - обработанные и необработанные результаты;
  - средства для просмотра результатов различными способами и в различном масштабе времени.
- Показано, как можно найти все доступные события.
- Описываются неявные отношения между первичными и внешними ключами, характерные для динамических административных представлений для расширенных событий.
- Описывается содержимое связанных разделов.

В блогах и других неофициальных материалах расширенные события иногда называются *XEvent*.

> [!NOTE]
> Сведения о расширенных событиях в Базе данных SQL Azure, включая примеры кода, см. в разделе [Расширенные события в Базе данных SQL](/azure/azure-sql/database/xevent-db-diff-from-svr).

## <a name="preparations-before-demo"></a>Подготовительные действия перед демонстрацией

Для проведения демонстрации необходимо выполнить перечисленные ниже предварительные условия.

1. [Скачивание SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)

   Каждый месяц следует устанавливать последнее ежемесячное обновление для SSMS.
2. Войдите в Microsoft SQL Server 2014 или более поздней версии.
3. Ваша учетная запись должна иметь [разрешение сервера](../../t-sql/statements/grant-server-permissions-transact-sql.md)**ALTER ANY EVENT SESSION**.

  Дополнительные сведения о безопасности и разрешениях, связанных с расширенными событиями, можно найти в конце этого раздела в [приложении](#appendix1).

## <a name="demo-of-ssms-integration"></a>Демонстрация интеграции с SSMS

SSMS.exe предоставляет отличный пользовательский интерфейс для расширенных событий. Его возможности столь широки, что многим пользователям не потребуется прибегать к Transact-SQL или динамическим административным представлениям для работы с расширенными событиями.

В этом разделе вы ознакомитесь с действиями, которые необходимо выполнить в пользовательском интерфейсе для создания расширенного события и просмотра его данных. Далее вы сможете более глубоко ознакомиться с принципами, стоящими за выполняемыми действиями.

### <a name="steps-of-demo"></a>Этапы демонстрации

Вы можете изучить демонстрируемые действия, даже если не собираетесь их выполнять. Демонстрация начинается с диалогового окна **Создание сеанса** . Оно содержит четыре страницы:

- Общие сведения
- События
- Хранилище данных
- Дополнительно

В результате обновления пользовательского интерфейса SSMS текст и сопроводительные снимки экрана могут стать с течением времени немного неточными. Однако если расхождения незначительны, снимки экрана по-прежнему можно использовать в качестве иллюстраций.

1. Подключитесь к SSMS.

2. В обозревателе объектов щелкните **Управление** > **Расширенные события** > **Создать сеанс**. Диалоговое окно **Создание сеанса** предпочтительнее **мастера создания сеанса**, хотя во многом они схожи.

3. В левом верхнем углу выберите страницу **Общие** . Затем введите *МойСеанс* или любое другое имя в текстовом поле **Имя сеанса** . *Не* нажимайте сейчас кнопку **ОК** — это нужно будет сделать только в конце демонстрации.

   ![Создание сеанса > Общие > Имя сеанса](../../relational-databases/extended-events/media/xevents-session-newsessions-10-general-ssms-yoursessionnode.png)

4. В левом верхнем углу выберите страницу **События** и нажмите кнопку **Выбрать** .

   ![Создание сеанса > События > Выбрать > Библиотека событий, Выбранные события](../../relational-databases/extended-events/media/xevents-session-newsessions-14-events-ssms-rightclick-not-wizard.png)

5. В области **Библиотека событий** в раскрывающемся списке выберите элемент **Только имена событий**.
    - В текстовом поле введите **sql**, чтобы отфильтровать длинный список доступных событий с помощью оператора *contains* .
    - Прокрутите список и щелкните событие с именем **sql_statement_completed**.
    - Нажмите кнопку со стрелкой вправо **>** , чтобы переместить событие в поле **Выбранные события** .

6. Оставаясь на странице **События** , нажмите кнопку **Настроить** в правой ее части.

   На приведенном ниже снимке экрана, левая часть которого обрезана для наглядности, показана область **Параметры конфигурации событий**.

    ![Создание сеанса > События > Настроить > Фильтр (предикат) > Поле](../../relational-databases/extended-events/media/xevents-session-newsessions-20b-events-ssms-yoursessionnode.png)

7. Перейдите на вкладку **Фильтр (предикат)** . Затем щелкните **Щелкните здесь, чтобы добавить предложение** для записи всех инструкций SQL SELECT с предложением HAVING.

8. В раскрывающемся списке **Поле** выберите **sqlserver.sql_text**.
   - В списке **Оператор** выберите оператор LIKE.
   - В поле **Значение** введите **%SELECT%HAVING%** .

   > [!NOTE]
   > В этом двухкомпонентном имени *sqlserver* — это имя пакета, а *sql_text* — имя поля. Ранее выбранное событие *sql_statement_completed* должно находиться в том же пакете, что и выбранное поле.

9. В левом верхнем углу выберите страницу **Хранилище данных** .

10. В области **Назначения** щелкните **Щелкните здесь, чтобы добавить назначение**.
    - В раскрывающемся списке **Тип** выберите элемент **event_file**.
    - Это значит, что данные события будут сохранены в файле, который можно просмотреть.
    
    > [!NOTE]
    > Хранилище BLOB-объектов Azure нельзя использовать в качестве целевого расположения хранилища данных в локальном экземпляре SQL Server.

    ![Создание сеанса > Хранилище данных > Назначения > Тип > event_file](../../relational-databases/extended-events/media/xevents-session-newsessions-30-datastorage-ssms-yoursessionnode.png)

11. В области **Свойства** в текстовом поле **Имя файла на сервере** введите полный путь к файлу и его имя.
    - Файл должен иметь расширение *.xel*.
    - Для нашего небольшого теста размер файла будет меньше 1 МБ.

    ![Создание сеанса > Дополнительно > Максимальная задержка диспетчеризации > ОК](../../relational-databases/extended-events/media/xevents-session-newsessions-40-advanced-ssms-yoursessionnode.png)

12. В левом верхнем углу выберите страницу **Дополнительно**.
    - Уменьшите значение параметра **Максимальная задержка диспетчеризации** до 3 секунд.
    - Наконец, нажмите кнопку **ОК** в нижней части окна.

13. Вернувшись в **обозреватель объектов**, разверните папку **Управление** > **Сеансы**, чтобы увидеть узел **МойСеанс**.

    ![Узел нового сеанса событий с именем "МойСеанс" в обозревателе событий в папке "Управление" > "Расширенные события" > "Сеансы"](../../relational-databases/extended-events/media/xevents-session-newsessions-50-objectexplorer-ssms-yoursessionnode.png)

#### <a name="edit-your-event-session"></a>Редактирование сеанса событий

В **обозревателе объектов** SSMS можно изменить сеанс событий, щелкнув его узел правой кнопкой мыши и выбрав пункт **Свойства**. Откроется то же самое диалоговое окно с несколькими страницами.

### <a name="corresponding-t-sql-for-your-event-session"></a>Соответствующие инструкции T-SQL для сеанса событий

Вы использовали пользовательский интерфейс SSMS, чтобы сформировать скрипт T-SQL для создания сеанса событий. Чтобы просмотреть сформированный скрипт, выполните указанные ниже действия.

- Щелкните узел сеанса правой кнопкой мыши и выберите пункты **Создать скрипт для сеанса** > **CREATE в** > **Буфер обмена**.
- Вставьте скрипт в любой текстовый редактор.

Ниже приведена инструкция T-SQL CREATE EVENT SESSION для сеанса *МойСеанс*, которая была сформирована в результате выполнения действий в пользовательском интерфейсе.

```sql
CREATE EVENT SESSION [YourSession]
    ON SERVER
    ADD EVENT sqlserver.sql_statement_completed
    (
        ACTION(sqlserver.sql_text)
        WHERE
        ( [sqlserver].[like_i_sql_unicode_string]([sqlserver].[sql_text], N'%SELECT%HAVING%')
        )
    )
    ADD TARGET package0.event_file
    (SET
        filename = N'C:\Junk\YourSession_Target.xel',
        max_file_size = (2),
        max_rollover_files = (2)
    )
    WITH (
        MAX_MEMORY = 2048 KB,
        EVENT_RETENTION_MODE = ALLOW_MULTIPLE_EVENT_LOSS,
        MAX_DISPATCH_LATENCY = 3 SECONDS,
        MAX_EVENT_SIZE = 0 KB,
        MEMORY_PARTITION_MODE = NONE,
        TRACK_CAUSALITY = OFF,
        STARTUP_STATE = OFF
    );
GO
```

#### <a name="pre-drop-of-the-event-session"></a>Предварительное выполнение инструкции DROP для сеанса событий

Перед выполнением инструкции CREATE EVENT SESSION можно выполнить инструкцию DROP EVENT SESSION на тот случай, если имя сеанса уже существует.

```sql
IF EXISTS (SELECT *
      FROM sys.server_event_sessions
      WHERE name = 'YourSession')
BEGIN
    DROP EVENT SESSION YourSession
          ON SERVER;
END
go
```

#### <a name="alter-to-start-and-stop-the-event-session"></a>Инструкция ALTER для запуска и остановки сеанса событий

Созданный сеанс событий по умолчанию не запускается автоматически. Вы можете запустить или остановить сеанс событий в любой момент с помощью приведенной ниже инструкции T-SQL ALTER EVENT SESSION.

```sql
ALTER EVENT SESSION [YourSession]
      ON SERVER
    --ON DATABASE
    STATE = START;   -- STOP;
```

Вы также можете указать, что сеанс событий должен запускаться автоматически при запуске экземпляра SQL Server. См. ключевое слово **STARTUP STATE = ON** в инструкции CREATE EVENT SESSION.

- В пользовательском интерфейсе SSMS имеется соответствующий флажок на странице **Создание сеанса** > **Общие** .

## <a name="test-your-event-session"></a>Тестирование сеанса событий

Чтобы протестировать сеанс событий, выполните указанные ниже несложные действия.

1. В **обозревателе объектов** SSMS щелкните узел сеанса событий правой кнопкой мыши и выберите пункт **Начать сеанс**.
2. Выполните приведенную ниже инструкцию `SELECT...HAVING` два раза.
    - В идеале следует поменять значение `HAVING Count` между запусками с 2 на 3. Таким образом можно увидеть различия в результатах.
3. Щелкните узел сеанса правой кнопкой мыши и выберите пункт **Остановить сеанс**.
4. Прочитайте следующий подраздел о [выборе и просмотре результатов](#select-the-full-results-xml-37).

```sql
SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o
            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) >= 3   --2     -- Try both values during session.
    ORDER BY
        c.name;
```

Для полноты ниже приведен образец результата предыдущей инструкции SELECT...HAVING.

```
/*** Approximate output, 6 rows, all HAVING Count >= 3:
name                   Count-Per-Column-Repeated-Name
---------------------  ------------------------------
event_group_type       4
event_group_type_desc  4
event_session_address  5
event_session_id       5
is_trigger_event       4
trace_event_id         3
***/
```

<a name="select-the-full-results-xml-37"/>

### <a name="select-the-full-results-as-xml"></a>Выбор полных результатов в формате XML

В SSMS выполните приведенную ниже инструкцию T-SQL SELECT, чтобы получить результаты, каждая строка которых содержит данные по отдельному экземпляру события. С помощью инструкции CAST AS XML можно легко просмотреть результаты.

> [!NOTE]
> Система событий всегда добавляет длинный номер к указанному имени *XEL* -файла event_file. Перед выполнением приведенной ниже инструкции SELECT из файла необходимо скопировать полное имя, предоставленное системой, и вставить его в инструкцию SELECT.

```sql
SELECT
        object_name,
        file_name,
        file_offset,
        event_data,
        'CLICK_NEXT_CELL_TO_BROWSE_XML RESULTS!'
                AS [CLICK_NEXT_CELL_TO_BROWSE_XML_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]
                -- TODO: In ssms.exe results grid, double-click this xml cell!
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\Junk\YourSession_Target_0_131085363367310000.xel',
            null, null, null
        );
```

Приведенная выше инструкция SELECT позволяет просмотреть полные результаты из любой строки двумя способами.

- Выполните инструкцию SELECT в SSMS, а затем щелкните ячейку в столбце **event_data_XML** . Это очень удобно.
- Скопируйте длинную строку XML из ячейки в столбце **event_data** . Вставьте строку в любой простой текстовый редактор, например Notepad.exe, и сохраните ее в файле с расширением XML. Затем откройте XML-файл в браузере.

#### <a name="display-of-results-for-one-event"></a>Отображение результатов для одного события

Ниже приведена часть результатов в формате XML. Этот код XML сокращен для наглядности. Обратите внимание на то, что в элементе `<data name="row_count">` показано значение `6`, что соответствует 6 строкам результатов, показанным ранее. И мы можем видеть всю инструкцию SELECT.

```xml
<event name="sql_statement_completed" package="sqlserver" timestamp="2016-05-24T04:06:08.997Z">
  <data name="duration">
    <value>111021</value>
  </data>
  <data name="cpu_time">
    <value>109000</value>
  </data>
  <data name="physical_reads">
    <value>0</value>
  </data>
  <data name="last_row_count">
    <value>6</value>
  </data>
  <data name="offset">
    <value>0</value>
  </data>
  <data name="offset_end">
    <value>584</value>
  </data>
  <data name="statement">
    <value>SELECT
        c.name,
        Count(*)  AS [Count-Per-Column-Repeated-Name]
    FROM
             sys.syscolumns  AS c
        JOIN sys.sysobjects  AS o

            ON o.id = c.id
    WHERE
        o.type = 'V'
        AND
        c.name like '%event%'
    GROUP BY
        c.name
    HAVING
        Count(*) &gt;= 3   --2     -- Try both values during session.
    ORDER BY
        c.name</value>
  </data>
</event>
```

## <a name="ssms-to-display-results"></a>Вывод результатов в SSMS

В пользовательском интерфейсе SSMS есть несколько функций, с помощью которых можно просматривать результаты, полученные из расширенного события. Подробные сведения см. в следующем разделе:

- [Расширенный просмотр целевых данных из расширенных событий в SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)

Основными функциями являются пункты контекстного меню **Просмотр целевых данных** и **Интерактивный просмотр данных**.

### <a name="view-target-data"></a>Просмотр целевых данных

В **обозревателе объектов** SSMS можно щелкнуть правой кнопкой мыши целевой узел, находящийся под узлом сеанса событий. В контекстном меню выберите пункт **Просмотр целевых данных**. В SSMS будут выведены данные.

По мере получения новых данных от события отображаемые данные не обновляются. Но вы можете еще раз выбрать пункт **Просмотр целевых данных** .

![Пункт "Просмотр целевых данных" в контекстном меню узла SSMS "Управление" > "Расширенные события" > "Сеансы" > "МойСеанс" > "package0.event_file"](../../relational-databases/extended-events/media/xevents-viewtargetdata-ssms-targetnode-61.png)

### <a name="watch-live-data"></a>Интерактивный просмотр данных

В **обозревателе объектов** SSMS можно щелкнуть правой кнопкой мыши узел сеанса событий. В контекстном меню выберите пункт **Интерактивный просмотр данных**. В SSMS будут в режиме реального времени выводиться поступающие данные.

![Пункт "Интерактивный просмотр данных" в контекстном меню узла SSMS "Управление" > "Расширенные события" > "Сеансы" > "МойСеанс"](../../relational-databases/extended-events/media/xevents-watchlivedata-ssms-yoursessionnode-63.png)

## <a name="scenarios"></a>Сценарии

Существует бесчисленное множество ситуаций, в которых можно эффективно использовать расширенные события. В приведенных ниже статьях рассматриваются примеры ситуаций, в которых во время выполнения запросов возникают блокировки.

Сценарии использования событий для возникновения блокировок описываются в приведенных ниже статьях. В них также рассматривается ряд особых приемов, таких как использование **\@dbid** и динамической инструкции `EXECUTE (@YourSqlString)`:

- [Поиск объектов, на которые наложено наибольшее число блокировок](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)
  - В этом сценарии используется целевая гистограмма package0.histogram, которая обрабатывает необработанные данные события перед их отображением.
- [Определение запросов, содержащих блокировки](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)
  - В этом примере используется [целевое сопоставление package0.pair_matching](/previous-versions/sql/sql-server-2016/ff878062(v=sql.130)), в котором парой событий является is sqlserver.lock_acquire и lock_release.

## <a name="terms-and-concepts-in-extended-events"></a>Термины и понятия, связанные с расширенными событиями

В приведенной ниже таблице перечислены термины, используемые в связи с расширенными событиями, и объясняется их смысл.

| Термин | Описание |
| :--- | :---------- |
| сеанс событий | Целью является конструкция, основанная на одном или нескольких событиях, а также вспомогательные элементы, такие как действия. Инструкция CREATE EVENT SESSION создает каждый сеанс событий. С помощью инструкции ALTER можно по желанию запускать и останавливать сеансы. <br/> <br/> Сеанс событий часто называется просто *сеансом*, если из контекста понятно, что имеется в виду именно *сеанс событий*. <br/> <br/> Более подробные сведения о сеансах событий см. в статье [Сеансы расширенных событий SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md). |
| event | Определенное событие в системе, наступление которого отслеживается активным сеансом событий. <br/> <br/> Например, событие *sql_statement_completed* представляет момент завершения какой-либо инструкции T-SQL. Событие может сообщать различные данные, например длительность. |
| target | Элемент, который получает выходные данные из регистрируемого события. Служит для вывода данных. <br/> <br/> Примерами могут служить *event_file* и его облегченная версия *ring_buffer*, хранимая в памяти. Более сложная целевая *гистограмма* выполняет ряд задач по обработке данных перед их выводом. <br/> <br/> Любой целевой объект можно использовать для любого сеанса событий. Дополнительные сведения см. в разделе [Целевые объекты для расширенных событий в SQL Server](../../relational-databases/extended-events/targets-for-extended-events-in-sql-server.md). |
| action | Поле, известное событию. Данные из этого поля отправляются в целевой объект. Поле действия тесно связано с *фильтром предиката*. |
| фильтром предиката | Проверка данных в поле события, благодаря которой только нужное подмножество экземпляров события отправляется целевому объекту. <br/> <br/> Например, фильтр может включать только те экземпляры события *sql_statement_completed* , в которых инструкция T-SQL содержит строку *HAVING*. |
| Пакет | Квалификатор имени, добавляемый к каждому элементу в наборе элементов, связанном с пакетом событий. <br/> <br/> Например, пакет событий может включать события, связанные с текстом T-SQL. Одно из событий может быть связано с кодом T-SQL в пакете, отделенном командой GO. А другое более частное событие может быть связано с отдельными инструкциями T-SQL. Кроме того, для каждой инструкции T-SQL есть события начала и завершения. <br/> <br/> Соответствующие событиям поля также содержатся в пакете с событиями. Большинство целевых объектов находятся в пакете *package0* и используются с событиями из многих других пакетов. |

## <a name="how-to-discover-the-available-events-in-packages"></a>Обнаружение других событий в пакетах

Приведенная ниже инструкция T-SQL SELECT возвращает строку для каждого доступного события, имя которого содержит строку из трех символов "sql". Конечно, можно изменить значение LIKE для поиска других имен событий. В строках также указывается имя пакета, содержащего событие.

```sql
SELECT   -- Find an event you want.
        p.name         AS [Package-Name],
        o.object_type,
        o.name         AS [Object-Name],
        o.description  AS [Object-Descr],
        p.guid         AS [Package-Guid]
    FROM
              sys.dm_xe_packages  AS p
        JOIN  sys.dm_xe_objects   AS o
                ON  p.guid = o.package_guid
    WHERE
        o.object_type = 'event'   --'action'  --'target'
        AND
        p.name LIKE '%'
        AND
        o.name LIKE '%sql%'
    ORDER BY
        p.name, o.object_type, o.name;
```

Ниже показана возвращаемая строка, формат которой изменен на следующий: имя столбца = значение. Данные получены от события *sql-statement_completed* , которое использовалось в предыдущем примере. Предложение для столбца Object-Descr особенно полезно.

```
Package-Name = sqlserver
object_type  = event
Object-Name  = sql_statement_completed
Object-Descr = Occurs when a Transact-SQL statement has completed.
Package-Guid = 655FD93F-3364-40D5-B2BA-330F7FFB6491
```

### <a name="ssms-ui-for-search"></a>Пользовательский интерфейс SSMS для поиска

Другой способ поиска — использование диалогового окна **Создание сеанса** > **События** > **Библиотека событий** в пользовательском интерфейсе SSMS, которое показано на предыдущем снимке экрана.

### <a name="sql-trace-event-classes-with-extended-events"></a>Классы событий SQL Trace с расширенными событиями

Описание использования расширенных событий с классами событий и столбцами трассировки SQL можно найти в следующей статье: [Просмотр эквивалентов расширенных событий для классов событий трассировки SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)

### <a name="event-tracing-for-windows-etw-with-extended-events"></a>Трассировка событий Windows с расширенными событиями

Описание использования расширенных событий с трассировкой событий Windows можно найти в следующих разделах:

- [Цель "Средство трассировки событий для Windows"](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [Мониторинг активности системы с помощью расширенных событий](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)

## <a name="additional-items"></a>Дополнительные элементы

В этом разделе кратко рассматривается ряд других элементов.

### <a name="event-sessions-installed-with-sql-server"></a>Сеансы событий, устанавливаемые вместе с SQL Server

В состав SQL Server входит ряд готовых расширенных событий. Все они настроены так, чтобы запускаться при запуске системы SQL. Эти сеансы событий собирают данные, которые могут быть полезны в случае системной ошибки. Как и все расширенные события, они используют незначительное количество ресурсов, поэтому корпорация Майкрософт рекомендует не отключать их.

Просмотреть события сеансов можно в **обозревателе объектов** SSMS на странице **Управление** > **Расширенные события** > **Сеансы**.  На июнь 2016 г. список устанавливаемых сеансов событий следующий:

- AlwaysOn_health
- system_health
- telemetry_events

### <a name="powershell-provider-for-extended-events"></a>Поставщик PowerShell для расширенных событий

Управлять расширенными событиями SQL Server можно с помощью поставщика SQL Server PowerShell. Подробные сведения см. в следующем разделе: [Использование поставщика PowerShell для расширенных событий](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)

### <a name="system-views-for-extended-events"></a>Системные представления для расширенных событий

В число системных представлений для расширенных событий входят следующие представления:

- *представления каталога:* для сведений о сеансах событий, которые были определены инструкцией CREATE EVENT SESSION;

- *динамические административные представления:* для сведений о сеансах событий, активных в настоящее время.

В разделе[Использование SELECT и JOIN в системных представлениях для расширенных событий в SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) приводятся следующие сведения:

- инструкции по присоединению представлений друг к другу;
- несколько полезных инструкций SELECT для представлений;
- взаимосвязь между:
  - столбцами представлений;
  - предложениями CREATE EVENT SESSION;
  - элементами управления пользовательского интерфейса SSMS.

## <a name="appendix-selects-to-ascertain-permission-owner-in-advance"></a><a name="appendix1"></a> Приложение. Инструкции SELECT для предварительного определения владельца разрешения

В этом разделе упоминаются следующие разрешения:

- ALTER ANY EVENT SESSION
- VIEW SERVER STATE
- CONTROL SERVER

Приведенные ниже инструкции Transact-SQL SELECT могут сообщить, кто имеет эти разрешения.

### <a name="union-direct-permissions-plus-role-derived-permissions"></a>Прямые разрешения UNION, а также разрешения, производные от роли

Приведенная ниже инструкция SELECT...UNION ALL возвращает строки, которые показывают, у кого есть необходимые разрешения для создания сеансов событий и запроса расширенных событий из системных представлений каталога.

```sql
-- Ascertain who has the permissions listed in the ON clause.
-- 'CONTROL SERVER' permission includes the permissions
-- 'ALTER ANY EVENT SESSION' and 'VIEW SERVER STATE'.
SELECT
        'Owner-is-Principal'  AS [Type-That-Owns-Permission],
        NULL                  AS [Role-Name],
        prin.name             AS [Owner-Name],

        perm.permission_name
            COLLATE Latin1_General_CI_AS_KS_WS
            AS [Permission-Name]
    FROM
             sys.server_permissions  AS perm
        JOIN sys.server_principals   AS prin

            ON prin.principal_id = perm.grantee_principal_id
    WHERE
        perm.permission_name IN
            ('ALTER ANY EVENT SESSION',
            'VIEW SERVER STATE',
            'CONTROL SERVER')
UNION ALL

-- Plus check for members of the 'sysadmin' fixed server role,
-- because 'sysadmin' includes the 'CONTROL SERVER' permission.
SELECT
        'Owner-is-Role'
        , prin.name  -- [Role-Name]
        , CAST( (IsNull(pri2.name, N'No members'))
            AS nvarchar(128))
        , NULL
    FROM
        sys.server_role_members  AS rolm
        RIGHT OUTER JOIN sys.server_principals    AS prin
            ON prin.principal_id = rolm.role_principal_id
        LEFT OUTER JOIN sys.server_principals     AS pri2
            ON rolm.member_principal_id = pri2.principal_id
    WHERE
        prin.name = 'sysadmin'
    ORDER BY
        1,2,3,4;
```

### <a name="has_perms_by_name-function"></a>HAS_PERMS_BY_NAME , функция

Приведенная ниже инструкция SELECT сообщает ваши разрешения. В ней используется встроенная функция [HAS_PERMS_BY_NAME](../../t-sql/functions/has-perms-by-name-transact-sql.md).

Кроме того, если у вас есть право на временное *олицетворение* других учетных записей, вы можете раскомментировать инструкции [EXECUTE AS LOGIN](../../t-sql/statements/execute-as-transact-sql.md) и REVERT, чтобы получить сведения о других учетных записях.

```sql
--EXECUTE AS LOGIN = 'AccountNameHere';
SELECT HAS_PERMS_BY_NAME
    (
       null
       , null
       , 'ALTER ANY EVENT SESSION'
    );
--REVERT;
```

### <a name="security-links"></a>Ссылки на сведения о безопасности

Ниже приведены ссылки на документацию по этим инструкциям SELECT и разрешениям.

- Подробные сведения о встроенной функции [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)
- [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)
- [GRANT, предоставление разрешений на сервер (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)
- [sys.server_principals (Transact-SQL)](../system-catalog-views/sys-server-principals-transact-sql.md)
- Блог: [Эффективные разрешения для ядра СУБД](https://social.technet.microsoft.com/wiki/contents/articles/15180.effective-database-engine-permissions.aspx)
- Масштабируемый [плакат](https://aka.ms/sql-permissions-poster)в формате PDF, на котором показана иерархия всех разрешений SQL Server.

## <a name="links-to-supporting-information"></a>Ссылки на вспомогательную информацию

- [sys.fn_xe_file_target_read_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md)
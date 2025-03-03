---
title: Автоматическая настройка
description: Сведения об автоматической настройке в SQL Server и базе данных SQL Azure
ms.custom: fasttrack-edit
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- performance tuning [SQL Server]
- automatic tuning
- aprc
- automatic plan regression correction
- last known good plan
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ea61597a7f9874b7438392167380c5f01b11527
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551670"
---
# <a name="automatic-tuning"></a>Автоматическая настройка
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

Функция автоматической настройки базы данных предоставляет сведения о возможных проблемах с обработкой запросов и рекомендуемые решения. Она также может автоматически исправлять выявленные проблемы.

Автоматическая настройка, введенная в [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , уведомляет вас при обнаружении потенциальных проблем с производительностью и позволяет применять корректирующие действия или позволяет [!INCLUDE[ssde_md](../../includes/ssde_md.md)] автоматически устранять проблемы с производительностью. Автоматическая настройка [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет выявление и устранение проблем с производительностью, вызванных **регрессией выбора плана выполнения запроса**. Автоматическая настройка [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] также создает необходимые индексы и удаляет неиспользуемые индексы. Дополнительные сведения о планах выполнения запросов см. в разделе [планы выполнения](../../relational-databases/performance/execution-plans.md).

Выполняет [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] мониторинг запросов, выполняемых в базе данных, и автоматически повышает производительность рабочей нагрузки. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]Содержит встроенный механизм аналитики, который может автоматически настраивать и улучшать производительность запросов, динамически адаптируя базу данных к рабочей нагрузке. Доступны две функции автоматической настройки:

-    **Автоматическое исправление плана** определяет проблемные планы выполнения запросов, такие как [чувствительность к параметрам или проблемы с сканированием параметров](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) , и устраняет проблемы производительности, связанные с планом выполнения запроса, вызывая Последний известный хороший план до регрессии. **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]) и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

-    **Автоматическое управление индексами** определяет индексы, которые должны быть добавлены в базу данных, и индексы, которые следует удалить. **Область применения**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

## <a name="why-automatic-tuning"></a>Почему автоматическая настройка

Три основные задачи в классическом администрировании базы данных — мониторинг рабочей нагрузки, определение критических [!INCLUDE[tsql_md](../../includes/tsql-md.md)] запросов и определение индексов, которые следует добавить для повышения производительности, или индексы, которые не используются редко и могут быть удалены для повышения производительности. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Предоставляет подробные сведения о запросах и индексах, которые необходимо отслеживать. Однако постоянное наблюдение за базой данных является сложной и утомительной задачей, особенно при работе с множеством баз данных. Управление огромным числом баз данных может быть невозможно эффективно. Вместо того, чтобы выполнять мониторинг и настройку базы данных вручную, можно рассмотреть возможность делегирования некоторых действий по мониторингу и настройке с [!INCLUDE[ssde_md](../../includes/ssde_md.md)] помощью функции автоматической настройки.

### <a name="how-does-automatic-tuning-work"></a>Как работает автоматическая настройка

Автоматическая настройка — это непрерывный процесс мониторинга и анализа, который постоянно узнает о характеристиках рабочей нагрузки и выявляет потенциальные проблемы и улучшения.

![Процесс автоматической настройки](./media/tuning-process.png)

Этот процесс позволяет базе данных динамически адаптироваться к рабочей нагрузке путем поиска индексов и планов, которые могут повысить производительность рабочих нагрузок и индексов, влияющих на рабочие нагрузки. На основе этих результатов автоматическая настройка применяет действия по настройке, повышающие производительность рабочей нагрузки. Кроме того, автоматическая настройка постоянно отслеживает производительность базы данных после реализации любых изменений, чтобы обеспечить повышение производительности рабочей нагрузки. Все действия, которые не улучшают производительность, будут автоматически отменены. Этот процесс проверки является ключевой функцией, которая гарантирует, что любые изменения, внесенные при автоматической настройке, не снижают общую производительность рабочей нагрузки.

## <a name="automatic-plan-correction"></a>Автоматическое исправление плана

Автоматическое исправление плана — это функция автоматической настройки, которая определяет **регрессию выбора плана выполнения** и автоматически устраняет проблему, вызывая Последний известный хороший план. Дополнительные сведения о планах выполнения запросов и оптимизаторе запросов см. в разделе [руководств по архитектуре обработки запросов](../../relational-databases/query-processing-architecture-guide.md).

> [!IMPORTANT]
> Автоматическое исправление плана зависит от того, включено хранилище запросов в базе данных для отслеживания рабочей нагрузки. 

### <a name="what-is-execution-plan-choice-regression"></a>Что такое регрессия выбора плана выполнения?

[!INCLUDE[ssdenoversion_md](../../includes/ssdenoversion_md.md)]Для выполнения запросов в могут использоваться разные планы выполнения [!INCLUDE[tsql_md](../../includes/tsql-md.md)] . Планы запросов зависят от статистики, индексов и других факторов. Оптимальный план, который должен использоваться для выполнения [!INCLUDE[tsql_md](../../includes/tsql-md.md)] запроса, может меняться со временем в зависимости от изменений в этих факторах. В некоторых случаях новый план может быть не выше, чем предыдущий, и новый план может привести к снижению производительности, например по [чувствительности к параметрам или](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) к проблемам, связанным с сканированием параметров. 

 ![Регрессия выбора плана выполнения запроса](media/plan-choice-regression.png "Регрессия выбора плана выполнения запроса") 

При обнаружении регрессии выбора плана следует найти предыдущий хороший план и принудительно использовать его вместо текущего. Это можно сделать с помощью `sp_query_store_force_plan` процедуры. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]В [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] содержит сведения о регрессионных планах и рекомендованных корректирующих действиях. Кроме того, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] позволяет полностью автоматизировать этот процесс и позволить [!INCLUDE[ssde_md](../../includes/ssde_md.md)] устранить все проблемы, связанные с изменением плана.

> [!IMPORTANT]
> Автоматическое исправление плана должно использоваться в области обновления уровня совместимости базы данных после того, как будет записан базовый план, чтобы автоматически устранить риски обновления рабочей нагрузки. Дополнительные сведения об этом варианте использования см. в статье [обеспечение стабильной производительности во время обновления до более новой SQL Server](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). 

### <a name="automatic-plan-choice-correction"></a>Автоматическое исправление выбора плана

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]При обнаружении регрессии выбора плана может автоматически переключиться на последний известный хороший план.

![Исправление выбора плана выполнения запроса](media/force-last-good-plan.png "Исправление выбора плана выполнения запроса") 

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]Автоматически обнаруживает любую потенциальную регрессию выбора плана, включая план, который следует использовать вместо неверного плана. Когда [!INCLUDE[ssde_md](../../includes/ssde_md.md)] применяет последний известный хороший план до регрессии, он автоматически отслеживает производительность принудительного плана. Если принудительный план не выше, чем регрессивный план, новый план будет непринудительным, а [!INCLUDE[ssde_md](../../includes/ssde_md.md)] будет компилировать новый план. Если [!INCLUDE[ssde_md](../../includes/ssde_md.md)] проверяет, что принудительный план лучше, чем регрессионный, то принудительный план будет храниться. Он будет храниться до тех пор, пока не произойдет перекомпиляция (например, при следующем обновлении статистики или изменении схемы). Дополнительные сведения о форсировании планов и типах планов, которые можно принудительно применить, см. в разделе [ограничения на форсирование](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md#plan-forcing-limitations)планов.

> [!NOTE]
> Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] экземпляр перезапускается до проверки действия форсирования плана, этот план будет автоматически непринудительно выполняться. В противном случае Форсирование планов сохраняется при [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] перезапуске.

### <a name="enabling-automatic-plan-choice-correction"></a>Включение автоматического исправления выбранного плана

Можно включить автоматическую настройку для каждой базы данных и указать, что при обнаружении некоторой регрессии изменения плана необходимо принудительно применить последний хороший план. Автоматическая настройка включается с помощью следующей команды.

```sql   
ALTER DATABASE <yourDatabase>
SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON ); 
```

После включения этого параметра [!INCLUDE[ssde_md](../../includes/ssde_md.md)] будет автоматически принудительно использовать любые рекомендации, в которых предполагаемое увеличение ЦП превышает 10 секунд, или число ошибок в новом плане выше, чем количество ошибок в рекомендуемом плане, и убедиться, что принудительный план лучше, чем текущий.

### <a name="alternative---manual-plan-choice-correction"></a>Альтернатива — исправление выбранного плана вручную

Без автоматической настройки пользователи должны периодически отслеживать систему и искать запросы, которые были регрессионы. Если какой-либо план был регрессй, пользователь должен найти предыдущий хороший план и принудительно использовать его вместо текущего, используя `sp_query_store_force_plan` процедуру. Рекомендуется принудительно применить последний известный хороший план, так как старые планы могут быть недопустимы из-за изменения статистики или индекса. Пользователь, который вызывает последний известный хороший план, должен отслеживать производительность запроса, выполняемого с помощью принудительного плана, и проверять, что принудительный план работает должным образом. В зависимости от результатов мониторинга и анализа план должен быть принудительно запущен, или пользователь должен найти другой способ оптимизации запроса, например переписать его. Принудительные планы вручную не должны быть принудительно бесконечно, так как [!INCLUDE[ssde_md](../../includes/ssde_md.md)] должны иметь возможность применять оптимальные планы. Пользователь или администратор баз данных должен в конечном итоге отменить принудительное применение плана с помощью `sp_query_store_unforce_plan` процедуры и позволить [!INCLUDE[ssde_md](../../includes/ssde_md.md)] найти оптимальный план. 

> [!TIP]
> Кроме того, можно использовать запросы с представлением хранилища запросов **с принудительными планами** для поиска и принудительного применения планов.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предоставляет все необходимые представления и процедуры, необходимые для отслеживания производительности и устранения проблем в хранилище запросов.

В [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] среде можно найти регрессию выбора плана с помощью системных представлений хранилища запросов. Начиная с [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] , программа [!INCLUDE[ssde_md](../../includes/ssde_md.md)] обнаруживает и отображает возможные регрессии выбора планов и рекомендуемые действия, которые должны применяться в [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) динамического административного представления. Динамическое административное представление отображает сведения о проблеме, важность проблемы и подробные сведения, такие как идентифицированный запрос, идентификатор регрессионного плана, идентификатор плана, который использовался в качестве базового для сравнения, и [!INCLUDE[tsql_md](../../includes/tsql-md.md)] инструкцию, которая может быть выполнена для устранения проблемы.

| type | description; | DATETIME | score | подробности | ... |
| --- | --- | --- | --- | --- | --- |
| `FORCE_LAST_GOOD_PLAN` | Время ЦП изменено с 4 мс до 14 мс | 3/17/2017 | 83 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |
| `FORCE_LAST_GOOD_PLAN` | Время ЦП изменено с 37 МС на 84 МС | 16.03.2017 | 26 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |

Некоторые столбцы из этого представления описаны в следующем списке:
 - Тип рекомендуемого действия `FORCE_LAST_GOOD_PLAN` .
 - Описание, содержащее сведения о том [!INCLUDE[ssde_md](../../includes/ssde_md.md)] , что это изменение плана является потенциальной регрессией производительности.
 - Дата и время, когда обнаруживается потенциальная регрессия.
 - Оценка этой рекомендации.
 - Сведения о проблемах, например идентификатор обнаруженного плана, идентификатор регрессионного плана, идентификатор плана, который должен быть вынужден устранить проблему, [!INCLUDE[tsql_md](../../includes/tsql-md.md)] сценарий, который можно применить для устранения проблемы и т. д. Сведения хранятся в [формате JSON](../json/json-data-sql-server.md).

Используйте следующий запрос для получения сценария, который устраняет проблему, и дополнительные сведения о предполагаемом выигрыше:

```sql   
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressedPlanId int '$.regressedPlanId',
            recommendedPlanId int '$.recommendedPlanId',
            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,
            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float
          ) AS planForceDetails;
```

[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]     

| reason | score | скрипт | \_идентификатор запроса | Идентификатор текущего плана \_ | Рекомендуемый \_ идентификатор плана | предполагаемое \_ увеличение | \_подвержена ошибкам
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Время ЦП изменено с 3 по МС на 46 МС | 36 | Пакет EXEC \_ SP \_ хранилище запросов \_ Force \_ план 12, 17; | 12 | 28 | 17 | 11,59 | 0

Столбец `estimated_gain` представляет предполагаемое количество секунд, которое будет сохранено, если рекомендованный план будет использоваться для выполнения запроса, а не для текущего плана. Рекомендуемый план следует принудительно применять вместо текущего плана, если рост превышает 10 секунд. Если в текущем плане имеется больше ошибок (например, времени ожидания или прерываний выполнения), чем в рекомендуемом плане, столбцу будет `error_prone` присвоено значение `YES` . План, подверженный ошибкам, является еще одной причиной, по которой Рекомендуемый план следует принудительно применять вместо текущего.

Несмотря на то, что [!INCLUDE[ssde_md](../../includes/ssde_md.md)] предоставляет все сведения, необходимые для обнаружения регрессий по выбору планов, непрерывный мониторинг и устранение проблем с производительностью могут стать утомительным процессом. Автоматическая настройка значительно упрощает этот процесс.

> [!NOTE]
> Данные в `sys.dm_db_tuning_recommendations` динамическом административном наборе данных не сохраняются после перезапуска ядра СУБД. Используйте `sqlserver_start_time` столбец в [sys.dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) , чтобы найти время последнего запуска ядра СУБД.   

## <a name="automatic-index-management"></a>Автоматическое управление индексами

В [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] Управление индексами выполняется легко, так как [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] вы узнаете о рабочей нагрузке и гарантируете, что данные всегда индексируются оптимально. Правильное проектирование индекса имеет решающее значение для обеспечения оптимальной производительности при рабочей нагрузке. Автоматическое управление индексами поможет оптимизировать их. Автоматическое управление индексами может как устранять проблемы с производительностью в неправильно индексированных базах данных, так и поддерживать и оптимизировать индексы в существующей схеме базы данных. Автоматическая настройка в [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] выполняет следующие действия.

 - Определяет индексы, которые могут повысить производительность [!INCLUDE[tsql_md](../../includes/tsql-md.md)] запросов, считывающих данные из таблиц.
 - Определяет избыточные индексы или индексы, которые не использовались в течение более длительного периода времени, которые можно удалить. Удаление ненужных индексов повышает производительность запросов, которые обновляют данные в таблицах.

### <a name="why-do-you-need-index-management"></a>Для чего необходимо управление индексами

Индексы ускоряют некоторые запросы, считывающие данные из таблиц, однако они могут замедлить запросы, которые обновляют данные. Необходимо тщательно проанализировать необходимость создания индекса и какие столбцы нужно включать в него. Некоторые индексы могут оказаться ненужными через некоторое время. Поэтому необходимо периодически указывать и удалять эти индексы, которые не предоставляют никаких преимуществ. Если вы пропускаете неиспользуемые индексы, производительность запросов, которые обновляют данные, будет уменьшена без каких бы то ни было преимуществ для запросов, считывающих данные. Неиспользуемые индексы также влияют на общую производительность системы, так как дополнительные обновления требуют ненужного ведения журнала.

Поиск оптимального набора индексов, повышающего производительность запросов, которые считывают данные из таблиц и оказывают минимальное влияние на обновления, может потребовать непрерывного и сложного анализа.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] использует встроенные аналитические средства и Расширенные правила, которые анализируют запросы, выдают индексы, оптимальные для текущих рабочих нагрузок, и определяет индексы, которые может потребоваться удалить. База данных SQL Azure обеспечивает минимальный необходимый набор индексов, которые оптимизируют запросы, считывающие данные, с минимальным влиянием на другие запросы.

### <a name="automatic-index-management"></a>Автоматическое управление индексами

Помимо обнаружения, [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] может автоматически применять определенные рекомендации. Если вы обнаружите, что встроенные правила повышают производительность базы данных, вы можете [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] автоматически управлять индексами.

Сведения о включении автоматической настройки в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и разрешении функции автоматической настройки для полного управления рабочей нагрузкой см. [в статье Включение автоматической настройки в базе данных SQL Azure с помощью портал Azure](/azure/sql-database/sql-database-automatic-tuning-enable).

Когда объект [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] применяет рекомендацию CREATE INDEX или DROP INDEX, он автоматически отслеживает производительность запросов, затрагиваемых индексом. Новый индекс будет храниться только в том случае, если производительность затронутые запросы улучшены. Удаленный индекс будет автоматически создан повторно, если имеются некоторые запросы, которые выполняются медленнее из-за отсутствия индекса.

### <a name="automatic-index-management-considerations"></a>Рекомендации по автоматическому управлению индексами

Действия, необходимые для создания необходимых индексов в [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] , могут потреблять ресурсы и временно влиять на производительность рабочей нагрузки. Чтобы уменьшить влияние создания индекса на производительность рабочей нагрузки, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  находит подходящее временное окно для любой операции управления индексами. Действие настройки откладывается, если базе данных требуются ресурсы для выполнения рабочей нагрузки, и перезапускается, когда в базе данных достаточно неиспользуемых ресурсов, которые можно использовать для задачи обслуживания. Одна из важных функций автоматического управления индексами — это проверка действий. При [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  создании или удалении индекса процесс мониторинга анализирует производительность рабочей нагрузки, чтобы убедиться, что действие улучшило общую производительность. Если это не было существенно улучшено, действие будет немедленно отменено. Таким образом, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  Автоматическое выполнение действий по настройке не негативно сказывается на производительности рабочей нагрузки. Индексы, созданные автоматической настройкой, прозрачны для операции обслуживания в базовой схеме. Изменения схемы, например удаление или переименование столбцов, не блокируются при наличии автоматически созданных индексов. Индексы, созданные автоматически, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  немедленно удаляются при удалении связанной таблицы или столбцов.

### <a name="alternative---manual-index-management"></a>Альтернативное управление индексами вручную

Без автоматического управления индексами пользователю или администратору базы данных придется вручную запрашивать [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) просмотра или использовать отчет панели мониторинга производительности в [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] для поиска индексов, которые могут повысить производительность, создавать индексы с использованием сведений, представленных в этом представлении, и вручную отслеживать производительность запроса. Чтобы найти индексы, которые следует удалить, пользователям следует отслеживать статистику использования операций индексов для поиска редко используемых индексов.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] упрощает этот процесс. [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] анализирует рабочую нагрузку, определяет запросы, которые могут выполняться быстрее с новым индексом, и определяет неиспользуемые или дублирующиеся индексы. Дополнительные сведения об идентификации индексов, которые необходимо изменить, см. в статье [Использование помощника по базам данных SQL на портале Azure](/azure/sql-database/sql-database-advisor-portal).

## <a name="see-also"></a>См. также раздел  
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;&#41;Transact-SQL ](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_automatic_tuning_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)  
 [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)     
 [sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)           
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)    
 [Функции JSON](../json/json-data-sql-server.md)    
 [Планы выполнения](../../relational-databases/performance/execution-plans.md)    
 [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Средства контроля и настройки производительности](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
 [Помощник по настройке запросов](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)    
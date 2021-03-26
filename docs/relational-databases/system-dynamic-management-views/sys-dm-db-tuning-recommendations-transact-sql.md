---
title: sys.dm_db_tuning_recommendations (Transact-SQL)
description: Узнайте, как найти потенциальные проблемы с производительностью и Рекомендуемые исправления в SQL Server и базе данных SQL Azure.
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_tuning_recommendations
- dm_db_tuning_recommendations
- sys.dm_db_tuning_recommendations_TSQL
- dm_db_tuning_recommendations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database tuning recommendations feature [SQL Server], sys.dm_db_tuning_recommendations dynamic management view
- sys.dm_db_tuning_recommendations dynamic management view
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 44c4b9bbc975074d1852f0e7a0f074e3c3acc979
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551672"
---
# <a name="sysdm_db_tuning_recommendations-transact-sql"></a>\_рекомендации по настройке sys.DM DB \_ \_ (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Возвращает подробные сведения о рекомендациях по настройке.  
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.

| **Имя столбца** | **Data type** | **Описание** |
| --- | --- | --- |
| **name** | **nvarchar(4000)** | Уникальное имя рекомендации. |
| **type** | **nvarchar(4000)** | Имя параметра автоматической настройки, который создал рекомендацию, например `FORCE_LAST_GOOD_PLAN` |
| **reason** | **nvarchar(4000)** | Причина, по которой была предоставлена эта рекомендация. |
| **действительно \_ с** | **datetime2** | При первом создании этой рекомендации. |
| **Последнее \_ обновление** | **datetime2** | Время последнего создания этой рекомендации. |
| **state** | **nvarchar(4000)** | Документ JSON, описывающий состояние рекомендации. Доступны следующие поля:<br />-   `currentValue` — Текущее состояние рекомендации.<br />-   `reason` — Константа, описывающая причину, по которой рекомендация находится в текущем состоянии.|
| **\_исполняемое \_ действие** | **bit** | 1 = рекомендацию можно выполнить в базе данных с помощью [!INCLUDE[tsql_md](../../includes/tsql-md.md)] скрипта.<br />0 = рекомендация не может быть выполнена для базы данных (например, только сведения или отмененная рекомендация). |
| **является действием, которое может быть \_ отменено \_** | **bit** | 1 = рекомендация может автоматически отслеживаться и возвращаться ядром СУБД.<br />0 = рекомендация не может быть автоматически отслеживаться и отменена. Большинство &quot; исполняемых &quot; действий будут &quot; восстановлены &quot; . |
| **\_ \_ время начала действия \_ при выполнении** | **datetime2** | Дата применения рекомендации. |
| **\_Длительность выполнения действия \_** | **time** | Длительность действия выполнения. |
| **выполнить \_ действие \_ , инициированное \_** | **nvarchar(4000)** | `User` = Пользователь вручную принудительно запланировать рекомендации. <br /> `System` = Система автоматически применила рекомендацию. |
| **время выполнения, \_ \_ инициированное действием \_** | **datetime2** | Дата применения рекомендации. |
| **\_ \_ время начала отката действия \_** | **datetime2** | Дата отката рекомендации. |
| **отменить \_ \_ Длительность действия** | **time** | Длительность действия отката. |
| **отменить \_ действие \_ , инициированное \_** | **nvarchar(4000)** | `User` = Пользователь вручную не рекомендовал Рекомендуемый план. <br /> `System` = Система автоматически отменяет рекомендацию. |
| **\_время отмены \_ инициации действия \_** | **datetime2** | Дата отката рекомендации. |
| **понять** | **int** | Предполагаемое значение и влияние на эту рекомендацию по шкале 0-100 (чем больше, тем выше) |
| **Дополнительно** | **nvarchar(max)** | Документ JSON, содержащий дополнительные сведения о рекомендации. Доступны следующие поля:<br /><br />`planForceDetails`<br />-    `queryId` — \_ идентификатор запроса регрессионного запроса.<br />-    `regressedPlanId` — plan_id регрессионного плана.<br />-   `regressedPlanExecutionCount` — Число выполнений запроса с регрессионным планом до обнаружения регрессии.<br />-    `regressedPlanAbortedCount` — Число обнаруженных ошибок во время выполнения регрессивного плана.<br />-    `regressedPlanCpuTimeAverage` — Среднее время ЦП (в микросекундах), затраченное на регрессионный запрос до обнаружения регрессии.<br />-    `regressedPlanCpuTimeStddev` — Стандартное отклонение времени ЦП, потребляемого регрессионным запросом до обнаружения регрессии.<br />-    `recommendedPlanId` — plan_id плана, который должен быть принудительно вынужден.<br />-   `recommendedPlanExecutionCount`— Число выполнений запроса с планом, который должен быть принудительно завершен до обнаружения регрессии.<br />-    `recommendedPlanAbortedCount` — Число обнаруженных ошибок во время выполнения плана, который должен быть принудительно выполнен.<br />-    `recommendedPlanCpuTimeAverage` Среднее время ЦП (в микросекундах), затраченное на выполнение запроса с планом, который должен быть принудительно завершен (вычислено до обнаружения регрессии).<br />-    `recommendedPlanCpuTimeStddev` Стандартное отклонение времени ЦП, потребляемого регрессионным запросом до обнаружения регрессии.<br /><br />`implementationDetails`<br />-  `method` — Метод, который должен использоваться для исправления регрессии. Значение всегда равно `TSql` .<br />-    `script` - [!INCLUDE[tsql_md](../../includes/tsql-md.md)] Скрипт, который должен быть выполнен для принудительного применения рекомендуемого плана. |
  
## <a name="remarks"></a>Примечания  
 Информация, возвращаемая, `sys.dm_db_tuning_recommendations` обновляется, когда ядро СУБД определяет потенциальную регрессию производительности запросов и не сохраняется. Рекомендации сохраняются только до перезапуска ядра СУБД. Используйте `sqlserver_start_time` столбец в [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) , чтобы найти время последнего запуска ядра СУБД.  Администраторы баз данных должны периодически создавать резервные копии рекомендаций по настройке, если они хотят обеспечить их работу после повторного использования сервера.   

 `currentValue`Поле в `state` столбце может иметь следующие значения:
 
 | Состояние | Описание |
 |--------|-------------|
 | `Active` | Рекомендация активна и еще не применена. Пользователь может принять сценарий рекомендаций и выполнить его вручную. |
 | `Verifying` | Рекомендация применяется, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] а внутренний процесс проверки сравнивает производительность принудительного плана с регрессивным планом. |
 | `Success` | Рекомендация успешно применена. |
 | `Reverted` | Рекомендация отменена, так как нет значительного выигрыша в производительности. |
 | `Expired` | Срок действия рекомендации истек, и он больше не может быть применен. |

Документ JSON в `state` столбце содержит причину, которая описывает, почему является рекомендацией в текущем состоянии. Значения в поле Причина могут быть: 

| Причина | Описание |
|--------|-------------|
| `SchemaChanged` | Срок действия рекомендации истек из-за изменения схемы ссылочной таблицы. Новая рекомендация будет создана, если в новой схеме обнаружена новая регрессия плана запроса. |
| `StatisticsChanged`| Срок действия рекомендации истек из-за изменения статистики в связанной таблице. Новая рекомендация будет создана, если будет обнаружена новая регрессия плана запроса на основе новой статистики. |
| `ForcingFailed` | Рекомендуемый план не может быть принудительно применен к запросу. Найдите `last_force_failure_reason` в представлении [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md) , чтобы узнать причину сбоя. |
| `AutomaticTuningOptionDisabled` | `FORCE_LAST_GOOD_PLAN` параметр отключен пользователем в процессе проверки. Включите `FORCE_LAST_GOOD_PLAN` параметр, используя инструкцию [ALTER database SET AUTOMATIC_TUNING &#40;инструкции TRANSACT-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) или принудительный запуск плана вручную с помощью скрипта в `details` столбце. |
| `UnsupportedStatementType` | Невозможно принудительно применить план к запросу. Примерами неподдерживаемых запросов являются курсоры и `INSERT BULK` операторы. |
| `LastGoodPlanForced` | Рекомендация успешно применена. |
| `AutomaticTuningOptionNotEnabled`| [!INCLUDE[ssde_md](../../includes/ssde_md.md)] обнаружена потенциальная регрессия производительности, но `FORCE_LAST_GOOD_PLAN` параметр не включен — см. раздел [ALTER database SET AUTOMATIC_TUNING &#40;TRANSACT-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Примените рекомендацию вручную или включите `FORCE_LAST_GOOD_PLAN` параметр. |
| `VerificationAborted`| Процесс проверки прерван из-за перезапуска или очистки хранилища запросов. |
| `VerificationForcedQueryRecompile`| Запрос перекомпилируется, так как нет существенного улучшения производительности. |
| `PlanForcedByUser`| Пользователь вручную принудительно затребовал план с помощью [sp_query_store_force_plan &#40;процедуры&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md) . Ядро СУБД не будет применять рекомендацию, если пользователь явно решил принудительно применить некоторый план. |
| `PlanUnforcedByUser` | Пользователь вручную не принудительно вынудить план с помощью [sp_query_store_unforce_plan &#40;процедуры&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md) . Так как пользователь явно отменяет Рекомендуемый план, ядро СУБД продолжает использовать текущий план и создает новую рекомендацию, если в будущем происходит некоторое снижение плана. |
| `UserForcedDifferentPlan` | Пользователь вручную принудительно затребовал другой план, используя [sp_query_store_force_plan &#40;процедуры&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md) . Ядро СУБД не будет применять рекомендацию, если пользователь явно решил принудительно применить некоторый план. |
| `TempTableChanged` | Изменяется временная таблица, использованная в плане. |

 Статистические `details` данные в столбце не отображают статистику плана времени выполнения (например, текущее время ЦП). Сведения о рекомендациях выполняются во время обнаружения регрессии и описываются причины [!INCLUDE[ssde_md](../../includes/ssde_md.md)] снижения производительности. Используйте `regressedPlanId` и `recommendedPlanId` для запроса [представлений каталога хранилища запросов](../../relational-databases/performance/how-query-store-collects-data.md) , чтобы найти точную статистику плана времени выполнения.

## <a name="examples-of-using-tuning-recommendations-information"></a>Примеры использования сведений о рекомендациях по настройке  

### <a name="example-1"></a>Пример 1
Следующий пример получает созданный [!INCLUDE[tsql](../../includes/tsql-md.md)] скрипт, который вызывает хороший план для любого заданного запроса:  
 
```sql
SELECT name, reason, score,
    JSON_VALUE(details, '$.implementationDetails.script') AS script,
    details.* 
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON(details, '$.planForceDetails')
    WITH (    [query_id] int '$.queryId',
            regressed_plan_id int '$.regressedPlanId',
            last_good_plan_id int '$.recommendedPlanId') AS details
WHERE JSON_VALUE(state, '$.currentValue') = 'Active';
```
### <a name="example-2"></a>Пример 2
Следующий пример получает созданный [!INCLUDE[tsql](../../includes/tsql-md.md)] скрипт, который принудительно вызывает хороший план для любого заданного запроса и дополнительные сведения о предполагаемом выигрыше:

```sql
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  *(regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
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

### <a name="example-3"></a>Пример 3
Следующий пример получает созданный [!INCLUDE[tsql](../../includes/tsql-md.md)] скрипт, который принудительно вызывает хороший план для любого заданного запроса и дополнительные сведения, включающие текст запроса и планы запросов, хранимые в хранилище запросов:

```sql
WITH cte_db_tuning_recommendations
AS (SELECT reason,
        score,
        query_id,
        regressedPlanId,
        recommendedPlanId,
        current_state = JSON_VALUE(state, '$.currentValue'),
        current_state_reason = JSON_VALUE(state, '$.reason'),
        script = JSON_VALUE(details, '$.implementationDetails.script'),
        estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
        error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
    FROM sys.dm_db_tuning_recommendations
    CROSS APPLY OPENJSON(Details, '$.planForceDetails')
    WITH ([query_id] int '$.queryId',
        regressedPlanId int '$.regressedPlanId',
        recommendedPlanId int '$.recommendedPlanId',
        regressedPlanErrorCount int,    
        recommendedPlanErrorCount int,
        regressedPlanExecutionCount int,
        regressedPlanCpuTimeAverage float,
        recommendedPlanExecutionCount int,
        recommendedPlanCpuTimeAverage float
        )
    )
SELECT qsq.query_id,
    qsqt.query_sql_text,
    dtr.*,
    CAST(rp.query_plan AS XML) AS RegressedPlan,
    CAST(sp.query_plan AS XML) AS SuggestedPlan
FROM cte_db_tuning_recommendations AS dtr
INNER JOIN sys.query_store_plan AS rp ON rp.query_id = dtr.query_id
    AND rp.plan_id = dtr.regressedPlanId
INNER JOIN sys.query_store_plan AS sp ON sp.query_id = dtr.query_id
    AND sp.plan_id = dtr.recommendedPlanId
INNER JOIN sys.query_store_query AS qsq ON qsq.query_id = rp.query_id
INNER JOIN sys.query_store_query_text AS qsqt ON qsqt.query_text_id = qsq.query_text_id;
```

Дополнительные сведения о функциях JSON, которые можно использовать для запроса значений в представлении рекомендаций, см. в разделе [Поддержка JSON](../json/json-data-sql-server.md) в [!INCLUDE[ssde_md](../../includes/ssde_md.md)] .
  
## <a name="permissions"></a>Разрешения  

Требуется `VIEW SERVER STATE` разрешение в [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] .   
Требуется `VIEW DATABASE STATE` разрешение для базы данных в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .   

## <a name="see-also"></a>См. также раздел  
 [Автоматическая настройка](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [sys.database_automatic_tuning_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)   
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [Поддержка JSON](../json/json-data-sql-server.md) [sys.dm_os_sys_info &#40;Transact-SQL&#41;](sys-dm-os-sys-info-transact-sql.md)    
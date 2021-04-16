---
description: Сценарии использования темпоральных таблиц
title: Сценарии использования темпоральных таблиц | Документация Майкрософт
ms.custom: ''
ms.date: 05/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 4b8fa2dd-1790-4289-8362-f11e6d63bb09
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c869a7d0d767bb2985f9078f9628083d221ac24
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2021
ms.locfileid: "107561081"
---
# <a name="temporal-table-usage-scenarios"></a>Сценарии использования темпоральных таблиц

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Темпоральные таблицы обычно используются в сценариях, в которых требуется отслеживание журнала изменений данных. Мы рекомендуем использовать темпоральные таблицы в следующих случаях, поскольку они дают существенный выигрыш в производительности:

## <a name="data-audit"></a>Аудит данных

Используйте темпоральное системное управление версиями в таблицах, хранящих важные сведения, для которых необходимо отслеживать, что и когда было изменено, а также выполнять экспертизу данных на какой-либо момент времени.

Темпоральные таблицы с системным управлением версиями позволяют планировать сценарии аудита данных на ранних стадиях цикла разработки или добавлять аудит данных в существующие приложения или решения, когда он необходим.

На следующей схеме показан сценарий таблицы Employee с примером данных, включая текущие (помеченные синим цветом) и предыдущие версии строк (помеченные серым цветом).
В правой части схемы показаны версии строк по оси времени, а также какие строки выбираются в разных типах запросов к темпоральной таблице с предложением SYSTEM_TIME или без него.

![Схема, показывающая первый сценарий использования темпоральных таблиц.](../../relational-databases/tables/media/temporalusagescenario1.png "TemporalUsageScenario1")

### <a name="enabling-system-versioning-on-a-new-table-for-data-audit"></a>Включение системного управления версиями в новой таблице для аудита данных

Если вы определили информацию, для которой необходим аудит данных, создайте таблицы базы данных как темпоральные с системным управлением версиями. В следующем простом примере показан сценарий с информацией в таблице Employee в гипотетической базе данных HR.

```sql
CREATE TABLE Employee
(
  [EmployeeID] int NOT NULL PRIMARY KEY CLUSTERED
  , [Name] nvarchar(100) NOT NULL
  , [Position] varchar(100) NOT NULL
  , [Department] varchar(100) NOT NULL
  , [Address] nvarchar(1024) NOT NULL
  , [AnnualSalary] decimal (10,2) NOT NULL
  , [ValidFrom] datetime2 (2) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (2) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.EmployeeHistory));
```

Разные варианты создания темпоральной таблицы с системным управлением версиями описываются в разделе [Создание темпоральной таблицы с системным управлением версиями](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md).

### <a name="enabling-system-versioning-on-an-existing-table-for-data-audit"></a>Включение системного управления версиями в существующей таблице для аудита данных

Если требуется выполнить аудит данных в существующих базах данных, используйте инструкцию ALTER TABLE, чтобы добавить системное управление версиями в таблицы, не являющиеся темпоральными. Во избежание критических изменений в приложении добавляйте столбцы периода как скрытые (HIDDEN), как объясняется в разделе [Замена не являющихся темпоральными таблиц темпоральными таблицами с системным управлением версиями](./creating-a-system-versioned-temporal-table.md). В следующем примере показано включение системного управления версиями в существующей таблице Employee в гипотетической базе данных HR.

```sql
/*
Turn ON system versioning in Employee table in two steps
(1) add new period columns (HIDDEN)
(2) create default history table
*/
ALTER TABLE Employee
ADD
    ValidFrom datetime2 (2) GENERATED ALWAYS AS ROW START HIDDEN
        constraint DF_ValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())  
    , ValidTo datetime2 (2) GENERATED ALWAYS AS ROW END HIDDEN
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);
  
ALTER TABLE Employee
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Employee_History));
```

> [!IMPORTANT]
> Точность типа данных datetime2 одинакова в исходной таблице и в таблице журнала с системным управлением версиями.

После выполнения приведенного выше скрипта все изменения данных будут прозрачно собираться в таблице журнала. В типичном сценарии аудита данных запрашиваются все изменения данных, примененные к отдельной строке в течение интересующего периода времени. Таблица журнала по умолчанию создается со сбалансированным деревом кластеризованного хранилища строк для эффективного решения этого варианта использования.

### <a name="performing-data-analysis"></a>Выполнение анализа данных

После включения системного управления версиями с помощью любого из перечисленных выше методов достаточно одного запроса, чтобы выполнить аудит данных. Следующий запрос ищет версии строк для записи о сотруднике с EmployeeID = 1000, которые были активны по крайней мере часть времени между 1 января 2014 г. и 1 января 2015 г. (включая верхнюю границу периода).

```sql
SELECT * FROM Employee
    FOR SYSTEM_TIME
      BETWEEN '2014-01-01 00:00:00.0000000' AND '2015-01-01 00:00:00.0000000'
        WHERE EmployeeID = 1000 ORDER BY ValidFrom;
```

Чтобы проанализировать весь журнал изменения данных для этого конкретного сотрудника, замените FOR SYSTEM_TIME BETWEEN...AND на SYSTEM_TIME ALL:

```sql
SELECT * FROM Employee
    FOR SYSTEM_TIME ALL WHERE
        EmployeeID = 1000 ORDER BY ValidFrom;
```

Чтобы найти версии строк, которые были активны только в течение некоторого периода (но не вне его), используйте предложение CONTAINED IN. Этот запрос очень эффективен, поскольку запрашивает только таблицу журнала:

```sql
SELECT * FROM Employee FOR SYSTEM_TIME
    CONTAINED IN ('2014-01-01 00:00:00.0000000', '2015-01-01 00:00:00.0000000')
        WHERE EmployeeID = 1000 ORDER BY ValidFrom;
```

Наконец, в некоторых сценариях аудита может потребоваться узнать, как выглядела вся таблица в какой-либо момент времени в прошлом:

```sql
SELECT * FROM Employee FOR SYSTEM_TIME AS OF '2014-01-01 00:00:00.0000000' ;
```

Темпоральные таблицы с системным управлением версиями хранят значения для столбцов периода в часовом поясе UTC, хотя всегда удобнее работать с местным часовым поясом и для фильтрации данных, и для отображения результатов. В следующих примерах кода показано, как применять условие фильтрации, которое изначально указывается в местном часовом поясе, а затем преобразуется в UTC с помощью предложения AT TIME ZONE, появившегося в SQL Server 2016:

```sql
/*Add offset of the local time zone to current time*/
DECLARE @asOf DATETIMEOFFSET = GETDATE() AT TIME ZONE 'Pacific Standard Time'
/*Convert AS OF filter to UTC*/
SET @asOf = DATEADD (MONTH, -9, @asOf) AT TIME ZONE 'UTC';

SELECT
    EmployeeID
    , Name
    , Position
    , Department
    , [Address]
    , [AnnualSalary]
    , ValidFrom AT TIME ZONE 'Pacific Standard Time' AS ValidFromPT
    , ValidTo AT TIME ZONE 'Pacific Standard Time' AS ValidToPT
FROM Employee
    FOR SYSTEM_TIME AS OF @asOf where EmployeeId = 1000
```

Предложение AT TIME ZONE удобно использовать во всех сценариях, где применяются таблицы с системным управлением версиями.

> [!TIP]
> Условия фильтрации, указанные в темпоральных предложениях с FOR SYSTEM_TIME, поддерживают SARG (т. е. SQL Server может использовать соответствующий базовый кластеризованный индекс для выполнения поиска вместо операции сканирования.
> При выполнении прямого запроса к таблице журнала важно, чтобы используемое условие фильтра поддерживало SARG. Для этого указывайте фильтры в формате \<period column> {< | > | =, ...} date_condition AT TIME ZONE 'UTC'.
> Если применить AT TIME ZONE к столбцам периода, то SQL Server будет выполнять сканирование таблицы или индекса, что может обходиться очень дорого. Избегайте подобных условий в запросах: \<period column> AT TIME ZONE '\<your time zone>' > {< | > | =, ...} date_condition.

См. также [Запрос данных в темпоральной таблице с системным управлением версиями](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md).

## <a name="point-in-time-analysis-time-travel"></a>Анализ на определенный момент времени (переход во времени)

В отличие от аудита данных, где обычно внимание сосредоточено на изменениях, произошедших с отдельными записями, в сценариях перехода во времени пользователи хотят видеть, как меняется со временем весь набор данных. Иногда переход во времени включает несколько связанных темпоральных таблиц, изменяющихся в независимом темпе, для которых требуется анализировать следующее:

- тренды для важных индикаторов в исторических и текущих данных;
- точный моментальный снимок всех данных на какой-либо момент времени в прошлом (вчера, месяц назад и т. д.);
- различия между двумя интересующими моментами времени (например, между данными месяц назад и данными три месяца назад).

Существует много реальных сценариев, в которых требуется анализ с переходом во времени. Чтобы проиллюстрировать этот сценарий использования, давайте взглянем на OLTP с автоматически создаваемым журналом.

### <a name="oltp-with-auto-generated-data-history"></a>OLTP с автоматически создаваемым журналом данных

В системах транзакционной обработки анализ того, как важные метрики изменяются с течением времени, не является чем-то необычным. В идеальном случае анализ журнала не должен ухудшать производительность приложения OLTP, где доступ к актуальному состоянию данных должен осуществляться с минимальной задержкой и блокировкой данных. Чтобы пользователи могли прозрачно хранить полный журнал изменений для дальнейшего анализа отдельно от текущих данных, с минимальным влиянием на основную рабочую нагрузку OLTP, были разработаны темпоральные таблицы с системным управлением версиями.

При высоких рабочих нагрузках транзакционной обработки рекомендуется использовать [темпоральные таблицы с системным управлением версиями и таблицы, оптимизированные для операций в памяти](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md), которые позволяют хранить текущие данные в памяти, а полный журнал изменений — на диске наиболее экономичным способом.

Для таблицы журнала рекомендуется использовать кластеризованный индекс columnstore по следующим причинам.

- Типичный анализ трендов использует преимущества, предоставляемые кластеризованным индексом columnstore.
- Задача очистки данных с оптимизированными для памяти таблицами при большой рабочей нагрузке OLTP выполняется лучше, когда таблица журнала имеет кластеризованный индекс columnstore.
- Кластеризованный индекс columnstore обеспечивает отличное сжатие, особенно в сценариях, где не все столбцы изменяются в одно и то же время.

Использование темпоральных таблиц с OLTP в памяти сокращает необходимость сохранять весь набор данных в памяти и позволяет легко различать "горячие" и "холодные" данные.

В качестве примеров реальных ситуаций, попадающих в эту категорию, можно среди прочего указать управление запасами и валютные операции.

На следующей схеме показана упрощенная модель данных, используемая для управления запасами.

![Схема упрощенной модели данных, используемой для управления запасами.](../../relational-databases/tables/media/temporalusageinmemory.png "TemporalUsageInMemory")

В следующем примере кода создается таблица ProductInventory как темпоральная таблица с системным управлением версиями в памяти с кластеризованным индексом columnstore в таблице журнала (который фактически заменяет индекс хранилища строк, создаваемый по умолчанию):

> [!NOTE]
> Убедитесь, что база данных позволяет создавать таблицы, оптимизированные для памяти. См. раздел [Создание таблицы с оптимизацией памяти и хранимой процедуры, скомпилированной в собственном коде](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).

```sql
USE TemporalProductInventory
GO

BEGIN
    --If table is system-versioned, SYSTEM_VERSIONING must be set to OFF first
    IF ((SELECT temporal_type FROM SYS.TABLES WHERE object_id = OBJECT_ID('dbo.ProductInventory', 'U')) = 2)
    BEGIN
        ALTER TABLE [dbo].[ProductInventory] SET (SYSTEM_VERSIONING = OFF)
    END
    DROP TABLE IF EXISTS [dbo].[ProductInventory]
       DROP TABLE IF EXISTS [dbo].[ProductInventoryHistory]
END
GO

CREATE TABLE [dbo].[ProductInventory]
(
    ProductId int NOT NULL,
    LocationID INT NOT NULL,
    Quantity int NOT NULL CHECK (Quantity >=0),
  
    SysStartTime datetime2 GENERATED ALWAYS AS ROW START NOT NULL ,
    SysEndTime datetime2 GENERATED ALWAYS AS ROW END NOT NULL ,
    PERIOD FOR SYSTEM_TIME(SysStartTime,SysEndTime),

    --Primary key definition
    CONSTRAINT PK_ProductInventory PRIMARY KEY NONCLUSTERED (ProductId, LocationId)
)
WITH
(
    MEMORY_OPTIMIZED=ON,
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = [dbo].[ProductInventoryHistory],
        DATA_CONSISTENCY_CHECK = ON
    )
)

CREATE CLUSTERED COLUMNSTORE INDEX IX_ProductInventoryHistory ON [ProductInventoryHistory]
WITH (DROP_EXISTING = ON);
```

Для модели выше процедура для обслуживания запасов может выглядеть так:

```sql
CREATE PROCEDURE [dbo].[spUpdateInventory]
@productId int,
@locationId int,
@quantityIncrement int

WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT, LANGUAGE=N'English')
    UPDATE dbo.ProductInventory
        SET Quantity = Quantity + @quantityIncrement
            WHERE ProductId = @productId AND LocationId = @locationId

/*If zero rows were updated than this is insert of the new product for a given location*/
    IF @@rowcount = 0
        BEGIN
            IF @quantityIncrement < 0
                SET @quantityIncrement = 0
            INSERT INTO [dbo].[ProductInventory]
                (
                    [ProductId]
                    ,[LocationID]
                    ,[Quantity]
                )
                VALUES
                   (
                        @productId
                       ,@locationId
                       ,@quantityIncrement
                   )
        END
END;
```

Хранимая процедура SpUpdateInventory либо вставляет новый продукт на склад, либо обновляет количество продуктов для определенного расположения. Бизнес-логика очень проста и заключается в поддержании постоянной точности актуального состояния путем увеличения или уменьшения значения поля Quantity через обновление таблицы, при этом таблицы с системным управлением версиями прозрачно добавляют измерение журнала к данным, как показано на следующей схеме.

![Схема, демонстрирующая использование использования темпоральных таблиц с текущим использованием в памяти и хронологическое использование в кластеризованном индексе columnstore.](../../relational-databases/tables/media/temporalusageinmemory2b.png "TemporalUsageInMemory2b")

Теперь запрос актуального состояния может выполняться эффективно из модуля, скомпилированного в собственном коде:

```sql
CREATE PROCEDURE [dbo].[spQueryInventoryLatestState]
WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT, LANGUAGE=N'English')
    SELECT ProductId, LocationID, Quantity, SysStartTime
        FROM dbo.ProductInventory
    ORDER BY ProductId, LocationId
END;
GO
EXEC [dbo].[spQueryInventoryLatestState];
```

Анализ изменений данных с течением времени становится очень простым с использованием предложения FOR SYSTEM_TIME ALL, как показано в следующем примере.

```sql
DROP VIEW IF EXISTS vw_GetProductInventoryHistory;
GO
CREATE VIEW vw_GetProductInventoryHistory
AS
    SELECT ProductId, LocationId, Quantity, SysStartTime, SysEndTime
    FROM [dbo].[ProductInventory]
        FOR SYSTEM_TIME ALL;
GO
SELECT * FROM vw_GetProductInventoryHistory
    WHERE ProductId = 2;
```

На схеме ниже показан журнал данных для одного продукта, который можно легко отобразить, импортировав представление выше в Power Query, Power BI или аналогичное средство бизнес-аналитики:

![Схема журнала данных для одного продукта.](../../relational-databases/tables/media/producthistoryovertime.png "ProductHistoryOverTime")

Темпоральные таблицы могут использоваться в этом сценарии для выполнения других типов анализа с переходом во времени, например для восстановления состояния запасов в любой момент времени в прошлом или для сравнения моментальных снимков, относящихся к разным моментам времени.

Кроме того, в этом сценарии использования можно расширить таблицы Product и Location до темпоральных таблиц, что даст возможность последующего анализа журнала изменений UnitPrice и NumberOfEmployee.

```sql
ALTER TABLE Product
ADD
    SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN
        constraint DF_ValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())
    , SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);

ALTER TABLE Product
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProductHistory));

ALTER TABLE [Location]
ADD
    SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN
        constraint DFValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())
    , SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN
        constraint DFValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);

ALTER TABLE [Location]
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.LocationHistory));
```

Поскольку теперь модель данных включает несколько темпоральных таблиц, для анализа AS OF (на момент времени) рекомендуется создать представление, которое извлекает необходимые данные из связанных таблиц, и применить к нему предложение SYSTEM_TIME AS OF, так как это сильно упростит восстановление состояния всей модели данных:

```sql
DROP VIEW IF EXISTS vw_ProductInventoryDetails;
GO

CREATE VIEW vw_ProductInventoryDetails
AS
    SELECT PrInv.ProductId ,PrInv.LocationId, P.ProductName, L.LocationName, PrInv.Quantity
    , P.UnitPrice, L.NumberOfEmployees
    , P.SysStartTime AS ProductStartTime, P.SysEndTime AS ProductEndTime
    , L.SysStartTime AS LocationStartTime, L.SysEndTime AS LocationEndTime
    , PrInv.SysStartTime AS InventoryStartTime, PrInv.SysEndTime AS InventoryEndTime
FROM dbo.ProductInventory as PrInv
JOIN dbo.Product AS P ON PrInv.ProductId = P.ProductID
JOIN dbo.Location AS L ON PrInv.LocationId = L.LocationID;
GO
SELECT * FROM vw_ProductInventoryDetails
    FOR SYSTEM_TIME AS OF '2015.01.01';
```

На следующем рисунке показан план выполнения, созданный для запроса SELECT. Это показывает, что вся сложность работы с темпоральными отношениями полностью обрабатывается ядром SQL Server:

![Схема плана выполнения, созданного для запроса SELECT, показывающая, что все сложности работы с временными связями полностью обрабатываются ядром SQL Server.](../../relational-databases/tables/media/asofexecutionplan.png "ASOFExecutionPlan")

Для сравнения состояния складских запасов в два момента времени (день назад и месяц назад) используйте следующий код:

```sql
DECLARE @dayAgo datetime2 = DATEADD (day, -1, SYSUTCDATETIME());
DECLARE @monthAgo datetime2 = DATEADD (month, -1, SYSUTCDATETIME());

SELECT
    inventoryDayAgo.ProductId
    , inventoryDayAgo.ProductName
    , inventoryDayAgo.LocationName
    , inventoryDayAgo.Quantity AS QuantityDayAgo,inventoryMonthAgo.Quantity AS QuantityMonthAgo
    , inventoryDayAgo.UnitPrice AS UnitPriceDayAgo, inventoryMonthAgo.UnitPrice AS UnitPriceMonthAgo
FROM vw_ProductInventoryDetails
FOR SYSTEM_TIME AS OF @dayAgo AS inventoryDayAgo
JOIN vw_ProductInventoryDetails FOR SYSTEM_TIME AS OF @monthAgo AS inventoryMonthAgo
    ON inventoryDayAgo.ProductId = inventoryMonthAgo.ProductId AND inventoryDayAgo.LocationId = inventoryMonthAgo.LocationID;
```

## <a name="anomaly-detection"></a>Обнаружение аномалий

Обнаружение аномалий (или обнаружение выбросов) представляет собой выявление элементов, которые не соответствуют ожидаемому шаблону или другим элементам в наборе данных. Для обнаружения аномалий, возникающих периодически или нерегулярно, можно использовать темпоральные таблицы с системным управлением версиями, поскольку вы можете быстро находить определенные шаблоны с помощью темпоральных запросов. Вид аномалии зависит от собираемого типа данных и бизнес-логики.

В следующем примере показана упрощенная логика для обнаружения "всплесков" в цифрах продаж. Предположим, что вы работаете с темпоральной таблицей, в которой собирается журнал приобретенных продуктов.

```sql
CREATE TABLE [dbo].[Product]
                (
            [ProdID] [int] NOT NULL PRIMARY KEY CLUSTERED
        , [ProductName] [varchar](100) NOT NULL
        , [DailySales] INT NOT NULL
        , [ValidFrom] [datetime2] GENERATED ALWAYS AS ROW START NOT NULL
        , [ValidTo] [datetime2] GENERATED ALWAYS AS ROW END NOT NULL
        , PERIOD FOR SYSTEM_TIME ([ValidFrom], [ValidTo])
    )
    WITH( SYSTEM_VERSIONING = ON (HISTORY_TABLE = [dbo].[ProductHistory]
        , DATA_CONSISTENCY_CHECK = ON ))
```

На следующей схеме показаны покупки с течением времени.

![Схема покупок с течением времени.](../../relational-databases/tables/media/temporalanomalydetection.png "TemporalAnomalyDetection")

При условии что в обычные дни количество приобретенных продуктов имеет небольшой разброс, следующий запрос определяет единичные всплески — образцы, которые значительно отличаются от своих ближайших соседей (вдвое), тогда как окружающие образцы отличаются не сильно (менее чем на 20 %):

```sql
WITH CTE (ProdId, PrevValue, CurrentValue, NextValue, ValidFrom, ValidTo)
AS
    (
        SELECT
            ProdId, LAG (DailySales, 1, 1) over (partition by ProdId order by ValidFrom) as PrevValue
            , DailySales, LEAD (DailySales, 1, 1) over (partition by ProdId order by ValidFrom) as NextValue
             , ValidFrom, ValidTo from Product
        FOR SYSTEM_TIME ALL
)

SELECT
    ProdId
    , PrevValue
    , CurrentValue
    , NextValue
    , ValidFrom
    , ValidTo
    , ABS (PrevValue - NextValue) / convert (float, (CASE WHEN NextValue > PrevValue THEN PrevValue ELSE NextValue END)) as PrevToNextDiff
    , ABS (CurrentValue - PrevValue) / convert (float, (CASE WHEN CurrentValue > PrevValue THEN PrevValue ELSE CurrentValue END)) as CurrentToPrevDiff
    , ABS (CurrentValue - NextValue) / convert (float, (CASE WHEN CurrentValue > NextValue THEN NextValue ELSE CurrentValue END)) as CurrentToNextDiff
FROM CTE
    WHERE
        ABS (PrevValue - NextValue) / (CASE WHEN NextValue > PrevValue THEN PrevValue ELSE NextValue END) < 0.2
            AND ABS (CurrentValue - PrevValue) / (CASE WHEN CurrentValue > PrevValue THEN PrevValue ELSE CurrentValue END) > 2
            AND ABS (CurrentValue - NextValue) / (CASE WHEN CurrentValue > NextValue THEN NextValue ELSE CurrentValue END) > 2;
```

> [!NOTE]
> Этот пример преднамеренно упрощен. В рабочих сценариях для определения образцов, которые не соответствуют общему шаблону, вы, скорее всего, будете использовать расширенные статистические методы.

## <a name="slowly-changing-dimensions"></a>Медленно изменяющиеся измерения

Измерения в хранилищах данных обычно содержат относительно статические данные о сущностях, таких как географические расположения, клиенты и продукты. Однако в некоторых сценариях требуется отслеживание изменений данных и в таблицах измерений. Учитывая, что изменения в измерениях происходят гораздо реже, непредсказуемо и вне рамок расписания регулярного обновления, которое применяется к таблицам фактов, такие типы таблиц измерений называются медленно изменяющимися измерениями (SCD).

Существует несколько категорий медленно изменяющихся измерений, выделяемых на основе того, как сохраняется журнал изменений.

- Тип 0. Журнал не сохраняется. Атрибуты измерений отражают исходные значения.
- Тип 1. Атрибуты измерений отражают последние значения (предыдущие значения перезаписываются).
- Тип 2. Каждая версия элемента измерения представлена в виде отдельной строки таблицы, обычно со столбцами, представляющими период действительности.
- Тип 3. Хранение ограниченного журнала для выбранных атрибутов с помощью дополнительных столбцов в той же строке.
- Тип 4. Хранение журнала в отдельной таблице. При этом исходная таблица измерения поддерживает последние (текущие) версии элементов измерений.

При выборе стратегии SCD за точное хранение таблиц измерений отвечает уровень ETL (извлечение — преобразование — загрузка), и для этого обычно требуется много кода и сложное обслуживание.

Чтобы значительно снизить сложность кода, можно использовать темпоральные таблицы с системным управлением версиями в SQL Server 2016, поскольку журнал данных сохраняется автоматически. Темпоральные таблицы в SQL Server 2016 наиболее близки к SCD типа 4, учитывая, что они реализуются с помощью двух таблиц. Однако поскольку темпоральные запросы позволяют ссылаться только на текущую таблицу, можно также рассмотреть применение временных таблиц в средах, где планируется использовать SCD типа 2.

Для преобразования обычного измерения в SCD просто создайте новую или измените существующую таблицу, чтобы она стала темпоральной таблицей с системным управлением версиями. Если существующая таблица измерения содержит исторические данные, создайте отдельную таблицу, переместите в нее исторические данные и сохраните текущие (действующие) версии измерения в исходной таблице измерения. Затем с помощью синтаксиса ALTER TABLE преобразуйте таблицу измерения в темпоральную таблицу с системным управлением версиями с предопределенной таблицей журнала.

В следующем примере показан этот процесс и предполагается, что таблица измерения DimLocation уже имеет столбцы ValidFrom и ValidTo с типом datetime2, не допускающие значения NULL, которые заполняются процессом ETL:

```sql
/*Move "closed" row versions into newly created history table*/
SELECT * INTO DimLocationHistory
    FROM DimLocation
        WHERE ValidTo < '9999-12-31 23:59:59.99';
GO
/*Create clustered columnstore index which is a very good choice in DW scenarios*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_DimLocationHistory ON DimLocationHistory
/*Delete previous versions from DimLocation which will become current table in temporal-system-versioning configuration*/
DELETE FROM DimLocation
    WHERE ValidTo < '9999-12-31 23:59:59.99';
/*Add period definition*/
ALTER TABLE DimLocation ADD PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);
/*Enable system-versioning and bind history table to the DimLocation*/
ALTER TABLE DimLocation SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DimLocationHistory));
```

Для обслуживания SCD во время процесса загрузки хранилища данных после его создания не требуется никакой дополнительный код.

На следующем рисунке показано, как можно использовать темпоральные таблицы в простом сценарии с двумя SCD (DimLocation и DimProduct) и одной таблицей фактов.

![Схема: как можно использовать темпоральные таблицы в простом сценарии с двумя SCD (DimLocation и DimProduct) и одной таблицей фактов.](../../relational-databases/tables/media/temporalscd.png "TemporalSCD")

Чтобы использовать показанные выше SCD в отчетах, необходимо эффективно настроить запросы. Например, можно вычислить общий объем продаж и среднее количество проданных продуктов на человека за последние шесть месяцев. Обратите внимание, что для обеих метрик требуется корреляция важных для анализа данных из таблицы фактов и измерений, атрибуты которых могли измениться (DimLocation.NumOfCustomers, DimProduct.UnitPrice). Следующий запрос должным образом вычисляет требуемые метрики:

```sql
DECLARE @now datetime2 = SYSUTCDATETIME()
DECLARE @sixMonthsAgo datetime2 SET
    @sixMonthsAgo = DATEADD (month, -12, SYSUTCDATETIME())

SELECT DimProduct_History.ProductId
   , DimLocation_History.LocationId
    , SUM(F.Quantity * DimProduct_History.UnitPrice) AS TotalAmount
    , AVG (F.Quantity/DimLocation_History.NumOfCustomers) AS AverageProductsPerCapita
FROM FactProductSales F
/* find corresponding record in SCD history in last 6 months, based on matching fact */
JOIN DimLocation FOR SYSTEM_TIME BETWEEN @sixMonthsAgo AND @now AS DimLocation_History
    ON DimLocation_History.LocationId = F.LocationId
        AND F.FactDate BETWEEN DimLocation_History.ValidFrom AND DimLocation_History.ValidTo
/* find corresponding record in SCD history in last 6 months, based on matching fact */
JOIN DimProduct FOR SYSTEM_TIME BETWEEN @sixMonthsAgo AND @now AS DimProduct_History
    ON DimProduct_History.ProductId = F.ProductId
        AND F.FactDate BETWEEN DimProduct_History.ValidFrom AND DimProduct_History.ValidTo
    WHERE F.FactDate BETWEEN @sixMonthsAgo AND @now
GROUP BY DimProduct_History.ProductId, DimLocation_History.LocationId ;
```

**Рекомендации**

- Использование темпоральных таблиц с системным управлением версиями для SCD допустимо, если срок действия, вычисленный на основе времени транзакции базы данных, согласуется с вашей бизнес-логикой. При загрузке данных со значительной задержкой время транзакции может быть неприемлемо.
- По умолчанию темпоральные таблицы с системным управлением версиями не разрешают изменение исторических данных после загрузки (журнал можно изменить, установив значение OFF для параметра SYSTEM_VERSIONING). Это может быть ограничением в случаях, когда изменение исторических данных происходит регулярно.
- Темпоральные таблицы с системным управлением версиями формируют версию строки при любом изменении столбца. Если вы хотите запретить создание новых версий при изменении определенных столбцов, необходимо включить это ограничение в логику ETL.
- Если ожидается значительное число исторических строк в таблицах SCD, рассмотрите возможность использования кластеризованного индекса columnstore в качестве основного хранилища для таблицы журнала. Это уменьшит место, занимаемое таблицей журнала, и ускорит аналитические запросы.

## <a name="repairing-row-level-data-corruption"></a>Восстановление поврежденных данных на уровне строк

Вы можете использовать исторические данные в темпоральных таблицах с системным управлением версиями, чтобы быстро восстанавливать отдельные строки в любое из ранее записанных состояний. Это свойство темпоральных таблиц очень удобно использовать, когда можно найти затронутые строки или когда известно время нежелательного изменения данных. В этом случае вы можете выполнить восстановление очень эффективно без задействования резервных копий.

Такой подход имеет несколько преимуществ.

- Вы можете очень точно управлять областью восстановления. Записи, которые не были затронуты, должны остаться в последнем состоянии, что часто является принципиальным требованием.
- Операция очень эффективна, и база данных остается доступна для всех рабочих нагрузок с использованием данных.
- Сама операция восстановления поддерживает управление версиями. Имеется журнал аудита для самой операции восстановления, так что при необходимости позже можно проанализировать произошедшее.

Действие восстановления можно довольно легко автоматизировать. Ниже приведен пример кода хранимой процедуры, которая выполняет восстановление данных для таблицы Employee, используемой в сценарии аудита данных.

```sql
DROP PROCEDURE IF EXISTS sp_RepairEmployeeRecord;
GO

CREATE PROCEDURE sp_RepairEmployeeRecord
    @EmployeeID INT,
    @versionNumber INT = 1
AS

;WITH History
AS
(
        /* Order historical rows by their age in DESC order*/
        SELECT ROW_NUMBER () OVER (PARTITION BY EmployeeID ORDER BY [ValidTo] DESC) AS RN, *
        FROM Employee FOR SYSTEM_TIME ALL WHERE YEAR (ValidTo) < 9999 AND Employee.EmployeeID = @EmployeeID
)

/*Update current row by using N-th row version from history (default is 1 - i.e. last version)*/
UPDATE Employee
    SET [Position] = H.[Position], [Department] = H.Department, [Address] = H.[Address], AnnualSalary = H.AnnualSalary
    FROM Employee E JOIN History H ON E.EmployeeID = H.EmployeeID AND RN = @versionNumber
    WHERE E.EmployeeID = @EmployeeID
```

Эта хранимая процедура принимает входные параметры @EmployeeID и @versionNumber. По умолчанию эта процедура восстанавливает состояние строки из журнала до последней версии (@versionNumber = 1).

На следующем рисунке показано состояние строки до и после вызова процедуры. Красный прямоугольник отмечает текущую, неправильную версию строки, а зеленый прямоугольник отмечает правильную версию из журнала.

![Снимок экрана: состояние строки до и после вызова процедуры.](../../relational-databases/tables/media/temporalusagerepair1.png "TemporalUsageRepair1")

```sql
EXEC sp_RepairEmployeeRecord @EmployeeID = 1, @versionNumber = 1
```

![Снимок экрана: исправленная строка.](../../relational-databases/tables/media/temporalusagerepair2.png "TemporalUsageRepair2")

Можно определить эту хранимую процедуру восстановления таким образом, чтобы она принимала точное время вместо версии строки. Она будет восстанавливать строку до той версии, которая была активна на предоставленный момент времени (т. е. в состояние на момент времени).

```sql
DROP PROCEDURE IF EXISTS sp_RepairEmployeeRecordAsOf;
GO

CREATE PROCEDURE sp_RepairEmployeeRecordAsOf
    @EmployeeID INT,
    @asOf datetime2
AS

/*Update current row to the state that was actual AS OF provided date*/
UPDATE Employee
    SET [Position] = History.[Position], [Department] = History.Department, [Address] = History.[Address], AnnualSalary = History.AnnualSalary
    FROM Employee AS E JOIN Employee FOR SYSTEM_TIME AS OF @asOf AS History ON E.EmployeeID = History.EmployeeID
    WHERE E.EmployeeID = @EmployeeID
```

На следующем рисунке показан сценарий восстановления для тех же данных с условием времени. Здесь выделены параметр @asOf, выбранная строка в журнале, которая была действующей на указанный момент времени, и новая версия строки в текущей таблице после операции восстановления.

![Снимок экрана: сценарий восстановления с условием времени.](../../relational-databases/tables/media/temporalusagerepair3.png "TemporalUsageRepair3")

Корректировка данных может стать частью автоматической загрузки данных в системах хранения данных и подготовки отчетов. Если только что обновленное значение неправильно, для устранения этого во многих сценариях достаточно восстановить предыдущую версию. На следующей схеме показано, как этот процесс можно автоматизировать.

![Схема, показывающая, как можно автоматизировать процесс.](../../relational-databases/tables/media/temporalusagerepair4.png "TemporalUsageRepair4")

## <a name="next-steps"></a>Дальнейшие действия

- [Темпоральные таблицы](../../relational-databases/tables/temporal-tables.md)
 [Приступая к работе c темпоральными таблицами с системным управлением версиями](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Проверка согласованности системной темпоральной таблицы](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Секционирование с помощью темпоральных таблиц](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Рекомендации и ограничения для темпоральной таблицы](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Безопасность темпоральной таблицы](../../relational-databases/tables/temporal-table-security.md)
- [Темпоральные таблицы с системным управлением версиями и таблицы, оптимизированные для памяти](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Представления и функции метаданных для временной таблицы](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)

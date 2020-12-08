---
description: Оптимизация обработки JSON с помощью выполняющейся в памяти OLTP
title: Оптимизация обработки JSON с помощью выполняющейся в памяти OLTP
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
ms.assetid: d9c5adb1-3209-4186-bc10-8e41a26f5e57
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 54a6f6b52229b76ce2ffefd0a89d11130d8e2656
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595086"
---
# <a name="optimize-json-processing-with-in-memory-oltp"></a>Оптимизация обработки JSON с помощью выполняющейся в памяти OLTP
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2017-asdb.md)]

База данных SQL Azure и SQL Server позволяют работать с текстами в формате JSON. Чтобы увеличить производительность запросов, которые обрабатывают данные JSON, вы можете хранить документы JSON в таблицах, оптимизированных для памяти, используя стандартные столбцы строкового типа (типа NVARCHAR). Хранение данных JSON в таблицах, оптимизированных для памяти, повышает производительность запросов за счет доступа к данным в памяти в режиме без блокировки.

## <a name="store-json-in-memory-optimized-tables"></a>Хранение документов JSON в таблицах, оптимизированных для памяти
В примере ниже показана таблица `Product`, оптимизированная для памяти, с двумя столбцами JSON — `Tags` и `Data`.

```sql
CREATE SCHEMA xtp;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED, --standard column
    Name nvarchar(400) NOT NULL, --standard column
    Price float, --standard column

    Tags nvarchar(400),--json stored in string column
    Data nvarchar(4000) --json stored in string column

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="optimize-json-processing-with-additional-in-memory-features"></a>Оптимизация обработки JSON с помощью дополнительных функций выполнения в памяти
Функции, доступные в SQL Server и базе данных SQL Azure, позволяют полностью интегрировать возможности JSON с существующими технологиями выполнения OLTP в памяти. Вы сможете выполнять следующее:
 - [Проверять структуру документов JSON](#validate), хранящихся в таблицах, оптимизированных для памяти, с помощью компилируемых в собственном коде ограничений CHECK.
 - [Предоставлять строго типизированные значения](#computedcol), сохраненные в документах JSON, с помощью вычисляемых столбцов.
 - [Индексировать значения](#index) в документах JSON с помощью индексов, оптимизированных для памяти.
 - [Компилировать в собственном коде SQL-запросы](#compile), использующие значения из документов JSON или форматирующие результаты в виде текста JSON.

## <a name="validate-json-columns"></a><a name="validate"></a> Проверка столбцов JSON
База данных SQL Azure и SQL Server позволяют добавлять скомпилированные в собственном коде ограничения CHECK, проверяющие содержимое документов JSON, которые хранятся в столбце строкового типа. Благодаря скомпилированным в собственном коде проверочным ограничениям JSON тексты JSON хранятся в таблицах, оптимизированных для памяти, в правильном формате.

В следующем примере создается таблица `Product` с JSON-столбцом `Tags`. Столбец `Tags` имеет ограничение CHECK, которое использует функцию `ISJSON` для проверки в столбце текста JSON.

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Tags nvarchar(400)
            CONSTRAINT [Tags should be formatted as JSON]
                CHECK (ISJSON(Tags)=1),
    Data nvarchar(4000)

) WITH (MEMORY_OPTIMIZED=ON);
```

Вы можете также добавить скомпилированное в собственном коде ограничение CHECK в существующую таблицу, содержащую столбцы JSON.

```sql
ALTER TABLE xtp.Product
    ADD CONSTRAINT [Data should be JSON]
        CHECK (ISJSON(Data)=1)
```

## <a name="expose-json-values-using-computed-columns"></a><a name="computedcol"></a> Предоставление значений JSON с помощью вычисляемых столбцов
Вычисляемые столбцы позволяют вам предоставлять значения из текста JSON и получать доступ к этим значениям без необходимости повторно получать значения из текста JSON и повторно анализировать структуру JSON. Предоставленные этим способом значения строго типизированы и физически материализуются в вычисляемых столбцах. Доступ к значениям JSON с помощью материализованных вычисляемых столбцов выполняется быстрее, чем прямой доступ к значениям в документе JSON.

В примере ниже показано, как предоставить следующие два значения из столбца JSON `Data`:
-   страну, в которой был изготовлен товар;
-   себестоимость производства товара.

В этом примере вычисляемые столбцы `MadeIn` и `Cost` обновляются каждый раз, когда изменяется документ JSON, сохраненный в столбце `Data`.

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float)

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="index-values-in-json-columns"></a><a name="index"></a> Индексирование значений в столбцах JSON
База данных SQL Azure и SQL Server позволяют индексировать значения в столбцах JSON с помощью оптимизированных для памяти индексов. Индексируемые значения JSON должны быть строго типизированными и предоставленными с помощью вычисляемых столбцов, как описано в предыдущем примере.

Значения в столбцах JSON можно индексировать с помощью двух стандартных индексов — некластеризованного и хэш-индекса.
-   Некластеризованные индексы оптимизируют запросы, которые выбирают диапазоны строк по какому-либо значению JSON или сортируют результаты по значениям JSON.
-   Хэш-индексы оптимизируют запросы, которые выбирают одну или несколько записей, указывая точное значение для поиска.

В следующем примере создается таблица, которая предоставляет значения JSON с помощью двух вычисляемых столбцов. В примере создается некластеризованный индекс в одном значении JSON и хэш-индекс — в другом.

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float) PERSISTED,

    INDEX [idx_Product_MadeIn] NONCLUSTERED (MadeIn)

) WITH (MEMORY_OPTIMIZED=ON)

ALTER TABLE Product
    ADD INDEX [idx_Product_Cost] NONCLUSTERED HASH(Cost)
        WITH (BUCKET_COUNT=20000)
```

## <a name="native-compilation-of-json-queries"></a><a name="compile"></a> Компиляция запросов JSON в собственном коде
Если процедуры, функции и триггеры содержат запросы, которые используют встроенные функции JSON, компиляция в машинный код повышает производительность этих запросов и снижает количество циклов ЦП, необходимых для их выполнения.

В примере ниже показана скомпилированная в собственном коде процедура, использующая несколько функций JSON: **JSON_VALUE**, **OPENJSON** и **JSON_MODIFY**.

```sql
CREATE PROCEDURE xtp.ProductList(@ProductIds nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    SELECT ProductID,Name,Price,Data,Tags, JSON_VALUE(data,'$.MadeIn') AS MadeIn
    FROM xtp.Product
        JOIN OPENJSON(@ProductIds)
            ON ProductID = value
END;

CREATE PROCEDURE xtp.UpdateProductData(@ProductId int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE xtp.Product
    SET Data = JSON_MODIFY(Data, @Property, @Value)
    WHERE ProductID = @ProductId;
END
```

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Дополнительные сведения о JSON в SQL Server и базе данных SQL Azure  
  
### <a name="microsoft-videos"></a>Видео Майкрософт

Наглядные инструкции по встроенной поддержке JSON в SQL Server и базе данных SQL Azure см. в следующих видео.

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 и поддержка JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Использование JSON в SQL Server 2016 и базе данных SQL Azure)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON как мост между NoSQL и реляционными решениями)

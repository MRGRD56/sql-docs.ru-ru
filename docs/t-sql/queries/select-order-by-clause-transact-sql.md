---
description: SELECT — предложение ORDER BY (Transact-SQL)
title: Предложение ORDER BY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ORDER_TSQL
- BY
- ORDER_BY_TSQL
- BY_TSQL
- ORDER
- ORDER BY
dev_langs:
- TSQL
helpviewer_keywords:
- ad-hoc query paging
- OFFSET clause
- SELECT statement [SQL Server], FETCH clause
- clauses [SQL Server], ORDER BY
- SELECT statement [SQL Server], limiting the rows returned
- data [SQL Server], limiting the rows returned
- data [SQL Server], ad-hoc query paging
- sort orders [SQL Server]
- SELECT statement [SQL Server], OFFSET clause
- ORDER BY clause [Transact-SQL]
- LIMIT
- limiting result sets
- SELECT statement [SQL Server], ORDER BY clause
- query paging
- data [SQL Server], sorting
- limiting data returned in a query
- sort orders [SQL Server], ORDER BY clause
- FETCH clause
ms.assetid: bb394abe-cae6-4905-b5c6-8daaded77742
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e7deb9fdda73dfc85e92341ee77781b6d5491fba
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753914"
---
# <a name="select---order-by-clause-transact-sql"></a>SELECT — предложение ORDER BY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Сортирует данные, возвращенные запросом в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это предложение используется для следующих целей:  
  
-   Упорядочение результирующего набора запроса по заданному списку столбцов и (дополнительно) ограничение числа возвращаемых строк указанным диапазоном. Порядок, в котором строки возвращаются в результирующем наборе, не гарантируется, если не указано предложение ORDER BY.  
  
-   Определение порядка, в котором значения [ранжирующей функции](../../t-sql/functions/ranking-functions-transact-sql.md) применяются к результирующему набору.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  ORDER BY не поддерживается в инструкциях SELECT/INTO или CREATE TABLE AS SELECT (CTAS) в [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]   
[ <offset_fetch> ]  
  
<offset_fetch> ::=  
{   
    OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }  
    [  
      FETCH { FIRST | NEXT } {integer_constant | fetch_row_count_expression } { ROW | ROWS } ONLY  
    ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ ORDER BY   
    {  
    order_by_expression   
    [ ASC | DESC ]   
    } [ ,...n ]   
]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *order_by_expression*  
 Указывает столбец или выражение, по которому производится сортировка результирующего набора запроса. Столбец сортировки может быть указан с помощью имени или псевдонима столбца или неотрицательного целого числа, представляющего позицию столбца в списке выбора.  
  
 Можно указать несколько столбцов сортировки. Имена столбцов должны быть уникальными. Последовательность столбцов сортировки в предложении ORDER BY определяет организацию упорядоченного результирующего набора. Иными словами, результирующий набор сортируется по первому столбцу, затем упорядоченный список сортируется по второму и т. д.  
  
 Имена столбцов, на которые содержатся ссылки в предложении ORDER BY, должны однозначно соответствовать столбцу или псевдониму столбца в списке выбора либо столбцу, определенному в таблице, указанной в предложении FROM. Если предложение ORDER BY ссылается на псевдоним столбца в списке выбора, псевдоним должен использоваться отдельно, а не как часть выражения в предложении ORDER BY, например:
 
```sql
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName; -- correct 
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName + ''; -- wrong
```
  
 COLLATE *collation_name*  
 Указывает, что операция ORDER BY должна выполняться в соответствии с параметрами сортировки, указанными в аргументе *collation_name*, но не в соответствии с параметрами сортировки столбца, определенными в таблице или представлении. Аргументом *collation_name* может быть либо имя параметров сортировки Windows, либо имя параметров сортировки SQL. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). Аргумент COLLATE применяется только к столбцам типа **char**, **varchar**, **nchar** и **nvarchar**.  
  
 **ASC** | DESC  
 Указывает порядок сортировки значений в указанном столбце — по возрастанию или по убыванию. Значение ASC сортирует от низких значений к высоким. Значение DESC сортирует от высоких значений к низким. Порядок сортировки по умолчанию — ASC. Значения NULL рассматриваются как минимально возможные значения.  
  
 OFFSET { *integer_constant* | *offset_row_count_expression* } { ROW | ROWS }  
 Указывает число сток, которые необходимо пропустить, прежде чем будет начат возврат строк из выражения запроса. Это значение может быть целочисленной константой или выражением, значение которого больше нуля или равно нулю.  
  
**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *offset_row_count_expression* может быть переменной, параметром или вложенным запросом, возвращающим скалярную константу. При использовании вложенного запроса он не должен ссылаться на какие-либо столбцы, определенные в области внешнего запроса. Иными словами, он не может коррелировать с внешним запросом.  
  
 ROW и ROWS являются синонимами и оставлены для совместимости со стандартом ANSI.  
  
 В плане выполнения запроса значение смещения строки отображается в атрибуте **Offset** оператора запроса TOP.  
  
 FETCH { FIRST \| NEXT } { *integer_constant* \| *fetch_row_count_expression* } { ROW \| ROWS } ONLY  
 Указывает число строк, возвращаемых после обработки предложения OFFSET. Это значение может быть целочисленной константой или выражением, значение которого больше единицы или равно единице.  
  
**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 *fetch_row_count_expression* может быть переменной, параметром или вложенным запросом, возвращающим скалярную константу. При использовании вложенного запроса он не должен ссылаться на какие-либо столбцы, определенные в области внешнего запроса. Иными словами, он не может коррелировать с внешним запросом.  
  
 FIRST и NEXT являются синонимами и предусмотрены для совместимости со стандартом ANSI.  
  
 ROW и ROWS являются синонимами и оставлены для совместимости со стандартом ANSI.  
  
 В плане выполнения запроса значение смещения строки отображается в атрибуте **Rows** или **Top** оператора запроса TOP.  
  
## <a name="best-practices"></a>Рекомендации  
 Избегайте указания столбцов в предложении ORDER BY по их порядковому номеру в списке выбора. Например, хотя инструкция `SELECT ProductID, Name FROM Production.Production ORDER BY 2` верна, она будет не очень понятна другим пользователям по сравнению с тем случаем, когда столбцы указаны по именам. Кроме того, если список выбора изменится, в частности изменится порядок столбца или будут добавлены новые столбцы, то это потребует изменения предложения ORDER BY во избежание непредвиденных результатов.  
  
 В инструкции SELECT TOP (*N*) всегда указывайте предложение ORDER BY. Это единственный способ предсказуемым образом отметить строки, которые были обработаны предложением TOP. Дополнительные сведения см. в разделе [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md).  
  
## <a name="interoperability"></a>Совместимость  
 При использовании в инструкции SELECT...INTO или INSERT...SELECT предложения ORDER BY для вставки строк из другого источника вставка строк в указанном порядке не гарантируется.  
  
 Использование OFFSET и FETCH в представлении не приведет к изменению его свойства Updateability.  
  
## <a name="limitations-and-restrictions"></a>Ограничения  
 Нет ограничения на число столбцов в предложении ORDER BY, однако общий размер столбцов, перечисленных в нем, не может превышать 8060 байт.  
  
 Столбцы типа **ntext**, **text**, **image**, **geography**, **geometry** и **xml** не могут использоваться в предложении ORDER BY.  
  
 Нельзя указывать целое число или константу, если аргумент *order_by_expression* присутствует в ранжирующей функции. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 Если в качестве имени таблицы в предложении FROM используется псевдоним, то только псевдоним может быть использован для обозначения столбца этой таблицы в предложении ORDER BY.  
  
 Имена и псевдонимы столбцов, указанные в предложении ORDER BY, должны быть определены в списке выбора, если инструкция SELECT содержит одно из следующих предложений или операторов:  
  
-   UNION, оператор  
  
-   Оператор EXCEPT  
  
-   INTERSECT, оператор  
  
-   SELECT DISTINCT  
  
 Кроме того, если в инструкцию входит оператор UNION, EXCEPT или INTERSECT, то имена и псевдонимы столбцов должны быть указаны в списке выбора первого (слева) запроса.  
  
 В запросе, содержащем оператор UNION, EXCEPT или INTERSECT, предложение ORDER BY допускается только в конце инструкции. Это ограничение применяется только при использовании операторов UNION, EXCEPT и INTERSECT в запросах верхнего уровня, но не во вложенных запросах. См подраздел «Примеры» ниже.  
  
 Предложение ORDER BY недопустимо в представлениях, встроенных функциях, производных таблицах и вложенных запросах, если также не указаны предложения TOP либо OFFSET и FETCH. В этих объектах предложение ORDER BY используется только для определения строк, возвращаемых предложением TOP или OFFSET и FETCH. Предложение ORDER BY не гарантирует упорядочивания результатов при запросе этих конструкций, если оно не указано в самом запросе.  
  
 Предложения OFFSET и FETCH не поддерживаются в индексированных представлениях и представлениях, определенных с предложением CHECK OPTION.  
  
 Предложения OFFSET и FETCH могут быть использованы в любом запросе, допускающем применение TOP и ORDER BY, со следующими ограничениями.  
  
-   Предложение OVER не поддерживает OFFSET и FETCH.  
  
-   Предложения OFFSET и FETCH не могут быть указаны прямо в инструкциях INSERT, UPDATE, MERGE и DELETE, но могут быть указаны во вложенных запросах, определяемых этими инструкциями. Например, в инструкции INSERT INTO SELECT предложения OFFSET и FETCH могут быть указаны в инструкции SELECT.  
  
-   В запросе, содержащем оператор UNION, EXCEPT или INTERSECT, предложения OFFSET и FETCH могут быть указаны только в конечном запросе, который определяет порядок следования результатов запроса.  
  
-   TOP нельзя сочетать с OFFSET и FETCH в одном выражении запроса (в той же области запроса).  
  
## <a name="using-offset-and-fetch-to-limit-the-rows-returned"></a>Использование OFFSET и FETCH для ограничения числа возвращаемых строк  
 Для разбиения на страницы и ограничения числа строк, передаваемых клиентскому приложению, рекомендуется пользоваться предложениями OFFSET и FETCH, а не предложением TOP.  
  
 Применение в качестве решения для разбиения на страницы предложений OFFSET и FETCH потребует однократного выполнения запроса для каждой «страницы» данных, возвращаемых клиентскому приложению. Например, чтобы вернуть результаты запроса блоками по 10 строк, необходимо выполнить запрос для получения строк с 1 по 10, затем еще раз для получения строк с 11 по 20 и так далее. Каждый запрос выполняется независимо и никаким образом не связан с другими запросами. Это означает, что в отличие от использования курсора, где запрос выполняется всего один раз, а текущее состояние хранится на сервере, за отслеживание состояния отвечает клиентское приложение. Чтобы добиться стабильных результатов между запросами с предложениями OFFSET и FETCH, должны выполняться следующие условия.  
  
1.  Базовые данные, используемые запросом, должны быть неизменными. Иными словами, либо строки, обработанные запросом, не должны обновляться, либо все запросы страниц выполняемого запроса должны выполняться в одной транзакции, использующей моментальный снимок или сериализуемую изоляцию транзакции. Дополнительные сведения об уровнях изоляции транзакции см. в разделе [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
2.  Предложение ORDER BY содержит столбец или сочетание столбцов, которые гарантированно уникальны.  
  
 См. пример «Выполнение нескольких запросов в одной транзакции» в подразделе «Примеры» ниже в этом разделе.  
  
 Если согласованность планов выполнения важна для решения разбиения на страницы, подумайте над использованием указания запросов OPTIMIZE FOR для параметров OFFSET и FETCH. См. пункт «Указание выражений для значений OFFSET и FETCH» в подразделе «Примеры» ниже в этом разделе. Дополнительные сведения об OPTIMZE FOR см. в статье [Указания запросов (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Примеры  
  
|Категория|Используемые элементы синтаксиса|  
|--------------|------------------------------|  
|[Основной синтаксис](#BasicSyntax)|ORDER BY|  
|[Указание порядка по возрастанию или по убыванию](#SortOrder)|DESC • ASC|  
|[Указание параметров сортировки](#Collation)|COLLATE|  
|[Указание условного порядка](#Case)|CASE, выражение|  
|[Использование ORDER BY в ранжирующей функции](#Rank)|Ранжирующие функции|  
|[Ограничение числа возвращаемых строк](#Offset)|OFFSET • FETCH|  
|[Использование ORDER BY с UNION, EXCEPT и INTERSECT](#Union)|UNION|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Основной синтаксис  
 В примерах этого раздела показана базовая функциональность предложения ORDER BY с использованием минимально необходимого синтаксиса.  
  
#### <a name="a-specifying-a-single-column-defined-in-the-select-list"></a>A. Указание единственного столбца, определенного в списке выбора  
 В следующем примере производится упорядочение результирующего набора по числовому столбцу `ProductID`. Поскольку конкретный порядок сортировки не указан, используется порядок по умолчанию (по возрастанию).  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID;  
```  
  
#### <a name="b-specifying-a-column-that-is-not-defined-in-the-select-list"></a>Б. Указание столбца, не определенного в списке выбора  
 В следующем примере результирующий набор упорядочивается по столбцу, не включенному в список выбора, но определенному в таблице, указанной в предложении FROM.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name, Color  
FROM Production.Product  
ORDER BY ListPrice;  
```  
  
#### <a name="c-specifying-an-alias-as-the-sort-column"></a>В. Указание псевдонима в качестве столбца сортировки  
 В следующем примере в качестве столбца сортировки указывается псевдоним столбца `SchemaName`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT name, SCHEMA_NAME(schema_id) AS SchemaName  
FROM sys.objects  
WHERE type = 'U'  
ORDER BY SchemaName;  
```  
  
#### <a name="d-specifying-an-expression-as-the-sort-column"></a>Г. Указание выражения в качестве столбца сортировки  
 В следующем примере в качестве столбца сортировки используется выражение. Выражение определено с использованием функции DATEPART, чтобы сортировать результирующий набор по году поступления сотрудника на работу.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY DATEPART(year, HireDate);  
```  
  
###  <a name="specifying-ascending-and-descending-sort-order"></a><a name="SortOrder"></a> Указание порядка по возрастанию или по убыванию  
  
#### <a name="a-specifying-a-descending-order"></a>A. Указание порядка по убыванию  
 В следующем примере производится упорядочение результирующего набора по числовому столбцу `ProductID` в убывающем порядке.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID DESC;  
```  
  
#### <a name="b-specifying-an-ascending-order"></a>Б. Указание порядка по возрастанию  
 В следующем примере производится упорядочение результирующего набора по столбцу `Name` в возрастающем порядке. Символьные значения сортируются в алфавитном порядке, а не в числовом. Иными словами, отсортированное значение 10 находится перед 2.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY Name ASC ;  
```  
  
#### <a name="c-specifying-both-ascending-and-descending-order"></a>В. Указание порядка и по возрастанию, и по убыванию  
 В следующем примере производится упорядочение результирующего набора по двум столбцам. Результирующий набор запроса сначала сортируется по возрастанию по столбцу `FirstName`, а затем в убывающем порядке по столбцу `LastName`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'R%'  
ORDER BY FirstName ASC, LastName DESC ;  
```  
  
###  <a name="specifying-a-collation"></a><a name="Collation"></a> Указание параметров сортировки  
 Следующий пример показывает, как задание параметров сортировки в предложении ORDER BY может изменить порядок, в котором возвращаются результаты запроса. Созданная таблица содержит столбец, определенный без учета регистра и параметров сортировки диакритических знаков. Вставленные значения имеют различные сочетания регистра и диакритических знаков. Поскольку параметры сортировки в предложении ORDER BY не заданы, первый запрос при упорядочении значений использует порядок сортировки столбца. Во втором запросе в предложении ORDER BY указаны параметры сортировки без учета регистра символов и диакритических знаков, поэтому строки возвращаются в другом порядке.  
  
```sql
USE tempdb;  
GO  
CREATE TABLE #t1 (name NVARCHAR(15) COLLATE Latin1_General_CI_AI)  
GO  
INSERT INTO #t1 VALUES(N'Sánchez'),(N'Sanchez'),(N'sánchez'),(N'sanchez');  
  
-- This query uses the collation specified for the column 'name' for sorting.  
SELECT name  
FROM #t1  
ORDER BY name;  
-- This query uses the collation specified in the ORDER BY clause for sorting.  
SELECT name  
FROM #t1  
ORDER BY name COLLATE Latin1_General_CS_AS;  
```  
  
###  <a name="specifying-a-conditional-order"></a><a name="Case"></a> Указание условного порядка  
 В следующем примере выражение CASE используется в предложении ORDER BY, чтобы условно определить порядок сортировки строк на основе значения заданного столбца таблицы. В первом примере вычисляется значение столбца `SalariedFlag` таблицы `HumanResources.Employee`. Сотрудники, для которых столбец `SalariedFlag` имеет значение 1, возвращаются в порядке `BusinessEntityID` (по убыванию). Сотрудники, для которых столбец `SalariedFlag` имеет значение 0, возвращаются в порядке `BusinessEntityID` (по возрастанию). Во втором примере результирующий набор упорядочивается по столбцу `TerritoryName`, если столбец `CountryRegionName` содержит значение «США», и по столбцу `CountryRegionName` в остальных строках.  
  
```sql
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO  
```  
  
```sql
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
###  <a name="using-order-by-in-a-ranking-function"></a><a name="Rank"></a> Использование ORDER BY в ранжирующей функции  
 В следующем примере предложение ORDER BY используется в ранжирующих функциях ROW_NUMBER, RANK, DENSE_RANK и NTILE.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS "Rank"  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS "Quartile"  
    ,s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
###  <a name="limiting-the-number-of-rows-returned"></a><a name="Offset"></a> Ограничение числа возвращаемых строк  
 В следующих примерах предложения OFFSET и FETCH ограничивают число строк, возвращаемых запросом.  
  
**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
#### <a name="a-specifying-integer-constants-for-offset-and-fetch-values"></a>A. Указание целочисленных констант в качестве значений OFFSET и FETCH  
 В следующем примере в качестве значений предложений OFFSET и FETCH указана целочисленная константа. Первый запрос возвращает все строки, отсортированные по столбцу `DepartmentID`. Сравните результаты, возвращенные этим запросом, с результатами двух следующих запросов. В следующем запросе предложение `OFFSET 5 ROWS` используется для пропуска первых 5 строк и возврата оставшихся. Конечный запрос содержит предложение `OFFSET 0 ROWS`, чтобы начать с первой строки, а затем предложение `FETCH NEXT 10 ROWS ONLY`, ограничивающее число возвращаемых строк до 10 из сортированного результирующего набора.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Return all rows sorted by the column DepartmentID.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID;  
  
-- Skip the first 5 rows from the sorted result set and return all remaining rows.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID OFFSET 5 ROWS;  
  
-- Skip 0 rows and return only the first 10 rows from the sorted result set.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID   
    OFFSET 0 ROWS  
    FETCH NEXT 10 ROWS ONLY;  
```  
  
#### <a name="b-specifying-variables-for-offset-and-fetch-values"></a>Б. Указание переменных в качестве значений OFFSET и FETCH  
 В следующем примере объявлены переменные `@RowsToSkip` и `@FetchRows`. Затем эти переменные указаны в предложениях OFFSET и FETCH.  
  
```sql
USE AdventureWorks2012;  
GO  
-- Specifying variables for OFFSET and FETCH values    
DECLARE @RowsToSkip TINYINT = 2
      , @FetchRows TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @RowsToSkip ROWS   
    FETCH NEXT @FetchRows ROWS ONLY;  
```  
  
#### <a name="c-specifying-expressions-for-offset-and-fetch-values"></a>В. Указание выражений в качестве значений OFFSET и FETCH  
 В следующем примере выражение `@StartingRowNumber - 1` определяет значение OFFSET, а выражение `@EndingRowNumber - @StartingRowNumber + 1` — значение FETCH. Кроме этого, приведено указание запроса OPTIMIZE FOR. Это указание можно использовать, чтобы предоставить конкретное значение для локальной переменной при компиляции и оптимизации запросов. Значение используется только в процессе оптимизации запроса, но не в процессе выполнения. Дополнительные сведения см. в разделе [Указания запросов (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Specifying expressions for OFFSET and FETCH values      
DECLARE @StartingRowNumber TINYINT = 1  
      , @EndingRowNumber TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @EndingRowNumber - @StartingRowNumber + 1 ROWS ONLY  
OPTION ( OPTIMIZE FOR (@StartingRowNumber = 1, @EndingRowNumber = 20) );  
```  
  
#### <a name="d-specifying-a-constant-scalar-subquery-for-offset-and-fetch-values"></a>Г. Указание вложенного запроса, возвращающего скалярную константу, в качестве значений OFFSET и FETCH  
 В следующем примере вложенный запрос, возвращающий скалярную константу, используется для определения значения для предложения FETCH. Вложенный запрос возвращает единственное значение из столбца `PageSize` в таблице `dbo.AppSettings`.  
  
```sql
-- Specifying a constant scalar subquery  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.AppSettings (AppSettingID INT NOT NULL, PageSize INT NOT NULL);  
GO  
INSERT INTO dbo.AppSettings VALUES(1, 10);  
GO  
DECLARE @StartingRowNumber TINYINT = 1;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber ROWS   
    FETCH NEXT (SELECT PageSize FROM dbo.AppSettings WHERE AppSettingID = 1) ROWS ONLY;  
```  
  
#### <a name="e-running-multiple-queries-in-a-single-transaction"></a>Д. Выполнение нескольких запросов в одной транзакции  
 В следующем примере показан один из методов реализации решения разбиения на страницы, который обеспечивает стабильные результаты, возвращаемые во всех запросах. Этот запрос выполняется в одной транзакции уровня изоляции моментального снимка, а столбец, указанный в предложении ORDER BY, гарантирует уникальность столбца.  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Ensure the database can support the snapshot isolation level set for the query.  
IF (SELECT snapshot_isolation_state FROM sys.databases WHERE name = N'AdventureWorks2012') = 0  
    ALTER DATABASE AdventureWorks2012 SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Set the transaction isolation level  to SNAPSHOT for this query.  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
-- Beginning the transaction.
BEGIN TRANSACTION;  
GO  
-- Declare and set the variables for the OFFSET and FETCH values.  
DECLARE @StartingRowNumber INT = 1  
      , @RowCountPerPage INT = 3;  
  
-- Create the condition to stop the transaction after all rows have been returned.  
WHILE (SELECT COUNT(*) FROM HumanResources.Department) >= @StartingRowNumber  
BEGIN  
  
-- Run the query until the stop condition is met.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @RowCountPerPage ROWS ONLY;  
  
-- Increment @StartingRowNumber value.  
SET @StartingRowNumber = @StartingRowNumber + @RowCountPerPage;  
CONTINUE  
END;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
###  <a name="using-order-by-with-union-except-and-intersect"></a><a name="Union"></a> Использование ORDER BY с UNION, EXCEPT и INTERSECT  
 Если запрос содержит оператор UNION, EXCEPT или INTERSECT, предложение ORDER BY должно быть указано в конце инструкции, а результаты объединенного запроса должны быть отсортированы. В следующем примере возвращаются все продукты желтого или красного цвета, отсортированные в общем списке по столбцу `ListPrice`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Red'  
-- ORDER BY cannot be specified here.  
UNION ALL  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Yellow'  
ORDER BY ListPrice ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В следующем примере демонстрируется упорядочение результирующего набора по числовому столбцу `EmployeeKey` в возрастающем порядке.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey;  
```  
  
 В следующем примере производится упорядочение результирующего набора по числовому столбцу `EmployeeKey` в убывающем порядке.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey DESC;  
```  
  
 В следующем примере производится упорядочение результирующего набора по столбцу `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName;  
```  
  
 В следующем примере производится упорядочение по двум столбцам. Этот запрос сначала сортирует в возрастающем порядке по столбцу `FirstName`, а затем сортирует общие значения `FirstName` в убывающем порядке по столбцу `LastName`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName, FirstName;  
```  
  
## <a name="see-also"></a>См. также:  
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
 [Ранжирующие функции (Transact-SQL)](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)   
 [Указания запросов (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)   
 [EXCEPT и INTERSECT (Transact-SQL)](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UNION (Transact-SQL)](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  
  


---
description: SELECT — GROUP BY (Transact-SQL)
title: GROUP BY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUP
- CUBE
- ROLLUP
- GROUPING_SETS_TSQL
- GROUP BY
- GROUPING SETS
- GROUP_BY_TSQL
- GROUP_TSQL
- CUBE_TSQL
- ROLLUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, about GROUP BY clause
- dividing tables into groups
- GROUPING SETS
- GROUP BY clause
- table groups [SQL Server]
- groups [SQL Server], tables divided into groups
- summary values [SQL Server]
ms.assetid: 40075914-6385-4692-b4a5-62fe44ae6cb6
author: shkale-msft
ms.author: shkale
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d1081121051d3e033bbeec724fa3f5bb6ab4ba58
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751554"
---
# <a name="select---group-by--transact-sql"></a>SELECT — GROUP BY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Предложение инструкции SELECT, которое разделяет результат запроса на группы строк обычно с целью выполнения одного или нескольких статистических вычислений в каждой группе. Инструкция SELECT возвращает одну строку для каждой группы.  
  
## <a name="syntax"></a>Синтаксис  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database   
-- ISO-Compliant Syntax  
  
GROUP BY {
      column-expression  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
    | GROUPING SETS ( <grouping_set> [ ,...n ]  )  
    | () --calculates the grand total 
} [ ,...n ] 
 
<group_by_expression> ::=  
      column-expression  
    | ( column-expression [ ,...n ] )    
   
<grouping_set> ::=  
      () --calculates the grand total  
    | <grouping_set_item>  
    | ( <grouping_set_item> [ ,...n ] )  
  
<grouping_set_item> ::=  
      <group_by_expression>  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
  

-- For backward compatibility only.
-- Non-ISO-Compliant Syntax for SQL Server and Azure SQL Database 
  
GROUP BY 
      [ ALL ] column-expression [ ,...n ] 
    | column-expression [ ,...n ] [ WITH { CUBE | ROLLUP } ]   

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics 
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
    | ROLLUP ( <group_by_expression> [ ,...n ] ) 
} [ ,...n ]

```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
} [ ,...n ]

```  
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 
### <a name="column-expression"></a>*column-expression*  
Указывает на столбец или на нестатистическое вычисление в столбце. Этот столбец может принадлежать таблице, производной таблице или представлению. Столбец должен быть указан в предложении FROM инструкции SELECT, но не обязательно должен присутствовать в списке SELECT. 

Допустимые выражения см. в разделе [expression](~/t-sql/language-elements/expressions-transact-sql.md).    

Столбец должен быть указан в предложении FROM инструкции SELECT, но не обязательно должен присутствовать в списке SELECT. Каждый столбец таблицы или представления в любом нестатистическом выражении в списке \<select> должен быть включен в список GROUP BY.  
  
Следующие инструкции являются допустимыми.  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + ColumnB + constant FROM T GROUP BY ColumnA, ColumnB;  
```  
  
Следующие инструкции не являются допустимыми.  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + constant + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
```  

Выражение столбца не может содержать:

- псевдоним столбца, определенный в списке SELECT. Выражение может использовать псевдоним столбца для производной таблицы, определенной в предложении FROM.
- столбец типа **text**, **ntext**, или **image**. Однако столбец типа text, ntext или image можно использовать как аргумент для функции, возвращающей значение допустимого типа данных. Например, выражение может использовать SUBSTRING() и CAST(). Это также относится к выражениям в предложении HAVING.
- методы типа данных xml. Сюда может входить определяемой пользователем функции, которая использует методы типа данных xml. Сюда может входить вычисляемый столбец, который использует методы типа данных xml. 
- вложенный запрос. Возвращается ошибка 144. 
- столбец из индексированного представления. 
 
### <a name="group-by-column-expression--n-"></a>GROUP BY *column-expression* [ ,...n ] 

Группирует результаты инструкции SELECT в соответствии со значениями в списке одного или нескольких выражений столбцов. 

Например, этот запрос создает таблицу Sales со столбцами Country, Region и Sales. Он вставляет четыре строки, и две строки имеют совпадающие значения для столбцов Country и Region.  

```sql
CREATE TABLE Sales ( Country VARCHAR(50), Region VARCHAR(50), Sales INT );

INSERT INTO sales VALUES (N'Canada', N'Alberta', 100);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 200);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 300);
INSERT INTO sales VALUES (N'United States', N'Montana', 100);
```
В таблице Sales содержатся указанные далее строки.

| Country | Регион | Sales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 200 |
| Canada | British Columbia | 300 |
| США | Montana | 100 |

Этот запрос группирует значения столбцов Country и Region и возвращает общую сумму по каждому сочетанию значений.  
 
```sql 
SELECT Country, Region, SUM(sales) AS TotalSales
FROM Sales
GROUP BY Country, Region;
```
Результат запроса содержит 3 строки, так как существует 3 сочетания значений для Country и Region. Значение TotalSales для Canada и British Columbia является суммой двух строк. 

| Country | Регион | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| США | Montana | 100 |

### <a name="group-by-rollup"></a>GROUP BY ROLLUP

Создает группу для каждого сочетания выражений столбцов. Кроме того, выполняет сведение результатов в промежуточные и общие итоги. Для этого запрос перемещается справа налево, уменьшая количество выражений столбцов, по которым он создает группы и агрегаты. 

Порядок столбцов влияет на выходные данные ROLLUP и может отразиться на количестве строк в результирующем наборе.  

Например, `GROUP BY ROLLUP (col1, col2, col3, col4)` создает группы для каждой комбинации выражений столбцов в следующих списках.  

- col1, col2, col3, col4 
- col1, col2, col3, NULL
- col1, col2, NULL, NULL
- col1, NULL, NULL, NULL
- NULL, NULL, NULL, NULL — это общий итог.

Принимая во внимание таблицу из предыдущего примера, этот код выполняет операцию GROUP BY ROLLUP вместо простого предложения GROUP BY.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region);
```

Результатом запроса являются те же статистические вычисления, что и в простом предложении GROUP BY без ROLLUP. Кроме того, здесь создаются промежуточные итоги для каждого значения в столбце Country. Наконец, выводится общий итог для всех строк. Результат имеет следующий вид:

| Country | Регион | TotalSales |
| :------ | :----- | ---------: |
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| Canada | NULL | 600 |
| США | Montana | 100 |
| США | NULL | 100 |
| NULL | NULL | 700 |

### <a name="group-by-cube--"></a>GROUP BY CUBE ( )  

GROUP BY CUBE создает группы для всех возможных сочетаний столбцов. Для GROUP BY CUBE (a, b) результатами являются группы для уникальных значений (a, b) (NULL, b), (a, NULL) и (NULL, NULL).

Принимая во внимание таблицу из предыдущего примера, этот код выполняет операцию GROUP BY CUBE по столбцам Country и Region. 

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

Результатом запроса являются группы для уникальных значений (Country, Region), (NULL, Region), (Country, NULL) и (NULL, NULL). Результат выглядит следующим образом:

| Country | Регион | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| NULL | Alberta | 100 |
| Canada | British Columbia | 500 |
| NULL | British Columbia | 500 |
| США | Montana | 100 |
| NULL | Montana | 100 |
| NULL | NULL | 700
| Canada | NULL | 600 |
| США | NULL | 100 |
   
 ### <a name="group-by-grouping-sets--"></a>GROUP BY GROUPING SETS ( )  
 
Параметр GROUPING SETS позволяет объединять несколько предложений GROUP BY в одно предложение GROUP BY. Результаты эквивалентны тем, что формируются с применением конструкции UNION ALL к указанным группам. 

Например, `GROUP BY ROLLUP (Country, Region)` и `GROUP BY GROUPING SETS ( ROLLUP (Country, Region) )` возвращают одинаковые результаты. 

Если параметр GROUPING SETS имеет два или более элементов, результатом будет объединение элементов. Этот пример возвращает объединение результатов ROLLUP и CUBE для Country и Region.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( ROLLUP (Country, Region), CUBE (Country, Region) );
```

Результаты будут такими же, так как этот запрос возвращает объединение двух инструкций GROUP BY.

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region)
UNION ALL
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

SQL не консолидирует повторяющиеся группы, созданные для списка GROUPING SETS. Например, в `GROUP BY ( (), CUBE (Country, Region) )` оба элемента возвращают строку для общего итога, и в списке результатов будут указаны обе строки. 

 ### <a name="group-by-"></a>GROUP BY ()  
Указывает пустую группу, что приводит к созданию общего итога. Он полезен в качестве одного из элементов GROUPING SET. Например, эта инструкция выводит общий объем продаж для каждой страны, а затем — общий итог по всем странам.

```sql
SELECT Country, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( Country, () );
```

### <a name="group-by--all--column-expression--n-"></a>GROUP BY [ ALL ] column-expression [ ,...n ] 

Область применения: SQL Server и база данных SQL Azure

Примечание. Этот синтаксис поддерживается только для обратной совместимости. В будущей версии он будет удален. Избегайте использования этого синтаксиса в новых разработках и учитывайте необходимость изменения в будущем приложений, использующих этот синтаксис сейчас.

Указывает включить все группы в результаты независимо от того, соответствуют ли они условиям поиска в предложении WHERE. Группы, которые не соответствуют условиям поиска, имеют значение NULL для статистического вычисления. 

GROUP BY ALL
- Не поддерживается в запросах с доступом к удаленным таблицам, если в запросе присутствует также предложение WHERE.
- Применение предложения к столбцам, имеющим атрибут FILESTREAM, приведет к ошибке.
  
### <a name="with-distributed_agg"></a>WITH (DISTRIBUTED_AGG)
Область применения: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Указание запроса DISTRIBUTED_AGG заставляет систему MPP перераспределять таблицу по определенному столбцу до выполнения статистического вычисления. Только один столбец в предложении GROUP BY может иметь указание запроса DISTRIBUTED_AGG. После завершения запроса перераспределенная таблица удаляется. Исходная таблица не изменяется.  

Примечание. Указание запроса DISTRIBUTED_AGG предоставляется для обеспечения обратной совместимости с более ранними версиями [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] и не улучшает производительность большинства запросов. По умолчанию MPP уже перераспределяет данные для улучшения производительности для статистических вычислений. 
  
## <a name="general-remarks"></a>Общие замечания

### <a name="how-group-by-interacts-with-the-select-statement"></a>Взаимодействие GROUP BY с инструкцией SELECT
Список SELECT:
- Векторные агрегаты. Если в список SELECT включены агрегатные функции, инструкция GROUP BY вычисляет сводные значения для каждой группы. Они известны как векторные статистические вычисления. 
- Статистические функции DISTINCT. С конструкциями ROLLUP, CUBE и GROUPING SETS поддерживаются статистические функции AVG (DISTINCT *column_name*), COUNT (DISTINCT *column_name*) и SUM (DISTINCT *column_name*).
  
Предложение WHERE:
- SQL удаляет строки, которые не соответствуют условиям в предложении WHERE, до выполнения любых операций группирования.  
  
Предложение HAVING:
- SQL использует предложение HAVING для фильтрации групп в результирующем наборе. 
  
Предложение ORDER BY:
- Чтобы упорядочить результирующий набор, необходимо использовать предложение ORDER BY. Применение предложения GROUP BY не упорядочивает результирующий набор. 
  
Значения NULL:
- Если столбец группирования содержит значения NULL, они рассматриваются как равные и помещаются в одну группу.   
  
## <a name="limitations-and-restrictions"></a>Ограничения

Область применения: SQL Server (начиная с версии 2008) и [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

### <a name="maximum-capacity"></a>Максимальная емкость

Для предложения GROUP BY, использующего ROLLUP, CUBE или GROUPING SETS, используется максимум 32 выражения. Максимальное количество групп — 4096 (2<sup>12</sup>). Следующие примеры завершаются ошибкой, поскольку предложение GROUP BY имеет больше 4096 групп.  
 
-   В следующем примере формируется 4097 (2<sup>12</sup> + 1) группирующих наборов, поэтому пример завершится ошибкой.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), b )  
    ```  
  
-   В следующем примере формируется 4097 (2<sup>12</sup> + 1) групп, поэтому пример завершится ошибкой. И функция `CUBE ()`, и группирующий набор `()` формируют строку общего итога, а повторяющиеся группирующие наборы не устраняются.  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), ())  
    ```  

-   В этом примере используется синтаксис обратной совместимости. В примере создается 8192 (2<sup>13</sup>) группирующих наборов, поэтому он завершится ошибкой.  
  
    ```sql
    GROUP BY CUBE (a1, ..., a13)   
    GROUP BY a1, ..., a13 WITH CUBE   
    ```    
    Для предложений GROUP BY с поддержкой обратной совместимости и не содержащих операторов CUBE или ROLLUP количество элементов GROUP BY ограничивается размером столбцов GROUP BY, статистически обрабатываемых столбцов и статистических значений, включенных в запрос. Это объясняется ограничением размера промежуточной рабочей таблицы (8060 байт), необходимой для хранения промежуточных результатов запроса. При указании CUBE или ROLLUP максимально разрешенное количество выражений группирования равно 12.

### <a name="support-for-iso-and-ansi-sql-2006-group-by-features"></a>Поддержка функций предложения GROUP BY, совместимых с ISO и ANSI SQL-2006

Предложение GROUP BY поддерживает все возможности предложения GROUP BY, включенные в стандарт SQL-2006, со следующими синтаксическими исключениями.  
  
-   Применение группирующих наборов в предложении GROUP BY недопустимо, если они не составляют часть явного списка GROUPING SETS. Например, предложение `GROUP BY Column1, (Column2, ...ColumnN`) допускается в этом стандарте, но не в Transact-SQL.  Transact-SQL поддерживает `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))` и `GROUP BY Column1, Column2, ... ColumnN`, которые семантически эквивалентны. Эти инструкции семантически эквивалентны предыдущему примеру предложения `GROUP BY`. Это позволяет избежать возможности неправильной интерпретации предложения `GROUP BY Column1, (Column2, ...ColumnN`) как `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))`, не являющихся семантически эквивалентными.  
  
-   Применение группирующих наборов внутри группирующих наборов не допускается. Например, предложение `GROUP BY GROUPING SETS (A1, A2,...An, GROUPING SETS (C1, C2, ...Cn))` допускается в стандарте SQL-2006, но не в Transact-SQL. Transact-SQL поддерживает `GROUP BY GROUPING SETS( A1, A2,...An, C1, C2, ...Cn )` и `GROUP BY GROUPING SETS( (A1), (A2), ... (An), (C1), (C2), ... (Cn) )`, которые семантически эквивалентны первому примеру предложения GROUP BY и имеют более понятный синтаксис.  
  
-   GROUP BY [ALL/DISTINCT] используется только в простом предложении GROUP BY, которое содержит выражения столбцов. Это предложение не может использоваться с конструкциями GROUPING SETS, ROLLUP, CUBE, WITH CUBE или WITH ROLLUP. Ключевое слово ALL применяется по умолчанию и задано неявно. Оно допускается только в синтаксисе обратной совместимости.
  
### <a name="comparison-of-supported-group-by-features"></a>Сравнение поддерживаемых функций предложения GROUP BY  
 В следующей таблице описаны поддерживаемые возможности предложения GROUP BY с учетом версии SQL и уровня совместимости базы данных.  
  
|Компонент|SQL Server Integration Services|Уровень совместимости SQL Server — 100 или более|Уровень совместимости SQL Server 2008 или более поздней версии — 90.|  
|-------------|-------------------------------------|--------------------------------------------------|-----------------------------------------------------------|  
|Статистические функции DISTINCT|Не поддерживаются для конструкций WITH CUBE или WITH ROLLUP.|Поддерживаются для конструкций WITH CUBE, WITH ROLLUP, GROUPING SETS, CUBE или ROLLUP.|Аналогично уровню совместимости 100.|  
|Определяемая пользователем функция с именем CUBE или ROLLUP в предложении GROUP BY|Определяемая пользователем функция **dbo.cube(** _arg1_ **,** _...argN_ **)** or **dbo.rollup(** _arg1_ **,** ..._argN_ **)** в предложении GROUP BY недопустима.<br /><br /> Например: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|Определяемая пользователем функция **dbo.cube (** _arg1_ **,** ...argN **)** or **dbo.rollup(** arg1 **,** _...argN_ **)** в предложении GROUP BY недопустима.<br /><br /> Например: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`<br /><br /> Выдается следующее сообщение об ошибке: "Применение неверного синтаксиса недалеко от ключевого слова 'cube'&#124;'rollup'".<br /><br /> Чтобы избежать этой проблемы, замените конструкцию `dbo.cube` на `[dbo].[cube]` или конструкцию `dbo.rollup` на `[dbo].[rollup]`.<br /><br /> Следующий пример является допустимым: `SELECT SUM (x) FROM T  GROUP BY [dbo].[cube](y);`.|Определяемая пользователем функция **dbo.cube (** _arg1_ **,** _...argN_) or **dbo.rollup(** _arg1_ **,** _...argN_ **)** в предложении GROUP BY недопустима.<br /><br /> Например: `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|  
|GROUPING SETS|Не поддерживается|Поддерживается|Поддерживается|  
|CUBE|Не поддерживается|Поддерживается|Не поддерживается|  
|ROLLUP|Не поддерживается|Поддерживается|Не поддерживается|  
|Общий итог, такой как GROUP BY ()|Не поддерживается|Поддерживается|Поддерживается|  
|Функция GROUPING_ID|Не поддерживается|Поддерживается|Поддерживается|  
|Функция GROUPING|Поддерживается|Поддерживается|Поддерживается|  
|WITH CUBE|Поддерживается|Поддерживается|Поддерживается|  
|WITH ROLLUP|Поддерживается|Поддерживается|Поддерживается|  
|Удаление "повторяющегося" группирования с помощью конструкции WITH CUBE или WITH ROLLUP|Поддерживается|Поддерживается|Поддерживается| 
 
  
## <a name="examples"></a>Примеры  
  
### <a name="a-use-a-simple-group-by-clause"></a>A. Использование простого предложения GROUP BY  
 В следующем примере извлекается сумма для каждого `SalesOrderID` из таблицы `SalesOrderDetail`. В примере используется база данных AdventureWorks.  
  
```sql  
SELECT SalesOrderID, SUM(LineTotal) AS SubTotal  
FROM Sales.SalesOrderDetail AS sod  
GROUP BY SalesOrderID  
ORDER BY SalesOrderID;  
```  
  
### <a name="b-use-a-group-by-clause-with-multiple-tables"></a>Б. Использование предложения GROUP BY с несколькими таблицами  
 В следующем примере из таблицы `Address`, соединенной с таблицей `EmployeeAddress`, получается количество работников для каждого города `City`. В примере используется база данных AdventureWorks. 
  
```sql  
SELECT a.City, COUNT(bea.AddressID) EmployeeCount  
FROM Person.BusinessEntityAddress AS bea   
    INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
GROUP BY a.City  
ORDER BY a.City;  
```  
  
### <a name="c-use-a-group-by-clause-with-an-expression"></a>В. Использование предложения GROUP BY в выражениях  
 В следующем примере показано, как извлечь данные об общем объеме продаж за каждый год с помощью функции `DATEPART`. Одно и то же выражение должно присутствовать как в списке `SELECT`, так и в предложении `GROUP BY`.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
### <a name="d-use-a-group-by-clause-with-a-having-clause"></a>Г. Использование предложения GROUP BY с предложением HAVING  
 В следующем примере используется предложение `HAVING`, чтобы указать, какая из групп, сформированная в предложении `GROUP BY`, должна быть включена в результирующий набор.  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
HAVING DATEPART(yyyy,OrderDate) >= N'2003'  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
## <a name="examples-azure-synapse-analytics-and-parallel-data-warehouse"></a>Примеры: Azure Synapse Analytics и Parallel Data Warehouse  
  
### <a name="e-basic-use-of-the-group-by-clause"></a>Д. Базовое использование предложения GROUP BY  
 В следующем примере вычисляется общий объем всех продаж за каждый день. Выводится только одна строка, содержащая общий объем продаж по каждому дню.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales FROM FactInternetSales  
GROUP BY OrderDateKey ORDER BY OrderDateKey;  
```  
  
### <a name="f-basic-use-of-the-distributed_agg-hint"></a>Е. Базовое использование указания DISTRIBUTED_AGG  
 В этом примере показано указание запроса DISTRIBUTED_AGG для принудительного перемещения в таблице по столбцу `CustomerKey` перед выполнением статистического вычисления.  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT CustomerKey, SUM(SalesAmount) AS sas  
FROM FactInternetSales  
GROUP BY CustomerKey WITH (DISTRIBUTED_AGG)  
ORDER BY CustomerKey DESC;  
```  
  
### <a name="g-syntax-variations-for-group-by"></a>Ж. Варианты синтаксиса для GROUP BY  
 Если в списке Select статистические вычисления, каждый столбец в списке Select должен быть включен в список GROUP BY. Вычисляемые столбцы в списке Select можно указать в списке GROUP BY (делать это необязательно). Ниже приведены примеры синтаксически правильных инструкций SELECT.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName FROM DimCustomer GROUP BY LastName, FirstName;  
SELECT NumberCarsOwned FROM DimCustomer GROUP BY YearlyIncome, NumberCarsOwned;  
SELECT (SalesAmount + TaxAmt + Freight) AS TotalCost FROM FactInternetSales GROUP BY SalesAmount, TaxAmt, Freight;  
SELECT SalesAmount, SalesAmount*1.10 SalesTax FROM FactInternetSales GROUP BY SalesAmount;  
SELECT SalesAmount FROM FactInternetSales GROUP BY SalesAmount, SalesAmount*1.10;  
```  
  
### <a name="h-using-a-group-by-with-multiple-group-by-expressions"></a>З. Использование GROUP BY с несколькими выражениями GROUP BY  
 В следующем примере группируются результаты с помощью нескольких критериев `GROUP BY`. Если в каждой группе `OrderDateKey` есть подгруппы, которые могут отличаться по `DueDateKey`, для результирующего набора будет определено новое группирование.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, DueDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales
GROUP BY OrderDateKey, DueDateKey   
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-a-group-by-clause-with-a-having-clause"></a>И. Использование предложения GROUP BY с предложением HAVING  
 В следующем примере используется предложение `HAVING`, чтобы указать, какие группы, сформированные в предложении `GROUP BY`, должны быть включены в результирующий набор. В результаты будут включены только группы с датами заказов в 2004 году или более поздними.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales  
GROUP BY OrderDateKey   
HAVING OrderDateKey > 20040000   
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>См. также:  
 [GROUPING_ID (Transact-SQL)](~/t-sql/functions/grouping-id-transact-sql.md)   
 [GROUPING (Transact-SQL)](~/t-sql/functions/grouping-transact-sql.md)   
 [SELECT (Transact-SQL)](~/t-sql/queries/select-transact-sql.md)   
 [Предложение SELECT (Transact-SQL)](~/t-sql/queries/select-clause-transact-sql.md)  
  
  





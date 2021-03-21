---
title: ROW_NUMBER (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для функции ROW_NUMBER. Эта функция нумерует выходные данные результирующего набора.
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROW_NUMBER
- ROW_NUMBER_TSQL
- ROW_NUMBER()_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ROW_NUMBER function
- row numbers [SQL Server]
- sequential row numbers [SQL Server]
ms.assetid: 82fa9016-77db-4b42-b4c8-df6095b81906
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 29cd02108b56799e0e23ebf0da26f33df93e28d7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735668"
---
# <a name="row_number-transact-sql"></a>ROW_NUMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Нумерует выходные данные результирующего набора. В частности, возвращает последовательный номер строки в секции результирующего набора, 1 соответствует первой строке в каждой из секций. 
  
Функции `ROW_NUMBER` и `RANK` похожи. `ROW_NUMBER` нумерует все строки по порядку (например, 1, 2, 3, 4, 5). `RANK` назначает одинаковое числовое значение строкам, претендующим на один ранг (например, 1, 2, 2, 4, 5).   
  
> [!NOTE]
> `ROW_NUMBER` — это временное значение, вычисляемое во время выполнения запроса. Сведения о хранении номеров в таблице см. в разделах [Свойство IDENTITY](../../t-sql/statements/create-table-transact-sql-identity-property.md) и [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). 
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
 
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ROW_NUMBER ( )   
    OVER ( [ PARTITION BY value_expression , ... [ n ] ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 PARTITION BY *value_expression*  
 Делит результирующий набор, полученный от предложения [FROM](../../t-sql/queries/from-transact-sql.md), на секции, к которым применяется функция ROW_NUMBER. *value_expression* определяет столбец, по которому секционируется результирующий набор. Если параметр `PARTITION BY` не указан, функция обрабатывает все строки результирующего набора запроса как одну группу. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 *order_by_clause*  
 Предложение `ORDER BY` определяет последовательность, в которой строкам назначаются уникальные номера с помощью функции `ROW_NUMBER` в пределах указанной секции. Оно должно указываться обязательно. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **bigint**  
  
## <a name="general-remarks"></a>Общие замечания  
 Нет гарантии того, что строки, возвращенные запросом, использующим `ROW_NUMBER()`, будут расставлены в одинаковом порядке после каждого выполнения, если не соблюдены указанные ниже условия.  
  
1.  Все значения в секционированном столбце являются уникальными.  
  
2.  Все значения в столбцах `ORDER BY` являются уникальными.  
  
3.  Сочетания значений из столбца секционирования и столбцов `ORDER BY` являются уникальными.  
  
 Функция `ROW_NUMBER()` не детерминирована. Дополнительные сведения см. в разделе [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-simple-examples"></a>A. Простые примеры 

Приведенный ниже запрос возвращает четыре системные таблицы в алфавитном порядке.

```sql
SELECT 
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5
ORDER BY name ASC;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|name    |recovery_model_desc |  
|-----------  |------------ |  
|master |ПРОСТОЙ |
|model |FULL |
|msdb |ПРОСТОЙ |
|tempdb |ПРОСТОЙ |

Чтобы добавить столбец с номерами строк перед каждой строкой, добавьте столбец с помощью функции `ROW_NUMBER`, в данном случае с именем `Row#`. Предложение `ORDER BY` необходимо переместить к предложению `OVER`.

```sql
SELECT 
  ROW_NUMBER() OVER(ORDER BY name ASC) AS Row#,
  name, recovery_model_desc
FROM sys.databases 
WHERE database_id < 5;
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|Номер строки |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |master |ПРОСТОЙ |
|2 |model |FULL |
|3 |msdb |ПРОСТОЙ |
|4 |tempdb |ПРОСТОЙ |

Добавление предложения `PARTITION BY` для столбца `recovery_model_desc` приведет к тому, что нумерация начнется заново при изменении значения `recovery_model_desc`. 
 
```sql
SELECT 
  ROW_NUMBER() OVER(PARTITION BY recovery_model_desc ORDER BY name ASC) 
    AS Row#,
  name, recovery_model_desc
FROM sys.databases WHERE database_id < 5;
```  

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
   
|Номер строки |name    |recovery_model_desc |  
|------- |-----------  |------------ |  
|1 |model |FULL |
|1 |master |ПРОСТОЙ |
|2 |msdb |ПРОСТОЙ |
|3 |tempdb |ПРОСТОЙ |


### <a name="b-returning-the-row-number-for-salespeople"></a>Б. Возврат номера строки для salespeople  
 В следующем примере показан расчет номера строки для salespeople в [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)], выполняемый на основе ранжирования продаж за текущий год.  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT ROW_NUMBER() OVER(ORDER BY SalesYTD DESC) AS Row,   
    FirstName, LastName, ROUND(SalesYTD,2,1) AS "Sales YTD"   
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Row FirstName    LastName               SalesYTD  
--- -----------  ---------------------- -----------------  
1   Linda        Mitchell               4251368.54  
2   Jae          Pak                    4116871.22  
3   Michael      Blythe                 3763178.17  
4   Jillian      Carson                 3189418.36  
5   Ranjit       Varkey Chudukatil      3121616.32  
6   José         Saraiva                2604540.71  
7   Shu          Ito                    2458535.61  
8   Tsvi         Reiter                 2315185.61  
9   Rachel       Valdez                 1827066.71  
10  Tete         Mensa-Annan            1576562.19  
11  David        Campbell               1573012.93  
12  Garrett      Vargas                 1453719.46  
13  Lynn         Tsoflias               1421810.92  
14  Pamela       Ansman-Wolfe           1352577.13  
```  
  
### <a name="c-returning-a-subset-of-rows"></a>В. Возврат подмножества строк  
 В следующем примере показан расчет номеров всех строк в таблице `SalesOrderHeader` в порядке `OrderDate` с последующим возвращением строк с номерами от `50` до `60` включительно.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH OrderedOrders AS  
(  
    SELECT SalesOrderID, OrderDate,  
    ROW_NUMBER() OVER (ORDER BY OrderDate) AS RowNumber  
    FROM Sales.SalesOrderHeader   
)   
SELECT SalesOrderID, OrderDate, RowNumber    
FROM OrderedOrders   
WHERE RowNumber BETWEEN 50 AND 60;  
```  
  
### <a name="d-using-row_number-with-partition"></a>Г. Использование ROW_NUMBER() с PARTITION  
 В следующем примере аргумент `PARTITION BY` используется для секционирования результирующего набора запроса по столбцу `TerritoryName`. Предложение `ORDER BY`, указанное в предложении `OVER`, упорядочивает строки каждой секции по столбцу `SalesYTD`. Предложение `ORDER BY` в инструкции `SELECT` упорядочивает полный результирующий набор запроса по `TerritoryName`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT FirstName, LastName, TerritoryName, ROUND(SalesYTD,2,1) AS SalesYTD,  
ROW_NUMBER() OVER(PARTITION BY TerritoryName ORDER BY SalesYTD DESC) 
  AS Row  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL AND SalesYTD <> 0  
ORDER BY TerritoryName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
FirstName  LastName             TerritoryName        SalesYTD      Row  
---------  -------------------- ------------------   ------------  ---  
Lynn       Tsoflias             Australia            1421810.92    1  
José       Saraiva              Canada               2604540.71    1  
Garrett    Vargas               Canada               1453719.46    2  
Jillian    Carson               Central              3189418.36    1  
Ranjit     Varkey Chudukatil    France               3121616.32    1  
Rachel     Valdez               Germany              1827066.71    1  
Michael    Blythe               Northeast            3763178.17    1  
Tete       Mensa-Annan          Northwest            1576562.19    1  
David      Campbell             Northwest            1573012.93    2  
Pamela     Ansman-Wolfe         Northwest            1352577.13    3  
Tsvi       Reiter               Southeast            2315185.61    1  
Linda      Mitchell             Southwest            4251368.54    1  
Shu        Ito                  Southwest            2458535.61    2  
Jae        Pak                  United Kingdom       4116871.22    1  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-returning-the-row-number-for-salespeople"></a>Д. Возврат номера строки для salespeople  
 В приведенном ниже примере возвращается `ROW_NUMBER` для торговых представителей в зависимости от установленной для них квоты продаж.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) 
    AS RowNumber,  
    FirstName, LastName,   
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 Здесь приводится частичный результирующий набор.  

```  

RowNumber  FirstName  LastName            SalesQuota  
---------  ---------  ------------------  -------------  
1          Jillian    Carson              12,198,000.00  
2          Linda      Mitchell            11,786,000.00  
3          Michael    Blythe              11,162,000.00  
4          Jae        Pak                 10,514,000.00  
```

### <a name="f-using-row_number-with-partition"></a>Е. Использование ROW_NUMBER() с PARTITION  
 Следующий пример демонстрирует использование функции `ROW_NUMBER` с аргументом `PARTITION BY`. В результате функция `ROW_NUMBER` нумерует строки в каждой секции.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(PARTITION BY SalesTerritoryKey 
        ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    LastName, SalesTerritoryKey AS Territory,  
    CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName, SalesTerritoryKey;  
```  
  
 Здесь приводится частичный результирующий набор.  
 
```  
 
RowNumber  LastName            Territory  SalesQuota  
---------  ------------------  ---------  -------------  
1          Campbell            1           4,025,000.00  
2          Ansman-Wolfe        1           3,551,000.00  
3          Mensa-Annan         1           2,275,000.00  
1          Blythe              2          11,162,000.00  
1          Carson              3          12,198,000.00  
1          Mitchell            4          11,786,000.00  
2          Ito                 4           7,804,000.00  
```
  
## <a name="see-also"></a>См. также:  
 [RANK (Transact-SQL)](../../t-sql/functions/rank-transact-sql.md)   
 [DENSE_RANK (Transact-SQL)](../../t-sql/functions/dense-rank-transact-sql.md)   
 [NTILE (Transact-SQL)](../../t-sql/functions/ntile-transact-sql.md)  
  
  



---
description: DENSE_RANK (Transact-SQL)
title: DENSE_RANK (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DENSE_RANK_TSQL
- DENSE_RANK
dev_langs:
- TSQL
helpviewer_keywords:
- row ranking [SQL Server]
- DENSE_RANK function
- tied rows [SQL Server]
- ranking rows
ms.assetid: 03871fc6-9592-4016-b0b2-ff543f132b20
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 555c01bb38d77632d64543f03f5647d671700f3b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752074"
---
# <a name="dense_rank-transact-sql"></a>DENSE_RANK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает ранг каждой строки в секции результирующего набора без промежутков в значениях ранжирования. Ранг определенной строки равен количеству различных значений рангов, предшествующих строке, увеличенному на единицу.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
DENSE_RANK ( ) OVER ( [ <partition_by_clause> ] < order_by_clause > )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 \<partition_by_clause>  
Делит результирующий набор, полученный с помощью предложения [FROM](../../t-sql/queries/from-transact-sql.md), на секции, к которым затем применяется функция `DENSE_RANK`. Синтаксис `PARTITION BY` см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
 \<order_by_clause>  
Определяет порядок, в котором функция `DENSE_RANK` применяется к строкам в секции.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **bigint**  
  
## <a name="remarks"></a>Remarks  
Если две строки или несколько в одной секции имеют одинаковые значения ранга, каждой такой строке присваивается один и тот же ранг. Например, если двум лучшим менеджерам по продажам соответствует одинаковое значение SalesYTD, им обоим присваивается значение ранга 1. Менеджеру по продажам со следующим по величине значением SalesYTD присваивается значение ранга 2. Это значение превышает количество отдельных строк, предшествующих данной строке, на единицу. Таким образом, между номерами, возвращаемыми функцией `DENSE_RANK`, нет промежутков, и они всегда имеют последовательные значения ранга.  
  
Порядок сортировки, используемый для всего запроса, определяет порядок строк в результирующем наборе. Из этого следует, что строка с рангом 1 не всегда является первой строкой в секции.  
  
Функция `DENSE_RANK` не детерминирована. Дополнительные сведения см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-ranking-rows-within-a-partition"></a>A. Ранжирование строк внутри секции  
В приведенном ниже примере продукты ранжируются по количеству в указанных местоположениях в описи. Функция `DENSE_RANK` секционирует результирующий набор по `LocationID` и логически сортирует его по `Quantity`. Обратите внимание, что количество продуктов 494 и 495 совпадает. Так как они имеют одинаковое значение количества, им обоим присваивается значение ранга 1.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT i.ProductID, p.Name, i.LocationID, i.Quantity  
    ,DENSE_RANK() OVER   
    (PARTITION BY i.LocationID ORDER BY i.Quantity DESC) AS Rank  
FROM Production.ProductInventory AS i   
INNER JOIN Production.Product AS p   
    ON i.ProductID = p.ProductID  
WHERE i.LocationID BETWEEN 3 AND 4  
ORDER BY i.LocationID;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ProductID   Name                               LocationID Quantity Rank  
----------- ---------------------------------- ---------- -------- -----  
494         Paint - Silver                     3          49       1  
495         Paint - Blue                       3          49       1  
493         Paint - Red                        3          41       2  
496         Paint - Yellow                     3          30       3  
492         Paint - Black                      3          17       4  
495         Paint - Blue                       4          35       1  
496         Paint - Yellow                     4          25       2  
493         Paint - Red                        4          24       3  
492         Paint - Black                      4          14       4  
494         Paint - Silver                     4          12       5  
  
(10 row(s) affected)  
  
```  
  
### <a name="b-ranking-all-rows-in-a-result-set"></a>Б. Ранжирование всех строк в результирующем наборе  
В приведенном ниже примере возвращается список первых десяти сотрудников, ранжированных по окладу. Так как в инструкции `SELECT` предложение `PARTITION BY` не указывалось, функция `DENSE_RANK` применялась ко всем строкам результирующего набора.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) BusinessEntityID, Rate,   
       DENSE_RANK() OVER (ORDER BY Rate DESC) AS RankBySalary  
FROM HumanResources.EmployeePayHistory;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID Rate                  RankBySalary  
---------------- --------------------- --------------------  
1                125.50                1  
25               84.1346               2  
273              72.1154               3  
2                63.4615               4  
234              60.0962               5  
263              50.4808               6  
7                50.4808               6  
234              48.5577               7  
285              48.101                8  
274              48.101                8  
```  
  
## <a name="c-four-ranking-functions-used-in-the-same-query"></a>В. Использование четырех ранжирующих функций в одном запросе  
В этом примере демонстрируются четыре функции ранжирования:

+ [DENSE_RANK()](./dense-rank-transact-sql.md)
+ [NTILE()](./ntile-transact-sql.md)
+ [RANK()](./rank-transact-sql.md)
+ [ROW_NUMBER()](./row-number-transact-sql.md)

Они используются в одном запросе. См. конкретные примеры по каждой ранжирующей функции.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS Rank  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS Quartile  
    ,s.SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|FirstName|LastName|Row Number|Rank|Dense Rank|Quartile|SalesYTD|PostalCode|  
|---------------|--------------|----------------|----------|----------------|--------------|--------------|----------------|  
|Michael|Blythe|1|1|1|1|4557045,0459|98027|  
|Linda|Mitchell|2|1|1|1|5200475,2313|98027|  
|Jillian|Carson|3|1|1|1|3857163,6332|98027|  
|Garrett|Vargas|4|1|1|1|1764938,9859|98027|  
|Tsvi|Reiter|5|1|1|2|2811012,7151|98027|  
|Shu|Ito|6|6|2|2|3018725,4858|98055|  
|Josй|Saraiva|7|6|2|2|3189356,2465|98055|  
|David|Campbell|8|6|2|3|3587378,4257|98055|  
|Tete|Mensa-Annan|9|6|2|3|1931620,1835|98055|  
|Lynn|Tsoflias|10|6|2|3|1758385,926|98055|  
|Rachel|Valdez|11|6|2|4|2241204,0424|98055|  
|Jae|Pak|12|6|2|4|5015682,3752|98055|  
|Ranjit|Varkey Chudukatil|13|6|2|4|3827950,238|98055| 


## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-ranking-rows-within-a-partition"></a>Г. Ранжирование строк внутри секции  
В приведенном ниже примере торговые представители на каждой территории продаж ранжируются в соответствии с общим объемом продаж. Функция `DENSE_RANK` секционирует набор строк по `SalesTerritoryGroup` и логически сортирует результирующий набор по `SalesAmountQuota`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryGroup,  
    DENSE_RANK() OVER (PARTITION BY SalesTerritoryGroup ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryGroup != N'NA'  
GROUP BY LastName, SalesTerritoryGroup;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
 LastName          TotalSales     SalesTerritoryGroup  RankResult  
----------------  -------------  -------------------  --------  
Pak               10514000.0000  Europe               1  
Varkey Chudukatil  5557000.0000  Europe               2  
Valdez             2287000.0000  Europe               3  
Carson            12198000.0000  North America        1  
Mitchell          11786000.0000  North America        2  
Blythe            11162000.0000  North America        3  
Reiter             8541000.0000  North America        4  
Ito                7804000.0000  North America        5  
Saraiva            7098000.0000  North America        6  
Vargas             4365000.0000  North America        7  
Campbell           4025000.0000  North America        8  
Ansman-Wolfe       3551000.0000  North America        9  
Mensa-Annan        2753000.0000  North America        10  
Tsoflias           1687000.0000  Pacific              1 
```  

## <a name="see-also"></a>См. также:  
 [RANK (Transact-SQL)](../../t-sql/functions/rank-transact-sql.md)   
 [ROW_NUMBER (Transact-SQL)](../../t-sql/functions/row-number-transact-sql.md)   
 [NTILE (Transact-SQL)](../../t-sql/functions/ntile-transact-sql.md)   
 [Ранжирующие функции (Transact-SQL)](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [Функции](../../t-sql/functions/functions.md)  
  
  


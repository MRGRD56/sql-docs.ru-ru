---
description: RANK (Transact-SQL)
title: RANK (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RANK
- RANK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row ranking [SQL Server]
- tied rows [SQL Server]
- ranking rows
- RANK function [Transact-SQL]
ms.assetid: 2d96f6d2-5db7-4b3c-a63e-213c58e4af55
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cc59c58ab6e6b587724a7570f8d9c68579dd8389
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750644"
---
# <a name="rank-transact-sql"></a>RANK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает ранг каждой строки в секции результирующего набора. Ранг строки вычисляется как единица плюс количество рангов, находящихся до этой строки.  

Функции ROW_NUMBER и RANK похожи. ROW_NUMBER нумерует все строки по порядку (например, 1, 2, 3, 4, 5). RANK назначает одинаковое числовое значение строкам, претендующим на один ранг (например, 1, 2, 2, 4, 5).   
  
> [!NOTE]
> RANK — это временное значение, вычисляемое во время выполнения запроса. Сведения о хранении номеров в таблице см. в разделах [Свойство IDENTITY](../../t-sql/statements/create-table-transact-sql-identity-property.md) и [SEQUENCE](../../t-sql/statements/create-sequence-transact-sql.md). 
   
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
RANK ( ) OVER ( [ partition_by_clause ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* делит результирующий набор, полученный с помощью предложения FROM, на секции, к которым применяется функция. Если этот параметр не указан, функция обрабатывает все строки результирующего набора запроса как отдельные группы. _order\_by\_clause_ определяет порядок данных перед применением функции. Аргумент *order_by_clause* является обязательным. В функции RANK невозможно указать \<rows or range clause/> предложения OVER. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **bigint**  
  
## <a name="remarks"></a>Remarks  
 Если на один ранг претендуют две и более строки, все они получат одинаковый ранг. Например, если двум лучшим продавцам соответствует одинаковое значение SalesYTD, им обоим присваивается ранг 1. Менеджер по продажам со следующим по величине значением SalesYTD получит ранг номер три, так как перед ним находятся две строки с более высоким рангом. Поэтому функция RANK не всегда возвращает последовательные целые числа.  
  
 Порядок сортировки, используемый для всего запроса, определяет порядок, в котором строки будут появляться в результирующем наборе.  
  
 Функция RANK не детерминирована. Дополнительные сведения см. в разделе [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-ranking-rows-within-a-partition"></a>A. Ранжирование строк внутри секции  
 Следующий пример ранжирует продукты по количеству в указанных местоположениях в описи. Результирующий набор секционируется по `LocationID` и логически сортируется по `Quantity`. Обратите внимание, что количество продуктов 494 и 495 совпадает. Так как они занимают одно и то же место, рейтинг обоих продуктов равен единице.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT i.ProductID, p.Name, i.LocationID, i.Quantity  
    ,RANK() OVER   
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
ProductID   Name                   LocationID   Quantity Rank  
----------- ---------------------- ------------ -------- ----  
494         Paint - Silver         3            49       1  
495         Paint - Blue           3            49       1  
493         Paint - Red            3            41       3  
496         Paint - Yellow         3            30       4  
492         Paint - Black          3            17       5  
495         Paint - Blue           4            35       1  
496         Paint - Yellow         4            25       2  
493         Paint - Red            4            24       3  
492         Paint - Black          4            14       4  
494         Paint - Silver         4            12       5  
 (10 row(s) affected)  
```  
  
### <a name="b-ranking-all-rows-in-a-result-set"></a>Б. Ранжирование всех строк в результирующем наборе  
 Следующий пример возвращает список первых десяти сотрудников, отранжированных по их окладу. Поскольку предложение PARTITION BY не указывалось, функция RANK применялась ко всем строкам результирующего набора.  
  
```sql  
USE AdventureWorks2012  
SELECT TOP(10) BusinessEntityID, Rate,   
       RANK() OVER (ORDER BY Rate DESC) AS RankBySalary  
FROM HumanResources.EmployeePayHistory AS eph1  
WHERE RateChangeDate = (SELECT MAX(RateChangeDate)   
                        FROM HumanResources.EmployeePayHistory AS eph2  
                        WHERE eph1.BusinessEntityID = eph2.BusinessEntityID)  
ORDER BY BusinessEntityID;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
BusinessEntityID Rate                  RankBySalary  
---------------- --------------------- --------------------  
1                125.50                1  
2                63.4615               4  
3                43.2692               8  
4                29.8462               19  
5                32.6923               16  
6                32.6923               16  
7                50.4808               6  
8                40.8654               10  
9                40.8654               10  
10               42.4808               9  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-ranking-rows-within-a-partition"></a>В. Ранжирование строк внутри секции  
 В приведенном ниже примере торговые представители на каждой территории продаж ранжируются в соответствии с общим объемом продаж. Набор строк секционируется по столбцу `SalesTerritoryGroup` и сортируется по столбцу `SalesAmountQuota`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryRegion,  
    RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
LastName          TotalSales     SalesTerritoryRegion  RankResult
----------------  -------------  -------------------  --------
Tsoflias          1687000.0000   Australia            1
Saraiva           7098000.0000   Canada               1
Vargas            4365000.0000   Canada               2
Carson            12198000.0000  Central              1
Varkey Chudukatil 5557000.0000   France               1
Valdez            2287000.0000   Germany              1
Blythe            11162000.0000  Northeast            1
Campbell          4025000.0000   Northwest            1
Ansman-Wolfe      3551000.0000   Northwest            2
Mensa-Annan       2753000.0000   Northwest            3
Reiter            8541000.0000   Southeast            1
Mitchell          11786000.0000  Southwest            1
Ito               7804000.0000   Southwest            2
Pak               10514000.0000  United Kingdom       1
```  
  
## <a name="see-also"></a>См. также:  
 [DENSE_RANK (Transact-SQL)](../../t-sql/functions/dense-rank-transact-sql.md)   
 [ROW_NUMBER (Transact-SQL)](../../t-sql/functions/row-number-transact-sql.md)   
 [NTILE (Transact-SQL)](../../t-sql/functions/ntile-transact-sql.md)   
 [Ранжирующие функции (Transact-SQL)](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)  
  
  




---
title: Использование операторов PIVOT и UNPIVOT | Документы Майкрософт
description: Справочник по Transact-SQL для реляционных операторов PIVOT и UNPIVOT. Эти операторы можно использовать в инструкциях SELECT для изменения выражения с табличным значением в другой таблице.
ms.date: 10/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PIVOT_TSQL
helpviewer_keywords:
- FROM clause, UNPIVOT operator
- unpivoting tables
- table pivoting [SQL Server]
- UNPIVOT operator
- crosstab query
- PIVOT operator
- rotating table-valued expressions
- pivoting tables
- FROM clause, PIVOT operator
- rotating columns
ms.assetid: 24ba54fc-98f7-4d35-8881-b5158aac1d66
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 26d7cc4a646f4dcb9b0c4abd716c389608f47836
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751574"
---
# <a name="from---using-pivot-and-unpivot"></a>FROM — использование PIVOT и UNPIVOT

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Реляционные операторы `PIVOT` и `UNPIVOT` можно использовать для изменения возвращающего табличное значение выражения в другой таблице. `PIVOT` поворачивает возвращающее табличное значение выражение, преобразуя уникальные значения одного столбца выражения в несколько выходных столбцов. В случае необходимости `PIVOT` также объединяет оставшиеся повторяющиеся значения столбца и отображает их в выходных данных. `UNPIVOT` выполняет действия, обратные PIVOT, преобразуя столбцы возвращающего табличное значение выражения в значения столбца.  
  
Синтаксис `PIVOT` является более простым и понятным, чем синтаксис, который может выполнить то же действие с помощью последовательности инструкций `SELECT...CASE`. Полное описание синтаксиса инструкции `PIVOT` см. в разделе [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
В следующем синтаксисе приводится использование оператора `PIVOT`.  
  
```syntaxsql  
SELECT <non-pivoted column>,  
    [first pivoted column] AS <column name>,  
    [second pivoted column] AS <column name>,  
    ...  
    [last pivoted column] AS <column name>  
FROM  
    (<SELECT query that produces the data>)   
    AS <alias for the source query>  
PIVOT  
(  
    <aggregation function>(<column being aggregated>)  
FOR   
[<column that contains the values that will become column headers>]   
    IN ( [first pivoted column], [second pivoted column],  
    ... [last pivoted column])  
) AS <alias for the pivot table>  
<optional ORDER BY clause>;  
```  

## <a name="remarks"></a>Remarks  
Идентификаторы столбцов в предложении `UNPIVOT` следуют параметрам сортировки каталога. Для [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] параметры сортировки всегда соответствуют `SQL_Latin1_General_CP1_CI_AS`. Для частично автономных баз данных [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] параметры сортировки всегда соответствуют `Latin1_General_100_CI_AS_KS_WS_SC`. Если столбец используется в сочетании с другими столбцами, для предотвращения конфликтов требуется предложение collate (`COLLATE DATABASE_DEFAULT`).  

  
## <a name="basic-pivot-example"></a>Базовый пример PIVOT  
В следующем примере кода создается таблица, включающая два столбца и четыре строки.  
  
```sql
USE AdventureWorks2014 ;  
GO  
SELECT DaysToManufacture, AVG(StandardCost) AS AverageCost   
FROM Production.Product  
GROUP BY DaysToManufacture;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
DaysToManufacture AverageCost
----------------- -----------
0                 5.0885
1                 223.88
2                 359.1082
4                 949.4105
```
  
Для значения `DaysToManufacture`, равного трем, продукты не определены.  
  
Следующий код отображает тот же самый результат, сведенный так, что значения `DaysToManufacture` становятся заголовками. Для значения трех `[3]` дней приводится столбец, даже если результат равен `NULL`.  
  
```sql
-- Pivot table with one row and five columns  
SELECT 'AverageCost' AS Cost_Sorted_By_Production_Days,   
[0], [1], [2], [3], [4]  
FROM  
(SELECT DaysToManufacture, StandardCost   
    FROM Production.Product) AS SourceTable  
PIVOT  
(  
AVG(StandardCost)  
FOR DaysToManufacture IN ([0], [1], [2], [3], [4])  
) AS PivotTable;  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Cost_Sorted_By_Production_Days 0           1           2           3           4         
------------------------------ ----------- ----------- ----------- ----------- -----------
AverageCost                    5.0885      223.88      359.1082    NULL        949.4105
```
  
## <a name="complex-pivot-example"></a>Сложный пример PIVOT  
Обычно оператор `PIVOT` может быть полезен при создании отчетов с перекрестными ссылками для предоставления сводки по данным. Например, пусть необходимо обратиться к таблице `PurchaseOrderHeader` образца базы данных `AdventureWorks2014` для определения количества заказов на покупку, размещенных некоторым сотрудником. Требуемые данные, отсортированные по поставщикам, можно извлечь при выполнении следующего запроса.  
  
```sql
USE AdventureWorks2014;  
GO  
SELECT VendorID, [250] AS Emp1, [251] AS Emp2, [256] AS Emp3, [257] AS Emp4, [260] AS Emp5  
FROM   
(SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM Purchasing.PurchaseOrderHeader) p  
PIVOT  
(  
COUNT (PurchaseOrderID)  
FOR EmployeeID IN  
( [250], [251], [256], [257], [260] )  
) AS pvt  
ORDER BY pvt.VendorID;  
```  
  
Здесь приводится частичный результирующий набор.  
  
```
VendorID    Emp1        Emp2        Emp3        Emp4        Emp5  
----------- ----------- ----------- ----------- ----------- -----------
1492        2           5           4           4           4
1494        2           5           4           5           4
1496        2           4           4           5           5
1498        2           5           4           4           4
1500        3           4           4           5           4
```
  
Данные, возвращаемые в результате выполнения указанного подзапроса выборки, сводятся в столбец `EmployeeID`.  
  
```sql
SELECT PurchaseOrderID, EmployeeID, VendorID  
FROM PurchaseOrderHeader;  
```  
  
Уникальные значения столбца `EmployeeID` становятся полями итогового результирующего набора. Таким образом, имеется столбец, соответствующий каждому идентификатору `EmployeeID`, указанному в предложении PIVOT: в данном случае это сотрудники `250`, `251`, `256`, `257` и `260`. `PurchaseOrderID` служит столбцом значений, по которому группируются столбцы, возвращаемые в конечный вывод и называемые столбцами группирования. В этом случае значения столбцов группирования обрабатываются с помощью функции `COUNT`. Обратите внимание, что при вычислении функции `COUNT` для каждого сотрудника появится предупреждающее сообщение о том, что значения NULL столбца `PurchaseOrderID` не учитываются.  
  
> [!IMPORTANT]  
>  При статистической обработке данных с использованием агрегатных функций, содержащих оператор `PIVOT`, пустые значения столбцов не учитываются.  

## <a name="unpivot-example"></a>Пример UNPIVOT
  
Оператор `UNPIVOT` выполняет действия, обратные оператору `PIVOT`, преобразуя столбцы данных в строки. Допустим, что таблица, созданная в ходе выполнения предыдущего примера, хранится в базе данных и имеет идентификатор `pvt`. Пусть необходимо преобразовать идентификаторы столбцов `Emp1`, `Emp2`, `Emp3`, `Emp4` и `Emp5` в строки данных, сгруппированные по поставщикам. Таким образом, необходимо определить два дополнительных столбца. Столбец, содержащий значения поворачиваемых столбцов (`Emp1`, `Emp2`...), будет называться `Employee`, а столбец, который будет содержать значения, в данный момент существующие в поворачиваемых столбцах, будет называться `Orders`. Указанные столбцы связаны соответственно с такими параметрами в определении на языке [!INCLUDE[tsql](../../includes/tsql-md.md)], как *pivot_column* и *value_column*. Вот запрос.  
  
```sql
-- Create the table and insert values as portrayed in the previous example.  
CREATE TABLE pvt (VendorID INT, Emp1 INT, Emp2 INT,  
    Emp3 INT, Emp4 INT, Emp5 INT);  
GO  
INSERT INTO pvt VALUES (1,4,3,5,4,4);  
INSERT INTO pvt VALUES (2,4,1,5,5,5);  
INSERT INTO pvt VALUES (3,4,3,5,4,4);  
INSERT INTO pvt VALUES (4,4,2,5,5,4);  
INSERT INTO pvt VALUES (5,5,1,5,5,5);  
GO  
-- Unpivot the table.  
SELECT VendorID, Employee, Orders  
FROM   
   (SELECT VendorID, Emp1, Emp2, Emp3, Emp4, Emp5  
   FROM pvt) p  
UNPIVOT  
   (Orders FOR Employee IN   
      (Emp1, Emp2, Emp3, Emp4, Emp5)  
)AS unpvt;  
GO  
```  
  
Здесь приводится частичный результирующий набор.  
  
```
VendorID    Employee    Orders
----------- ----------- ------
1            Emp1       4
1            Emp2       3 
1            Emp3       5
1            Emp4       4
1            Emp5       4
2            Emp1       4
2            Emp2       1
2            Emp3       5
2            Emp4       5
2            Emp5       5
...
```
  
Обратите внимание, что оператор `UNPIVOT` не является в точности обратным оператору `PIVOT`. Оператор `PIVOT` выполняет статистическую обработку и слияние нескольких строк в единую выходную строку. Оператор `UNPIVOT` не восстанавливает исходные возвращающие табличное значение выражения, так как строки были объединены. Кроме того, оператор `UNPIVOT` удаляет значения NULL из обрабатываемых им данных. Поэтому в случае наличия в исходных столбцах значений NULL данные на выходе могут отличаться от данных до их обработки с помощью оператора `PIVOT`.  
  
В представлении `Sales.vSalesPersonSalesByFiscalYears` образца базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] предложение `PIVOT` используется для определения полного объема продаж каждого менеджера в течение каждого финансового года. Чтобы создать скрипт для представления в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], найдите в **обозревателе объектов** это представление в папке **Представления** базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Щелкните правой кнопкой мыши имя представления и выберите **Создать скрипт для представления**.  
  
## <a name="see-also"></a>См. также:  
[FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
[CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  

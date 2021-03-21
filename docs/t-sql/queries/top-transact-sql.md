---
description: TOP (Transact-SQL)
title: TOP (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TOP_TSQL
- TOP
dev_langs:
- TSQL
helpviewer_keywords:
- TOP clause
- first set of query result rows [SQL Server]
- TOP clause, about TOP clause
- queries [SQL Server], results
ms.assetid: da983c0a-06c5-4cf8-a6a4-7f9d66f34f2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23f9bd7c9e0bddc53128b8ac6d8117a4cb609168
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748904"
---
# <a name="top-transact-sql"></a>TOP (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Ограничивает число строк, возвращаемых в результирующем наборе запроса до заданного числа или процентного значения в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Если вы используете TOP с предложением ORDER BY, в результирующий набор включаются только первые *N* строк отсортированного результата. В противном случае TOP возвращает *N* строк в неопределенном порядке. Это предложение позволяет ограничить число строк, возвращаемых инструкцией SELECT. Также TOP можно использовать для указания числа строк, затрагиваемых инструкциями INSERT, UPDATE, MERGE или DELETE.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
 
 Ниже приводится синтаксис для SQL Server и Базы данных SQL Azure:

```syntaxsql  
[   
    TOP (expression) [PERCENT]  
    [ WITH TIES ]  
]  
```  

Ниже приведен синтаксис для [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:

```syntaxsql  
[   
    TOP ( expression )   
    [ WITH TIES ]  
]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*expression*  
Числовое выражение, определяющее количество возвращаемых строк. Если значение *expression* указано в процентах, оно неявно преобразуется в формат **float**. В противном случае *expression* преобразуется в формат **bigint**.  
  
PERCENT  
Указывает на то, что запрос возвращает только первые *expression* процентов строк из результирующего набора. Дробные значения округляются до следующего целого числа.  
  
WITH TIES  
Возвращает две или более строки, которые соперничают за последнее место в ограниченном результирующем наборе. Этот аргумент можно использовать только с предложением **ORDER BY**. В режиме **WITH TIES** количество возвращаемых строк может превысить значение *expression*. Например, если *expression* имеет значение 5 и существуют еще 2 строки с такими же значениями столбцов, указанных в предложении **ORDER BY**, что и у пятой строки, результирующий набор будет содержать 7 строк.  
  
Предложение TOP можно сочетать с аргументом WITH TIES только в инструкциях SELECT, и только если вместе с ними задано предложение ORDER BY. Порядок возврата связанных записей произволен. ORDER BY не влияет на это правило.  
  
## <a name="best-practices"></a>Рекомендации  
В инструкции SELECT всегда указывайте ORDER BY вместе с предложением TOP. Дело в том, что это единственный предсказуемый способ отбора строк предложением TOP.  
  
Для реализации решения разбиения на страницы пользуйтесь предложениями OFFSET и FETCH в предложении ORDER BY, а не предложением TOP. Решение разбиения на страницы (т.е. постраничной выдачи данных клиенту) проще реализовать с помощью предложений OFFSET и FETCH. Дополнительные сведения см. в разделе [Предложение ORDER BY (Transact-SQL)](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
Для ограничения числа возвращаемых строк пользуйтесь TOP (или OFFSET и FETCH), а не SET ROWCOUNT. Эти методы предпочтительнее, чем SET ROWCOUNT, по следующим причинам:  
  
-   Являясь частью инструкции SELECT, оптимизатор запросов может принимать значение *expression* в предложениях TOP или FETCH во время оптимизации запроса. Так как SET ROWCOUNT используется вне инструкции, выполняющей запрос, значение не может быть учтено в плане запроса.  
  
## <a name="compatibility-support"></a>Поддержка совместимости  
В целях обратной совместимости скобки в инструкции SELECT необязательны, если выражение является целочисленной константой. Мы рекомендуем всегда использовать круглые скобки для аргумента TOP в инструкциях SELECT. Такой подход обеспечивает согласованность с синтаксисом инструкций INSERT, UPDATE, MERGE и DELETE. 
  
## <a name="interoperability"></a>Совместимость  
Выражение TOP не влияет на другие выражения, которые могут быть запущены триггером. Таблицы **inserted** и **deleted** в триггерах всегда возвращают только те строки, которые реально затронуты инструкциями INSERT, UPDATE, MERGE или DELETE. Например, если INSERT TRIGGER срабатывает в результате выполнения инструкции INSERT с предложением TOP,  
  
то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет обновлять строки через представления. Так как предложение TOP можно включать в определение представления, некоторые строки могут исчезнуть из представления после обновления, если они больше не соответствуют требованиям выражения TOP.  
  
При использовании в инструкции MERGE предложение TOP применяется *после* соединения всей исходной таблицы и всей целевой таблицы, а также удаления соединенных строк, которые не соответствуют требованиям к вставке, обновлению или удалению. Предложение TOP дополнительно сокращает количество соединенных строк до указанного значения, а затем к оставшимся соединенным строкам применяются операции вставки, обновления или удаления без определенного порядка. Это означает, что порядок распределения строк по действиям, определенным в предложениях WHEN отсутствует. Например если TOP (10) в запросе затрагивает 10 строк, в их число могут попасть 7 обновляемых и 3 добавляемые строки. Или же 1 строка будет удаляемая, 5 обновляемых и 4 добавляемые строки либо любой другой вариант. Инструкция MERGE выполняет полное сканирование исходной и целевой таблиц, поэтому предложение TOP для изменения большой таблицы путем создания нескольких пакетов может снизить производительность ввода-вывода. В этом случае необходимо сделать так, чтобы все последующие пакеты содержали только новые строки.  
  
Соблюдайте осторожность при использовании предложения TOP в запросе, содержащем операторы UNION, UNION ALL, EXCEPT и INTERSECT. Есть вероятность того, что результаты запроса будут непредсказуемыми из-за неожиданной логики обработки предложений TOP и ORDER BY в операции выбора. Например, исходя из следующих таблиц и данных, предположим, что необходимо вернуть самый недорогой красный и синий автомобили. Иными словами, красный седан и синий фургон.  
  
```sql  
CREATE TABLE dbo.Cars(Model VARCHAR(15), Price MONEY, Color VARCHAR(10));  
INSERT dbo.Cars VALUES  
    ('sedan', 10000, 'red'), ('convertible', 15000, 'blue'),   
    ('coupe', 20000, 'red'), ('van', 8000, 'blue');  
```  
  
Чтобы получить такие результаты, можно написать следующий запрос.  
  
```sql  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'red'  
UNION ALL  
SELECT TOP(1) Model, Color, Price  
FROM dbo.Cars  
WHERE Color = 'blue'  
ORDER BY Price ASC;  
GO    
```  
  
Ниже приведен результирующий набор.  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 convertible   blue       15000.00
 ```  
  
Возвращаются непредвиденные результаты, так как инструкция TOP логически выполняется раньше, чем предложение ORDER BY, которое сортирует результаты оператора (в данном случае UNION ALL). Таким образом, предыдущие запросы вернут произвольный красный и произвольный синий автомобили, а затем отсортируют результат такого объединения по цене. Следующий пример показывает, как правильно написать запрос, который позволит получить нужный результат.  
  
```sql  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'red'  
      ORDER BY Price ASC) AS a  
UNION ALL  
SELECT Model, Color, Price  
FROM (SELECT TOP(1) Model, Color, Price  
      FROM dbo.Cars  
      WHERE Color = 'blue'  
      ORDER BY Price ASC) AS b;  
GO    
```  
  
Использование TOP и ORDER BY во вложенной операции выбора гарантирует, что результаты предложения ORDER BY применяются к предложению TOP, а не к сортировке результата операции UNION.  
  
 Результирующий набор:  
  
 ```
 Model         Color      Price  
 ------------- ---------- -------  
 sedan         red        10000.00  
 van           blue        8000.00
 ```  
  
## <a name="limitations-and-restrictions"></a>Ограничения  
При использовании TOP в инструкции INSERT, UPDATE, MERGE или DELETE к строкам не применяется никакое упорядочение. Также вы не можете напрямую указать предложение ORDER BY в этих инструкциях. Если вы намерены использовать TOP, чтобы вставлять, удалять или изменять строки в определенном хронологическом порядке, добавьте TOP в предложение ORDER BY, включенное во вложенную инструкцию выборки. Подробнее см. раздел с примерами в этой статье.  
  
Предложение TOP не может быть использовано в инструкциях UPDATE и DELETE для секционированных представлений.  
  
TOP нельзя сочетать с OFFSET и FETCH в одном выражении запроса (в той же области запроса). Дополнительные сведения см. в разделе [Предложение ORDER BY (Transact-SQL)](../../t-sql/queries/select-order-by-clause-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
|Категория|Используемые элементы синтаксиса|  
|--------------|------------------------------|  
|[Основной синтаксис](#BasicSyntax)|TOP • PERCENT|  
|[Включить равные значения](#tie)|WITH TIES|  
|[Ограничение числа строк, обрабатываемых инструкциями DELETE, INSERT и UPDATE](#DML)|DELETE • INSERT • UPDATE|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Основной синтаксис  
В примерах этого раздела показана базовая функциональность предложения ORDER BY с использованием минимально необходимого синтаксиса.  
  
#### <a name="a-using-top-with-a-constant-value"></a>A. Использование ключевого слова TOP со значением константы  
В следующих примерах константа указывает число сотрудников, возвращаемых в результирующем наборе запроса. В первом примере возвращаются 10 произвольных строк, так как отсутствует предложение ORDER BY. Во втором примере предложение ORDER BY возвращает 10 сотрудников, последними принятых на работу.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Select the first 10 random employees.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee;  
GO  
-- Select the first 10 employees hired most recently.  
SELECT TOP(10)JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO  
```  
  
#### <a name="b-using-top-with-a-variable"></a>Б. Использование ключевого слова TOP с переменной  
В следующем примере переменная указывает число сотрудников, возвращаемых в результирующем наборе запроса.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @p AS INT = 10;  
SELECT TOP(@p)JobTitle, HireDate, VacationHours  
FROM HumanResources.Employee  
ORDER BY VacationHours DESC;  
GO  
```  
  
#### <a name="c-specifying-a-percentage"></a>В. Указание процентов  
В следующем примере PERCENT указывает число сотрудников, возвращаемых в результирующем наборе запроса. В таблице `HumanResources.Employee` содержится 290 сотрудников. 5 % от 290 является дробным числом, поэтому значение округляется вверх до целого числа.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(5)PERCENT JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY HireDate DESC;  
GO    
```  
  
###  <a name="including-tie-values"></a><a name="tie"></a> Включить равные значения  
  
#### <a name="a-using-with-ties-to-include-rows-that-match-the-values-in-the-last-row"></a>A. Использование ключевого слова WITH TIES для включения строк, соответствующих значениям в последней строке  
Следующий пример извлекает первые `10` процентов работников с наибольшей зарплатой и возвращает их в порядке убывания зарплаты. Выбор `WITH TIES` гарантирует, что в результирующий набор будут включены все работники с минимальной в этом списке зарплатой, даже если это приведет к превышению ограничения в `10` процентов работников.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT TOP(10) PERCENT WITH TIES  
pp.FirstName, pp.LastName, e.JobTitle, e.Gender, r.Rate  
FROM Person.Person AS pp   
    INNER JOIN HumanResources.Employee AS e  
        ON pp.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeePayHistory AS r  
        ON r.BusinessEntityID = e.BusinessEntityID  
ORDER BY Rate DESC;  
GO    
```  
  
###  <a name="limiting-the-rows-affected-by-delete-insert-or-update"></a><a name="DML"></a> Ограничение числа строк, обрабатываемых инструкциями DELETE, INSERT и UPDATE  
  
#### <a name="a-using-top-to-limit-the-number-of-rows-deleted"></a>A. Ограничение числа удаляемых строк с помощью ключевого слова TOP  
Если с инструкцией DELETE применяется предложение TOP (*n*), операция удаления производится с произвольной выборкой из *n* строк. Таким образом, инструкция DELETE выбирает любое число (*n*) строк, которые удовлетворяют условию, указанному в предложении WHERE. Следующий пример удаляет из таблицы `PurchaseOrderDetail``20` строк с датой завершения до 1 июля 2002 г.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
Если вы намерены с помощью предложения TOP удалять строки в требуемом хронологическом порядке, включите TOP во вложенный запрос с предложением ORDER BY. Следующий запрос удаляет из таблицы `PurchaseOrderDetail` 10 строк, имеющих самую раннюю дату. Чтобы гарантировать удаление только 10 строк, столбец, указанный в инструкции подзапроса выборки (`PurchaseOrderID`) должен являться первичным ключом таблицы. Использование неключевого столбца в инструкции подзапроса выборки может привести к удалению более чем 10 строк, если указанный столбец содержит повторяющиеся значения.  
  
```sql  
USE AdventureWorks2012;  
GO  
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
#### <a name="b-using-top-to-limit-the-number-of-rows-inserted"></a>Б. Ограничение числа вставляемых строк с помощью ключевого слова TOP  
В следующем примере создается таблица `EmployeeSales` и вставляется имя и данные о продажах за текущий год для 5 первых сотрудников из таблицы `HumanResources.Employee`. Инструкция INSERT выбирает из результата инструкции `SELECT` произвольные 5 строк, которые удовлетворяют условию, определенному в предложении WHERE. Предложение OUTPUT отображает строки, вставляемые в таблицу `EmployeeSales`. Обратите внимание, что в инструкции SELECT не используется предложение ORDER BY для определения 5 первых сотрудников.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   NVARCHAR(11) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  YearlySales  MONEY NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
Если вы намерены с помощью предложения TOP добавлять строки в требуемом хронологическом порядке, включите TOP во вложенный запрос с предложением ORDER BY. Следующий пример показывает, как это сделать. Предложение OUTPUT отображает строки, вставляемые в таблицу `EmployeeSales`. Обратите внимание, что вставка данных о 5 первых сотрудниках выполняется теперь по результатам предложения ORDER BY, а не для произвольных строк.  
  
```sql  
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
GO    
```  
  
#### <a name="c-using-top-to-limit-the-number-of-rows-updated"></a>В. Ограничение числа обновляемых строк с помощью ключевого слова TOP  
В следующем примере предложение TOP используется для обновления строк в таблице. Если предложение TOP (*n*) используется с инструкцией UPDATE, операция обновления выполняется для неопределенного количества строк. Таким образом, инструкция UPDATE выбирает любое число (*n*) строк, которые удовлетворяют условию, указанному в предложении WHERE. В следующем примере 10 случайно выбранных заказчиков переназначаются от одного менеджера по продажам к другому.  
  
```sql  
USE AdventureWorks2012;  
UPDATE TOP (10) Sales.Store  
SET SalesPersonID = 276  
WHERE SalesPersonID = 275;  
GO  
```  
  
Если нужно применить изменения с предложением TOP в определенной последовательности, укажите во вложенном запросе SELECT предложение ORDER BY. В следующем примере изменяется длительность отпуска для 10 сотрудников, имеющих наибольший стаж работы.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
В приведенном ниже примере возвращаются первые строки (31), соответствующие условию запроса. Предложение **ORDER BY** гарантирует, что возвращается 31 строка в порядке сортировки столбца `LastName` по алфавиту.  
  
Использование **TOP** без указания равных элементов.  
  
```sql  
SELECT TOP (31) FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Результат: возвращается 31 строка.  
  
Использование TOP с указанием WITH TIES.  
  
```sql  
SELECT TOP (31) WITH TIES FirstName, LastName   
FROM DimEmployee ORDER BY LastName;  
```  
  
Результат: возвращаются 33 строки, так как на 31-ю строку претендуют 3 сотрудника с фамилией Brown.  
  
## <a name="see-also"></a>См. также:  
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [Предложение ORDER BY &#40;Transact-SQL&#41;](../../t-sql/queries/select-order-by-clause-transact-sql.md)   
 [SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md)   
 [MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md)  
  
 

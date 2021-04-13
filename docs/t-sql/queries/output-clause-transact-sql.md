---
description: Предложение OUTPUT (Transact-SQL)
title: Предложение OUTPUT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/14/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OUTPUT_TSQL
- OUTPUT
dev_langs:
- TSQL
helpviewer_keywords:
- displaying updated rows
- INSERT statement [SQL Server], OUTPUT clause
- outputs [SQL Server]
- OUTPUT clause
- row additions [SQL Server], OUTPUT clause
- viewing updated rows
- row deletions [SQL Server], OUTPUT clause
- viewing deleted rows
- DELETE statement [SQL Server], OUTPUT clause
- row updates [SQL Server]
- displaying inserted rows
- viewing inserted rows
- displaying deleted rows
- UPDATE statement [SQL Server], OUTPUT clause
ms.assetid: 41b9962c-0c71-4227-80a0-08fdc19f5fe4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 194ce3acd5db4643053cde4b42bf5c0115ff9a37
ms.sourcegitcommit: 8f40e427c867b9d2e9e7dde43f9546fb4d907c4f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106506649"
---
# <a name="output-clause-transact-sql"></a>Предложение OUTPUT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает данные из строк, изменившихся в результате выполнения инструкций INSERT, UPDATE, DELETE или MERGE, или выражения на основе этих данных. Эти результаты могут быть возвращены приложению, например для вывода подтверждающих сообщений, архивирования и т. п. Результаты также могут быть вставлены в таблицу или табличную переменную. Кроме того, можно записать результаты предложения OUTPUT во вложенных инструкциях INSERT, UPDATE, DELETE или MERGE и вставить эти результаты в целевую таблицу или представление.  
  
> [!NOTE]  
>  Инструкция UPDATE, INSERT или DELETE с предложением OUTPUT возвращает строки клиенту даже в случае, если при выполнении инструкции возникли ошибки и был выполнен ее откат. Результат не может быть использован, если при выполнении инструкции возникли какие-либо ошибки.  
  
 **Применяется в:**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
<OUTPUT_CLAUSE> ::=  
{  
    [ OUTPUT <dml_select_list> INTO { @table_variable | output_table } [ ( column_list ) ] ]  
    [ OUTPUT <dml_select_list> ]  
}  
<dml_select_list> ::=  
{ <column_name> | scalar_expression } [ [AS] column_alias_identifier ]  
    [ ,...n ]  
  
<column_name> ::=  
{ DELETED | INSERTED | from_table_name } . { * | column_name }  
    | $action
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 \@*table_variable*  
 Указывает переменную **table**, в которую возвращенные строки вставляются вместо передачи вызывающему приложению. Аргумент \@*table_variable* необходимо объявить перед вызовом инструкции INSERT, UPDATE, DELETE или MERGE.  
  
 Если не указан аргумент *column_list*, переменная **table** должна иметь то же число столбцов, что и результирующий набор OUTPUT, за исключением столбцов идентификаторов и вычисляемых столбцов, которые следует пропустить. Если аргумент *column_list* указан, то любые пропущенные столбцы должны либо допускать значение NULL, либо для них должны быть определены значения по умолчанию.  
  
 Дополнительные сведения о переменных типа **table** см. в статье [table (Transact-SQL)](../../t-sql/data-types/table-transact-sql.md).  
  
 *output_table*  
 Указывает таблицу, в которую возвращенные строки вставляются вместо передачи вызывающему приложению. Таблица *output_table* может быть временной.  
  
 Если аргумент *column_list* не указан, то таблица должна иметь то же число столбцов, что и результирующий набор OUTPUT, за исключением столбцов идентификаторов и вычисляемых столбцов. Эти столбцы следует пропустить. Если аргумент *column_list* указан, то любые пропущенные столбцы должны либо допускать значение NULL, либо для них должны быть определены значения по умолчанию.  
  
 Таблица *output_table* не может:  
  
-   Иметь включенные триггеры, определенные для нее.  
  
-   Участвовать в ограничениях FOREIGN KEY с любой стороны.  
  
-   Иметь ограничения CHECK или активированные правила.  
  
*column_list*  
 Необязательный список имен столбцов для целевой таблицы, указанной в предложении INTO. Он аналогичен списку столбцов, указываемому в инструкции [INSERT](../../t-sql/statements/insert-transact-sql.md).  
  
 *scalar_expression*  
 Любое сочетание символов и операторов, результатом вычисления которого является единственное значение. Агрегатные функции в *scalar_expression* указывать нельзя.  
  
 Ссылки на изменяемые столбцы таблицы должны предваряться префиксом INSERTED или DELETED.  
  
 *column_alias_identifier*  
 Альтернативное имя, используемое для указания ссылок на этот столбец.  
  
 DELETED  
 Префикс столбца, который указывает на значение, удаляемое в результате выполнения операции обновления или удаления. Столбцы, имеющие префикс DELETED, отражают значение, которое было до завершения инструкции UPDATE, DELETE или MERGE.  
  
 Префикс DELETED не может указываться вместе с предложением OUTPUT в инструкции INSERT.  
  
 INSERTED  
 Префикс столбца, который указывает на значение, добавляемое в результате выполнении операции вставки или обновления. Столбцы, имеющие префикс INSERTED, отражают значение, полученное после завершения инструкции UPDATE, INSERT или MERGE, но до выполнения триггеров.  
  
 Префикс INSERTED не может указываться вместе с предложением OUTPUT в инструкции DELETE.  
  
 *from_table_name*  
 Префикс столбца, который обозначает таблицу, содержащуюся в предложении FROM инструкции DELETE, UPDATE или MERGE; эти инструкции указывают обновляемые или удаляемые строки.  
  
 Если изменяемая таблица указана также и в предложении FROM, все ссылки на столбцы, содержащиеся в этой таблице, должны предваряться префиксом INSERTED или DELETED.  
  
 \*  
 Указывает, что все столбцы, участвующие в операции удаления, вставки или обновления, возвращаются в том порядке, в котором они существуют в таблице.  
  
 Например, в следующей инструкции DELETE предложение `OUTPUT DELETED.*` возвращает все столбцы, удаленные из таблицы `ShoppingCartItem`:  
  
```sql
DELETE Sales.ShoppingCartItem
    OUTPUT DELETED.*;
```
  
 *column_name*  
 Явное указание столбца. Любая ссылка на изменяемую таблицу должна предваряться соответствующим префиксом INSERTED или DELETED, например: INSERTED **.** _столбец\_имя_.  
  
 $action  
 Доступен только для инструкции MERGE. Указывает столбец типа **nvarchar(10)** в предложении OUTPUT инструкции MERGE, которая возвращает одно из трех значений для каждой строки: INSERT, UPDATE или DELETE — согласно действию, которое было выполнено с этой строкой.  
  
## <a name="remarks"></a>Remarks  
 Предложение OUTPUT \<dml_select_list> и предложение OUTPUT \<dml_select_list> INTO { **\@** _table\_variable_ | _output\_table_ } можно определить в одной инструкции INSERT, UPDATE, DELETE или MERGE.  
  
> [!NOTE]  
>  Если не указано иное, ссылки на предложение OUTPUT относятся как к предложению OUTPUT, так и к предложению OUTPUT INTO.  
  
 Применение предложения OUTPUT может оказаться полезным при получении значения идентификаторов или вычисляемых столбцов после выполнения операций INSERT и UPDATE.  
  
 Если вычисляемый столбец включен в \<dml_select_list>, то соответствующий столбец в выходной таблице или табличной переменной вычисляемым не является. В него будет помещено значение, вычисленное в момент выполнения инструкции.  
  
 Нет никакой гарантии, что порядок, в котором изменения применяются к таблице, будет соответствовать порядку, в котором строки вставляются в выводную таблицу или табличную переменную.  
  
 Если параметры или переменные изменяются при выполнении инструкции UPDATE, предложение OUTPUT всегда возвращает значение параметра или переменной, которое было актуально до выполнения этой инструкции, а не измененное значение.  
  
 Предложение OUTPUT можно указывать с инструкциями UPDATE и DELETE, применяемыми к курсору с использованием синтаксиса WHERE CURRENT OF.  
  
 Предложение OUTPUT не поддерживается для:  
  
-   Инструкций DML, которые содержат ссылки на локальные секционированные представления, распределенные секционированные представления или удаленные таблицы.  
  
-   Инструкций INSERT, содержащих инструкции EXECUTE.  
  
-   Полнотекстовые предикаты не допускаются в предложении OUTPUT, если уровень совместимости базы данных установлен в 100.  
  
-   Предложение OUTPUT INTO не может быть использовано для вставки строк в представление или функцию, возвращающую набор строк.  
  
-   Определяемая пользователем функция не может быть создана, если в ней содержится предложение OUTPUT INTO, имеющее в качестве цели таблицу.  
  
 Чтобы предотвратить недетерминированное поведение, в предложении OUTPUT не могут содержаться следующие ссылки.  
  
-   Вложенные запросы или определяемые пользователем функции, которые обеспечивают (или предположительно обеспечивают) пользовательский или системный доступ к данным. Предполагается, что определяемые пользователем функции выполняют доступ к данным, если они не привязаны к схеме.  
  
-   Столбец из представления или встроенная функция с табличным значением, если этот столбец определяется с помощью одного из следующих методов.  
  
    -   вложенный запрос.  
  
    -   Определяемая пользователем функция, которая осуществляет или может осуществлять доступ к пользовательским или системным данным.  
  
    -   Вычисляемый столбец, содержащий определяемую пользователем функцию, которая осуществляет доступ к пользовательским или системным данным в своем определении.  
  
     При обнаружении [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] такого столбца в предложении OUTPUT возвращается ошибка 4186.   
  
## <a name="inserting-data-returned-from-an-output-clause-into-a-table"></a>Вставка в таблицу данных, которые были возвращены предложением OUTPUT  
 При сборе результатов предложения OUTPUT во вложенных инструкциях INSERT, UPDATE, DELETE или MERGE и вставки этих результатов в целевую таблицу или представление необходимо учитывать следующее.  
  
-   Вся операция является атомарной. Или инструкция INSERT выполняется вместе с вложенной инструкцией DML, содержащей предложение OUTPUT, или выполнение всей инструкции завершается с ошибкой.  
  
-   К целевому объекту внешней инструкции INSERT применяются следующие ограничения.  
  
    -   Целевой объект не должен быть удаленной таблицей, представлением или обобщенным табличным выражением.  
  
    -   Целевой объект не должен иметь ограничения FOREIGN KEY либо быть объектом ссылки ограничения FOREIGN KEY.  
  
    -   Триггеры не должны быть определены на целевом объекте.  
  
    -   Целевой объект не должен участвовать в репликации слиянием или обновляемых подписках для репликации транзакций.  
  
-   К вложенной инструкции DML применяются следующие ограничения.  
  
    -   Целевой объект не должен быть удаленной таблицей или секционированным представлением.  
  
    -   Сам источник не должен содержать предложение \<dml_table_source>.  
  
-   Предложение OUTPUT INTO не поддерживается в инструкциях INSERT, содержащих предложение \<dml_table_source>.  
  
-   \@\@ROWCOUNT возвращает только строки, вставленные внешней инструкцией INSERT.  
  
-   \@\@IDENTITY, SCOPE_IDENTITY и IDENT_CURRENT возвращают значения идентификаторов, созданные не внешней инструкцией INSERT, а только вложенной инструкцией DML.  
  
-   Уведомления о запросах рассматривают инструкцию как единую сущность, и тип любого созданного сообщения будет типом вложенной инструкции DML, даже если внешняя инструкция INSERT сделала значительное изменение.  
  
-   В предложении \<dml_table_source> предложения SELECT и WHERE не должны содержать вложенных запросов, агрегатных функций, ранжирующих функций, полнотекстовых предикатов, определяемых пользователем функций, осуществляющих доступ к данным, или функции TEXTPTR.  

## <a name="parallelism"></a>Parallelism
 Предложение OUTPUT, возвращающее результаты клиенту, всегда использует последовательный план.

Если в контексте базы данных с уровнем совместимости 130 или более высоким операция INSERT...SELECT использует указание WITH (TABLOCK) для инструкции SELECT, а также использует OUTPUT…INTO для вставки данных во временную или пользовательскую таблицу, конечная таблица для операции INSERT…SELECT допускает параллелизм в зависимости от стоимости поддерева.  Конечная таблица, указанная в предложении OUTPUT INTO, не допускает параллелизма. 
 
## <a name="triggers"></a>Триггеры  
 Возвращаемые из OUTPUT столбцы отражают данные после завершения выполнения инструкции INSERT, UPDATE или DELETE, но до выполнения триггеров.  
  
 Для триггеров INSTEAD OF возвращенные результаты формируются таким образом, как если бы операции INSERT, UPDATE или DELETE были действительно выполнены, даже если в результате выполнения триггера никакие реальные изменения данных не произведены. Если инструкция, включающая предложение OUTPUT, указывается в тексте триггера, то вставленные и удаленные таблицы триггера должны адресоваться по псевдонимам, чтобы избежать появления повторяющихся ссылок на столбцы в таблицах INSERTED и DELETED, относящихся к предложению OUTPUT.  
  
 Если предложение OUTPUT определено без указания ключевого слова INTO, для целевого объекта операции DML не могут быть определены триггеры, выполняемые для этой операции. Например, если предложение OUTPUT определено в инструкции UPDATE, целевая таблица не может иметь какие-либо включенные триггеры UPDATE.  
  
 Если установлен параметр disallow results from triggers процедуры sp_configure, инструкция с предложением OUTPUT без INTO при вызове из триггера приводит к ошибке.  
  
## <a name="data-types"></a>Типы данных  
 Предложение OUTPUT поддерживает типы данных больших объектов: **nvarchar(max)** , **varchar(max)** , **varbinary(max)** , **text**, **ntext**, **image** и **xml**. Если в инструкции UPDATE указано предложение .WRITE для изменения столбца **nvarchar(max)** , **varchar(max)** или **varbinary(max)** , то возвращаются полные образы значений до и после изменения, если на них есть ссылки. Функция TEXTPTR( ) не может входить в выражение, определенное в предложении OUTPUT для столбца, имеющего тип **text**, **ntext** или **image**.  
  
## <a name="queues"></a>Очереди  
 Предложение OUTPUT может применяться в приложениях, которые применяют таблицы в качестве очередей или для хранения промежуточных результирующих наборов, то есть в приложениях, которые постоянно добавляют и удаляют строки из таблиц. В следующем примере предложение OUTPUT указано в инструкции DELETE и возвращает удаленную строку вызывающему приложению.  
  
```sql
USE AdventureWorks2012;
GO

DELETE TOP(1) dbo.DatabaseLog WITH (READPAST)  
OUTPUT deleted.*  
WHERE DatabaseLogID = 7;  
GO
```
  
 В этом примере строка удаляется из таблицы, используемой в качестве очереди, и удаляемое значение возвращается приложению. Можно реализовать также и другую семантику, например применение таблицы как стека. Однако при этом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не гарантирует порядок, в котором строки обрабатываются и возвращаются инструкциями DML с предложением OUTPUT. Приложение должно указать соответствующее предложение WHERE, гарантирующее желаемую семантику, либо ориентироваться на то, что порядок, в котором строки обрабатываются операцией DML, не гарантируется. В следующем примере для реализации порядка обработки строк применяется вложенный запрос, который предполагает уникальность значений в столбце `DatabaseLogID`.  
  
```sql
USE tempdb;
GO

CREATE TABLE dbo.table1  
(  
    id INT,  
    employee VARCHAR(32)  
);  
GO  
  
INSERT INTO dbo.table1 VALUES   
      (1, 'Fred')  
     ,(2, 'Tom')  
     ,(3, 'Sally')  
     ,(4, 'Alice');  
GO  
  
DECLARE @MyTableVar TABLE  
(  
    id INT,  
    employee VARCHAR(32)  
);  
  
PRINT 'table1, before delete'   
SELECT * FROM dbo.table1;  
  
DELETE FROM dbo.table1  
OUTPUT DELETED.* INTO @MyTableVar  
WHERE id = 4 OR id = 2;  
  
PRINT 'table1, after delete'  
SELECT * FROM dbo.table1;  
  
PRINT '@MyTableVar, after delete'  
SELECT * FROM @MyTableVar;  
  
DROP TABLE dbo.table1;  
```

```
--Results  
--table1, before delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--2           Tom  
--3           Sally  
--4           Alice  
--  
--table1, after delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--3           Sally  
--@MyTableVar, after delete  
--id          employee  
------------- ------------------------------  
--2           Tom  
--4           Alice
```
  
> [!NOTE]  
>  Если сценарий позволяет нескольким приложениям производить разрушающее чтение из одной таблицы, в инструкциях UPDATE и DELETE следует указывать табличную подсказку READPAST. Это предотвратит блокировку, которая может возникнуть, если другое приложение уже считывает из таблицы первую подходящую запись.  
  
## <a name="permissions"></a>Разрешения  
 Необходимы разрешения SELECT на все столбцы, полученные через \<dml_select_list> или указанные в \<scalar_expression>.  
  
 Разрешения INSERT необходимы для всех таблиц, указанных в \<output_table>.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-output-into-with-a-simple-insert-statement"></a>A. Применение предложения OUTPUT INTO в простой инструкции INSERT  
 В следующем примере производится вставка строки в таблицу `ScrapReason`, а затем при помощи предложения `OUTPUT` результаты выполнения инструкции возвращаются в табличную переменную `@MyTableVar`. Поскольку столбец `ScrapReasonID` определен со свойством IDENTITY, для него значение в инструкции `INSERT` не указывается. Обратите внимание, что значение, которое компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] сформировал для этого столбца, возвращается предложением `OUTPUT` в столбец `inserted.ScrapReasonID`.  

  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (
    NewScrapReasonID SMALLINT,  
    Name VARCHAR(50),  
    ModifiedDate DATETIME); 
    
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
GO
```
  
### <a name="b-using-output-with-a-delete-statement"></a>Б. Применение предложения OUTPUT в инструкции DELETE  
 В следующем примере производится удаление всех строк из таблицы `ShoppingCartItem`. Предложение `OUTPUT deleted.*` указывает, что результаты выполнения инструкции `DELETE`, то есть все столбцы удаляемых строк, будут возвращены вызывающему приложению. Следующая инструкция `SELECT` проверяет результаты операции удаления из таблицы `ShoppingCartItem`.  
  
```sql
USE AdventureWorks2012;
GO

DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] FROM Sales.ShoppingCartItem WHERE ShoppingCartID = 20621;  
GO
```
  
### <a name="c-using-output-into-with-an-update-statement"></a>В. Применение предложения OUTPUT INTO в инструкции UPDATE  
 В следующем примере в столбце `VacationHours` для первых 10 строк таблицы `Employee` устанавливается значение 25 %. Предложение `OUTPUT` возвращает значение `VacationHours`, существующее до применения инструкции `UPDATE` в столбце `deleted.VacationHours`, и обновленное значение в столбце `inserted.VacationHours` к табличной переменной `@MyTableVar`.  
  
 Две следующие инструкции `SELECT` возвращают значения в табличную переменную `@MyTableVar`, а результаты операции обновления — в таблицу `Employee`.  
  
```sql
USE AdventureWorks2012;
GO
  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```

### <a name="d-using-output-into-to-return-an-expression"></a>Г. Применение предложения OUTPUT INTO для возврата выражения  
 Следующий пример, основанный на предыдущем примере, определяет выражение в предложении `OUTPUT` как разницу между обновленным значением `VacationHours` и значением `VacationHours` до применения операции обновления. Значение этого выражения возвращается в табличную переменную `@MyTableVar` в столбце `VacationHoursDifference`.  

  
```sql
USE AdventureWorks2012;  
GO

DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    VacationHoursDifference INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()  
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.VacationHours - deleted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours,   
    VacationHoursDifference, ModifiedDate  
FROM @MyTableVar;  
GO  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO
```

### <a name="e-using-output-into-with-from_table_name-in-an-update-statement"></a>Д. Применение предложения OUTPUT INTO с from_table_name в инструкции UPDATE  
 В приведенном ниже примере производится обновление столбца `ScrapReasonID` таблицы `WorkOrder` для всех заказов на производство с указанными значениями `ProductID` и `ScrapReasonID`. Предложение `OUTPUT INTO` возвращает значения из обновляемой таблицы (`WorkOrder`), а также из таблицы `Product`. Таблица `Product` в предложении `FROM` указывает, какие строки следует обновлять. Для таблицы `WorkOrder` определен триггер `AFTER UPDATE`, поэтому требуется ключевое слово `INTO`.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTestVar TABLE (  
    OldScrapReasonID INT NOT NULL,   
    NewScrapReasonID INT NOT NULL,   
    WorkOrderID INT NOT NULL,  
    ProductID INT NOT NULL,  
    ProductName NVARCHAR(50)NOT NULL);  
  
UPDATE Production.WorkOrder  
SET ScrapReasonID = 4  
OUTPUT deleted.ScrapReasonID,  
       inserted.ScrapReasonID,   
       inserted.WorkOrderID,  
       inserted.ProductID,  
       p.Name  
    INTO @MyTestVar  
FROM Production.WorkOrder AS wo  
    INNER JOIN Production.Product AS p   
    ON wo.ProductID = p.ProductID   
    AND wo.ScrapReasonID= 16  
    AND p.ProductID = 733;  
  
SELECT OldScrapReasonID, NewScrapReasonID, WorkOrderID,   
    ProductID, ProductName   
FROM @MyTestVar;  
GO
```

### <a name="f-using-output-into-with-from_table_name-in-a-delete-statement"></a>Е. Применение предложения OUTPUT INTO с from_table_name в инструкции DELETE  
 В следующем примере производится удаление строк из таблицы `ProductProductPhoto` на основе критерия поиска, определенного в предложении `FROM` инструкции `DELETE`. Предложение `OUTPUT` возвращает столбцы из таблицы, из которой производится удаление (`deleted.ProductID`, `deleted.ProductPhotoID`) и столбцы из таблицы `Product`. Эта таблица, указанная в предложении `FROM`, определяет, какие строки следует удалять.  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO
```
  
### <a name="g-using-output-into-with-a-large-object-data-type"></a>Ж. Применение предложения OUTPUT INTO с типом данных больших объектов  
 В следующем примере для обновления части значения в столбце `DocumentSummary`, имеющем тип `nvarchar(max)` в таблице `Production.Document`, используется предложение `.WRITE`. Слово `components` заменяется словом `features`, при этом указывается новое слово, начальное смещение слова, заменяемого в исходном тексте и число заменяемых символов (длина). В примере предложение `OUTPUT` возвращает образы столбца `DocumentSummary` до и после изменения в табличную переменную `@MyTableVar`. Обратите внимание, что возвращаются полные образы столбца `DocumentSummary` до и после изменения.  

  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO
```
  
### <a name="h-using-output-in-an-instead-of-trigger"></a>З. Применение предложения OUTPUT в триггере INSTEAD OF  
 Следующий пример демонстрирует применение предложения `OUTPUT` в триггере для возвращения результатов выполнения триггера. Первым делом создается представление для таблицы `ScrapReason`, затем для этого представления определяется триггер `INSTEAD OF INSERT`, позволяющий пользователю изменять в базовой таблице только столбец `Name`. Так как столбец `ScrapReasonID` базовой таблицы является столбцом `IDENTITY`, триггер не учитывает значение, предоставленное пользователем. Это приводит к тому, что компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] автоматически формирует верное значение. Указанное пользователем значение `ModifiedDate` также не учитывается, и вместо него подставляется текущая дата. Предложение `OUTPUT` возвращает значения, реально вставленные в таблицу `ScrapReason`.  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID('dbo.vw_ScrapReason','V') IS NOT NULL  
    DROP VIEW dbo.vw_ScrapReason;  
GO  
CREATE VIEW dbo.vw_ScrapReason  
AS (SELECT ScrapReasonID, Name, ModifiedDate  
    FROM Production.ScrapReason);  
GO  
CREATE TRIGGER dbo.io_ScrapReason   
    ON dbo.vw_ScrapReason  
INSTEAD OF INSERT  
AS  
BEGIN  
--ScrapReasonID is not specified in the list of columns to be inserted   
--because it is an IDENTITY column.  
    INSERT INTO Production.ScrapReason (Name, ModifiedDate)  
        OUTPUT INSERTED.ScrapReasonID, INSERTED.Name,   
               INSERTED.ModifiedDate  
    SELECT Name, getdate()  
    FROM inserted;  
END  
GO  
INSERT vw_ScrapReason (ScrapReasonID, Name, ModifiedDate)  
VALUES (99, N'My scrap reason','20030404');  
GO
```
  
 Ниже приведен результирующий набор, полученный 12 апреля 2004 года ('`2004-04-12'`). Обратите внимание, что столбцы `ScrapReasonIDActual` и `ModifiedDate` отражают те значения, которые были получены в результате выполнения триггера, а не те, что были указаны в инструкции `INSERT`.  
  
 ```
 ScrapReasonID  Name             ModifiedDate  
 -------------  ---------------- -----------------------  
 17             My scrap reason  2004-04-12 16:23:33.050
 ```  
  
### <a name="i-using-output-into-with-identity-and-computed-columns"></a>И. Применение предложения OUTPUT INTO со столбцами идентификаторов и вычисляемыми столбцами  
 В следующем примере создается таблица `EmployeeSales`, а затем в нее с помощью инструкции `INSERT` вставляется несколько строк, получаемых инструкцией `SELECT` из исходных таблиц. Таблица `EmployeeSales` содержит столбец идентификаторов (`EmployeeID`) и вычисляемый столбец (`ProjectedSales`).  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   INT IDENTITY (1,5) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar TABLE (  
  EmployeeID   INT NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales MONEY NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.EmployeeID,
         INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales,
         INSERTED.ProjectedSales
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
GO
```
  
### <a name="j-using-output-and-output-into-in-a-single-statement"></a>К. Применение предложений OUTPUT и OUTPUT INTO в одной инструкции  
 В следующем примере производится удаление строк из таблицы `ProductProductPhoto` на основе критерия поиска, определенного в предложении `FROM` инструкции `DELETE`. Предложение `OUTPUT INTO` возвращает столбцы из удаляемой таблицы (`deleted.ProductID`, `deleted.ProductPhotoID`) и столбцы из таблицы `Product` в табличную переменную `@MyTableVar`. Таблица `Product` в предложении `FROM` определяет, какие строки необходимо удалять. Предложение `OUTPUT` возвращает вызывающему приложению столбцы `deleted.ProductID`, `deleted.ProductPhotoID`, а также дату и время удаления строки из таблицы `ProductProductPhoto`.  

  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
OUTPUT DELETED.ProductID, DELETED.ProductPhotoID, GETDATE() AS DeletedDate   
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
WHERE p.ProductID BETWEEN 800 and 810;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, PhotoID, ProductModelID   
FROM @MyTableVar;  
GO
```
  
### <a name="k-inserting-data-returned-from-an-output-clause"></a>Л. Вставка данных, возвращенных предложением OUTPUT  
 В следующем примере собираются данные, возвращаемые предложением `OUTPUT` инструкции `MERGE`, а затем эти данные вставляются в другую таблицу. Инструкция `MERGE` ежедневно обновляет столбец `Quantity` таблицы `ProductInventory` в соответствии с заказами, обрабатываемыми в таблице `SalesOrderDetail`. Инструкция также удаляет строки с продуктами, запас которых сократился до `0` или ниже. В примере собираются удаленные строки и вставляются в другую таблицу, `ZeroInventory`, в которой ведется учет закончившихся продуктов.  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID(N'Production.ZeroInventory', N'U') IS NOT NULL  
    DROP TABLE Production.ZeroInventory;  
GO  
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate
FROM Production.ZeroInventory;
GO
```
  
## <a name="see-also"></a>См. также:  
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [table (Transact-SQL)](../../t-sql/data-types/table-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)

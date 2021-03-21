---
description: Хранимая процедура sp_executesql (Transact-SQL)
title: sp_executesql (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_executesql
- sp_executesql_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_executesql
- dynamic SQL
ms.assetid: a8d68d72-0f4d-4ecb-ae86-1235b962f646
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 48c145f62573b3a4a0388dfbaf3ab0e7ba6aa181
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750034"
---
# <a name="sp_executesql-transact-sql"></a>Хранимая процедура sp_executesql (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Выполняет инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] или пакет инструкций, которые могут выполняться много раз или создаваться динамически. Инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] или пакет инструкций могут содержать параметры.  
  
> [!IMPORTANT]  
>  Компиляция инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] во время выполнения подвергает приложения риску злонамеренного воздействия.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_executesql [ @stmt = ] statement  
[   
  { , [ @params = ] N'@parameter_name data_type [ OUT | OUTPUT ][ ,...n ]' }   
     { , [ @param1 = ] 'value1' [ ,...n ] }  
]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ \@ stmt =], *инструкция*  
 Строка в Юникоде, содержащая [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкцию или пакет. \@значение stmt должно быть константой Юникода или переменной Юникода. Более сложные выражения Юникода, например объединение двух строк с помощью оператора +, недопустимы. Символьные константы недопустимы. Если константа Юникода указана, она должна иметь префикс **N**. Например, константа в Юникоде **N "sp_who"** допустима, но символьная константа **"sp_who"** не является. Размер строки ограничивается только доступной серверу баз данных памятью. На 64-разрядных серверах размер строки ограничен 2 ГБ, максимальный размер — **nvarchar (max)**.  
  
> [!NOTE]  
>  \@stmt может содержать параметры, имеющие ту же форму, что и имя переменной, например: `N'SELECT * FROM HumanResources.Employee WHERE EmployeeID = @IDParameter'`  
  
 Каждый параметр, входящий в \@ stmt, должен иметь соответствующую запись в \@ списке определений параметров params и в списке значений параметров.  
  
 [ \@ params =] N ' \@ *parameter_name* *data_type* [,... *n* ] "  
 — Это одна строка, содержащая определения всех параметров, внедренных в \@ stmt. Строка должна быть либо константой Юникода, либо переменной Юникода. Определение каждого параметра состоит из имени параметра и типа данных. *n* — это заполнитель, указывающий дополнительные определения параметров. Каждый параметр, указанный в \@ stmt, должен быть определен в \@ параметре params. Если [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкция или пакет в \@ stmt не содержит параметров, \@ params не требуется. Этот аргумент по умолчанию принимает значение NULL.  
  
 [ \@ param1 =] '*Значение1*'  
 Значение для первого параметра, определенного в строке параметров. Это значение может быть константой или переменной в Юникоде. Для каждого параметра, входящего в stmt, должно быть указано значение параметра \@ . Значения не требуются, если [!INCLUDE[tsql](../../includes/tsql-md.md)] в инструкции или пакете в \@ stmt нет параметров.  
  
 [ OUT | OUTPUT ]  
 Показывает, что параметр процедуры является выходным. параметры **Text**, **ntext** и **Image** можно использовать в качестве выходных параметров, если только процедура не является процедурой среды CLR. Выходным параметром с ключевым словом OUTPUT может быть заполнитель курсора, если процедура не является процедурой CLR.  
  
 *n*  
 Заполнитель для значений дополнительных параметров. Значения могут быть только константами и переменными. Значения не могут представлять собой сложные выражения, такие как функции или выражения, построенные с помощью операторов.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или ненулевое значение (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 Возвращает результирующие наборы всех заданных инструкций SQL, встроенные в строку SQL.  
  
## <a name="remarks"></a>Remarks  
 sp_executesql параметры должны быть указаны в определенном порядке, как описано в разделе "синтаксис" ранее в этом разделе. Если параметры вводятся не в этом порядке, будет выдано сообщение об ошибке.  
  
 Относительно пакетов инструкций, области имен и контекста базы данных процедура sp_executesql ведет себя аналогично инструкции EXECUTE. [!INCLUDE[tsql](../../includes/tsql-md.md)]Инструкция или пакет в \@ параметре sp_executesql stmt не компилируются до тех пор, пока не будет выполнена инструкция sp_executesql. Содержимое \@ stmt компилируется и выполняется в виде плана выполнения, отделенного от плана выполнения пакета, который вызывал sp_executesql. Пакет, содержащийся в процедуре sp_executesql, не может ссылаться на переменные, объявленные в пакете, вызвавшем sp_executesql. Локальные курсоры или переменные в пакете sp_executesql недоступны пакету, вызвавшему sp_executesql. Изменения в контексте базы данных длятся только до завершения выполнения инструкции sp_executesql.  
  
 Процедура sp_executesql может использоваться вместо хранимых процедур для многократного выполнения инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], где единственные различия между инструкциями — значения параметров. Так как инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] сама остается неизменной и меняются только значения параметров, оптимизатор запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], вероятнее всего, повторно использует план выполнения, сформированный перед первым выполнением.  
  
> [!NOTE]  
>  Для улучшения производительности используйте полные имена объектов в строке инструкции.  
  
 Хранимая процедура sp_executesql поддерживает задание значений параметрам отдельно от строки [!INCLUDE[tsql](../../includes/tsql-md.md)], как показано в следующем примере.  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
  
/* Build the SQL string one time.*/  
SET @SQLString =  
     N'SELECT BusinessEntityID, NationalIDNumber, JobTitle, LoginID  
       FROM AdventureWorks2012.HumanResources.Employee   
       WHERE BusinessEntityID = @BusinessEntityID';  
SET @ParmDefinition = N'@BusinessEntityID tinyint';  
/* Execute the string with the first parameter value. */  
SET @IntVariable = 197;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
/* Execute the same string with the second parameter value. */  
SET @IntVariable = 109;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
```  
  
 Выходные параметры также могут быть использованы sp_executesql. В следующем примере название задания получается из таблицы `AdventureWorks2012.HumanResources.Employee` и возвращается в выходном параметре `@max_title`.  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @max_title VARCHAR(30);  
  
SET @IntVariable = 197;  
SET @SQLString = N'SELECT @max_titleOUT = max(JobTitle)   
   FROM AdventureWorks2012.HumanResources.Employee  
   WHERE BusinessEntityID = @level';  
SET @ParmDefinition = N'@level TINYINT, @max_titleOUT VARCHAR(30) OUTPUT';  
  
EXECUTE sp_executesql @SQLString, @ParmDefinition, @level = @IntVariable, @max_titleOUT=@max_title OUTPUT;  
SELECT @max_title;  
```  
  
 Возможность подставлять разные значения параметров в sp_executesql предоставляет следующие преимущества перед использованием инструкции EXECUTE.  
  
-   Так как собственно текст инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] в строке sp_executesql не меняется между выполнениями, оптимизатор запросов, вероятнее всего, сопоставит инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] во время второго выполнения с планом выполнения, сформированным во время первого выполнения. Следовательно, компиляция второй инструкции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не обязательна.  
  
-   Строка [!INCLUDE[tsql](../../includes/tsql-md.md)] строится только один раз.  
  
-   Целочисленный параметр определен в собственном формате. Приведение к Юникоду не требуется.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли public.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-executing-a-simple-select-statement"></a>A. Выполнение простой инструкции SELECT  
 В следующем примере создается и выполняется простая инструкция `SELECT`, содержащая внедренный параметр с именем `@level`.  
  
```sql  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorks2012.HumanResources.Employee   
          WHERE BusinessEntityID = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
### <a name="b-executing-a-dynamically-built-string"></a>Б. Выполнение динамически построенной строки  
 В следующем примере показано использование процедуры `sp_executesql` для выполнения динамически построенной строки. В этом примере хранимая процедура вставляет данные в набор таблиц, использующихся для секционирования данных о продажах по одному году. Для каждого месяца года создается одна таблица следующего формата:  
  
```sql  
CREATE TABLE May1998Sales  
    (OrderID INT PRIMARY KEY,  
    CustomerID INT NOT NULL,  
    OrderDate  DATETIME NULL  
        CHECK (DATEPART(yy, OrderDate) = 1998),  
    OrderMonth INT  
        CHECK (OrderMonth = 5),  
    DeliveryDate DATETIME NULL,  
        CHECK (DATEPART(mm, OrderDate) = OrderMonth)  
    )  
```  
  
 В этом образце хранимая процедура динамически строит и выполняет инструкцию `INSERT` для вставки новых заказов в соответствующую таблицу. В этом примере используется дата заказа для формирования имени таблицы, которая должна содержать данные, затем полученное имя вставляется в инструкцию `INSERT`.  
  
> [!NOTE]  
>  Это простой пример использования процедуры sp_executesql. Пример не включает в себя проверку ошибок и бизнес-правил, которые, например гарантируют то, что номера заказов не будут дублироваться в разных таблицах.  
  
```sql  
CREATE PROCEDURE InsertSales @PrmOrderID INT, @PrmCustomerID INT,  
                 @PrmOrderDate DATETIME, @PrmDeliveryDate DATETIME  
AS  
DECLARE @InsertString NVARCHAR(500)  
DECLARE @OrderMonth INT  
  
-- Build the INSERT statement.  
SET @InsertString = 'INSERT INTO ' +  
       /* Build the name of the table. */  
       SUBSTRING( DATENAME(mm, @PrmOrderDate), 1, 3) +  
       CAST(DATEPART(yy, @PrmOrderDate) AS CHAR(4) ) +  
       'Sales' +  
       /* Build a VALUES clause. */  
       ' VALUES (@InsOrderID, @InsCustID, @InsOrdDate,' +  
       ' @InsOrdMonth, @InsDelDate)'  
  
/* Set the value to use for the order month because  
   functions are not allowed in the sp_executesql parameter  
   list. */  
SET @OrderMonth = DATEPART(mm, @PrmOrderDate)  
  
EXEC sp_executesql @InsertString,  
     N'@InsOrderID INT, @InsCustID INT, @InsOrdDate DATETIME,  
       @InsOrdMonth INT, @InsDelDate DATETIME',  
     @PrmOrderID, @PrmCustomerID, @PrmOrderDate,  
     @OrderMonth, @PrmDeliveryDate  
  
GO  
```  
  
 Применение процедуры sp_executesql в этом случае более эффективно, чем использование инструкции EXECUTE для выполнения строки. При использовании процедуры sp_executesql формируется только 12 версий инструкции INSERT, по одной для таблицы каждого месяца. При использовании EXECUTE каждая инструкция INSERT должна быть уникальной, так как значения параметров будут различными. И хотя с помощью обоих методов будет создано одинаковое число пакетов, подобие инструкций INSERT, сформированных sp_executesql, увеличивает вероятность того, что оптимизатор запросов повторно использует планы выполнения.  
  
### <a name="c-using-the-output-parameter"></a>В. Использование параметра OUTPUT  
 В следующем примере используется `OUTPUT` параметр для хранения результирующего набора, созданного `SELECT` инструкцией в `@SQLString` параметре. `SELECT` Затем выполняются две инструкции, использующие значение `OUTPUT` параметра.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @SalesOrderNumber NVARCHAR(25);  
DECLARE @IntVariable INT;  
SET @SQLString = N'SELECT @SalesOrderOUT = MAX(SalesOrderNumber)  
    FROM Sales.SalesOrderHeader  
    WHERE CustomerID = @CustomerID';  
SET @ParmDefinition = N'@CustomerID INT,  
    @SalesOrderOUT NVARCHAR(25) OUTPUT';  
SET @IntVariable = 22276;  
EXECUTE sp_executesql  
    @SQLString  
    ,@ParmDefinition  
    ,@CustomerID = @IntVariable  
    ,@SalesOrderOUT = @SalesOrderNumber OUTPUT;  
-- This SELECT statement returns the value of the OUTPUT parameter.  
SELECT @SalesOrderNumber;  
-- This SELECT statement uses the value of the OUTPUT parameter in  
-- the WHERE clause.  
SELECT OrderDate, TotalDue  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderNumber = @SalesOrderNumber;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-executing-a-simple-select-statement"></a>Г. Выполнение простой инструкции SELECT  
 В следующем примере создается и выполняется простая инструкция `SELECT`, содержащая внедренный параметр с именем `@level`.  
  
```sql  
-- Uses AdventureWorks  
  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorksPDW2012.dbo.DimEmployee   
          WHERE EmployeeKey = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
## <a name="see-also"></a>См. также:  
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

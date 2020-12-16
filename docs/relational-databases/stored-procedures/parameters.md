---
title: Параметры | Документация Майкрософт
description: Узнайте, как использовать параметры для обмена данными между хранимой процедурой или функцией и вызвавшим ее приложением или инструментом.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], parameters
- user-defined functions [SQL Server], parameters
ms.assetid: c1f9bd93-3271-4098-a23b-7bd7a19ab65b
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9fb820d31c071850d2a2fc510368be4f3bb1839
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473085"
---
# <a name="parameters"></a>Параметры
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Параметры используются для обмена данными между хранимой процедурой или функцией и вызвавшим ее приложением или инструментом. 

*  Входные параметры позволяют передать значение в хранимую процедуру или функцию.
*  Выходные параметры позволяют возвратить из хранимой процедуры значение или переменную-курсор. Определяемые пользователем функции не могут задавать выходные параметры.
*  Каждая хранимая процедура возвращает целочисленный код возврата. Если хранимая процедура не задает значение кода возврата явно, возвращается значение 0.

Использование входного параметра, выходного параметра и кода возврата поясняет следующая хранимая процедура:
```
-- Create a procedure that takes one input parameter and returns one output parameter and a return code.
CREATE PROCEDURE SampleProcedure @EmployeeIDParm INT,
         @MaxTotal INT OUTPUT
AS
-- Declare and initialize a variable to hold @@ERROR.
DECLARE @ErrorSave INT
SET @ErrorSave = 0

-- Do a SELECT using the input parameter.
SELECT FirstName, LastName, JobTitle
FROM HumanResources.vEmployee
WHERE EmployeeID = @EmployeeIDParm

-- Save any nonzero @@ERROR value.
IF (@@ERROR <> 0)
   SET @ErrorSave = @@ERROR

-- Set a value in the output parameter.
SELECT @MaxTotal = MAX(TotalDue)
FROM Sales.SalesOrderHeader;

IF (@@ERROR <> 0)
   SET @ErrorSave = @@ERROR

-- Returns 0 if neither SELECT statement had an error; otherwise, returns the last error.
RETURN @ErrorSave
GO
```

При вызове хранимой процедуры или функции значениями входных параметров могут быть или константы, или значения переменных. Значения выходных параметров и кодов возврата нужно возвращать в переменных. Значения параметров и кодов возврата можно назначать переменным Transact-SQL или переменным приложения.

Если хранимая процедура вызывается в пакете или скрипте, в качестве параметров и кодов возврата можно использовать переменные Transact-SQL, определенные в том же пакете. Например, ниже показан пакет, выполняющий созданную ранее процедуру. Входной параметр задается как константа, а выходной параметр и код возврата возвращаются в переменных Transact-SQL:
```
-- Declare the variables for the return code and output parameter.
DECLARE @ReturnCode INT
DECLARE @MaxTotalVariable INT

-- Execute the stored procedure and specify which variables
-- are to receive the output parameter and return code values.
EXEC @ReturnCode = SampleProcedure @EmployeeIDParm = 19,
   @MaxTotal = @MaxTotalVariable OUTPUT

-- Show the values returned.
PRINT ' '
PRINT 'Return code = ' + CAST(@ReturnCode AS CHAR(10))
PRINT 'Maximum Quantity = ' + CAST(@MaxTotalVariable AS CHAR(10))
GO
```

Для обмена данными между переменными приложения, параметрами и кодами возврата в приложениях можно использовать маркеры параметров, связанные с переменными программы.

## <a name="see-also"></a>См. также:
[CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [Параметры и повторное использование планов выполнения](../../relational-databases/query-processing-architecture-guide.md)   
 [Переменные (Transact-SQL)](../../t-sql/language-elements/variables-transact-sql.md)

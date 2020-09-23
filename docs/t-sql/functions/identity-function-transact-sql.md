---
description: IDENTITY (функция) (Transact-SQL)
title: IDENTITY (функция) (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY function
- SELECT statement [SQL Server], IDENTITY function
- inserting identity columns
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY function
ms.assetid: ebec77eb-fc02-4feb-b6c5-f0098d43ccb6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 364bcd6eb68ca7c529c56fae70109b79cbc1dc18
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2020
ms.locfileid: "91114730"
---
# <a name="identity-function-transact-sql"></a>IDENTITY (функция) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Используется только в инструкции SELECT с предложением INTO *table* для вставки столбца идентификаторов в новую таблицу. Хотя они похожи, функция IDENTITY не является свойством IDENTITY, которое используется с инструкциями CREATE TABLE и ALTER TABLE.  
  
> [!NOTE]  
>  Сведения о том, как создать автоматически увеличивающееся числовое значение, которое может использоваться в нескольких таблицах или вызываться из приложений без ссылки на какие-либо таблицы, см. в разделе [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
IDENTITY (data_type [ , seed , increment ] ) AS column_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *data_type*  
 Тип данных столбца идентификаторов. Для столбца идентификаторов допустимы любые целочисленные типы данных, за исключением типов **bit** и **decimal**.  
  
 *seed*  
 Целочисленное значение, присваиваемое первой строке таблицы. Каждой последующей строке присваивается следующее значение идентификатора, равное последнему значению IDENTITY, увеличенному на значение *increment*. Если не указан ни аргумент *seed*, ни аргумент *increment*, то значения по умолчанию обоих равны 1.  
  
 *increment*  
 Целочисленное значение, добавляемое к значению *seed* для каждой последующей строки таблицы.  
  
 *column_name*  
 Имя столбца, который вставляется в новую таблицу.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Возвращает тот же тип, что и аргумент *data_type*.  
  
## <a name="remarks"></a>Комментарии  
 Так как данная функция создает столбец в таблице, имя столбца должно быть указано в списке выбора одним из следующих способов:  
  
```sql  
--(1)  
SELECT IDENTITY(int, 1,1) AS ID_Num  
INTO NewTable  
FROM OldTable;  
  
--(2)  
SELECT ID_Num = IDENTITY(int, 1, 1)  
INTO NewTable  
FROM OldTable;  
```  
  
## <a name="examples"></a>Примеры  
 В следующем примере все строки из таблицы `Contact` базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] вставляются в новую таблицу с именем `NewContact`. Функция IDENTITY используется, чтобы начать в таблице `NewContact` отсчет идентификационных номеров с 100 вместо 1.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'Person.NewContact', N'U') IS NOT NULL  
    DROP TABLE Person.NewContact;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY BULK_LOGGED;  
GO  
SELECT  IDENTITY(smallint, 100, 1) AS ContactNum,  
        FirstName AS First,  
        LastName AS Last  
INTO Person.NewContact  
FROM Person.Person;  
GO  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
GO  
SELECT ContactNum, First, Last FROM Person.NewContact;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [@@IDENTITY (Transact-SQL)](../../t-sql/functions/identity-transact-sql.md)   
 [Свойство IDENTITY (Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [SELECT @local_variable (Transact-SQL)](../../t-sql/language-elements/select-local-variable-transact-sql.md)   
 [DBCC CHECKIDENT (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [sys.identity_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-identity-columns-transact-sql.md)  
  
  

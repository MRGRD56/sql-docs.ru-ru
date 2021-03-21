---
description: CREATE TABLE (Transact-SQL) IDENTITY (Свойство)
title: IDENTITY (свойство) (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IDENTITY_TSQL
- IDENTITY
dev_langs:
- TSQL
helpviewer_keywords:
- IDENTITY property
- columns [SQL Server], creating
- identity columns [SQL Server], IDENTITY property
- autonumbers, identity numbers
ms.assetid: 8429134f-c821-4033-a07c-f782a48d501c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 848ba2bd20e77c8dff156482256639f4675562ad
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752674"
---
# <a name="create-table-transact-sql-identity-property"></a>CREATE TABLE (Transact-SQL) IDENTITY (Свойство)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Создает в таблице столбец идентификаторов. Это свойство указывается в инструкциях языка [!INCLUDE[tsql](../../includes/tsql-md.md)] CREATE TABLE и ALTER TABLE.  
  
> [!NOTE]  
>  Свойство IDENTITY отличается от свойства **Identity** распределенных управляющих объектов SQL (SQL-DMO), обеспечивающего доступ к свойству идентификаторов строк в столбцах.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
IDENTITY [ (seed , increment) ]
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *seed*  
 Значение, присваиваемое самой первой строке, загружаемой в таблицу.  
  
 *increment*  
 Значение приращения, которое прибавляется к значению идентификатора предыдущей загруженной строки.

 > [!NOTE]
 > В Azure Synapse Analytics значения для удостоверения не являются добавочными из-за распределенной архитектуры хранилища данных. Дополнительные сведения см. в статье [Использование свойства IDENTITY для создания суррогатных ключей в пуле Synapse SQL](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#allocation-of-values).
  
 Необходимо указывать либо оба аргумента (и seed, и increment), либо не указывать ни одного из них. Если ничего не указано, применяется значение по умолчанию (1,1).  
  
## <a name="remarks"></a>Remarks  
 Столбцы идентификаторов можно использовать для формирования значений ключей. Свойство идентификаторов столбца гарантирует следующее.  
  
-   Каждое новое значение будет сформировано на основе текущих аргументов seed и increment.  
  
-   Каждое новое значение для определенной транзакции будет отлично от других параллельных транзакций для таблицы.  
  
 Свойство идентификаторов столбца не гарантирует следующее.  
  
-   **Уникальность значения** — уникальность значения следует обеспечить с помощью ограничения **PRIMARY KEY** или **UNIQUE** либо индекса **UNIQUE**. - 
 
> [!NOTE]
> Azure Synapse Analytics не поддерживает ограничения **PRIMARY KEY** или **UNIQUE** либо индекс **UNIQUE**. Дополнительные сведения см. в статье [Использование свойства IDENTITY для создания суррогатных ключей в пуле Synapse SQL](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-identity#what-is-a-surrogate-key).

-   **Последовательные значения в пределах транзакции** ― при вставке транзакцией нескольких строк не гарантируется, что для них будут назначены последовательные значения. Это связано с тем, что в таблице могут выполняться другие параллельные операции вставки. Если значения должны быть последовательными, то транзакция должна использовать монопольную блокировку для таблицы или уровень изоляции **SERIALIZABLE**.  
  
-   **Последовательные значения после перезапуска сервера или других ошибок** -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может сохранять значения идентификаторов в кэше для обеспечения высокой производительности, и некоторые из присвоенных значений могут быть потеряны при сбое базы данных или перезагрузке сервера. Это может вызвать пропуски в значениях идентификатора при вставке. Если пропуски недопустимы, приложение должно использовать собственный механизм для создания значений ключей. Использование генератора последовательностей с параметром **NOCACHE** может привести к ограничению пропусков в незафиксированных транзакциях.  
  
-   **Повторное использование значений** — свойства идентификаторов, созданные конкретным свойством идентификатора с заданными аргументами seed и increment, не используются повторно подсистемой. Если определенная инструкция вставки завершается с ошибкой или производится ее откат, использованные значения идентификаторов не будут созданы повторно. Это может привести к появлению пропусков при создании последующих значений идентификаторов.  
  
 Эти ограничения были созданы намеренно и предназначены для повышения производительности, поскольку они являются допустимыми во многих типичных ситуациях. Если из-за этих ограничений невозможно использовать значения идентификаторов, рекомендуется создать отдельную таблицу, содержащую текущее значение, управление доступом к которой и назначение чисел будет выполняться приложением.  
  
 Если таблица со столбцом идентификаторов опубликована для репликации, этот столбец должен обслуживаться в соответствии с типом репликации. Дополнительные сведения см. в статье [Репликация столбцов идентификаторов](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Для каждой таблицы можно создать только один столбец идентификаторов.  
  
 В таблицах, оптимизированных для памяти, в качестве начального значения и значения приращения должно быть задано 1,1. Установка для параметров seed или increment значения, отличного от 1, приводит к следующей ошибке: "Использование для параметров seed и increment значений, отличных от 1, не поддерживается в таблицах, оптимизированных для памяти".  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-the-identity-property-with-create-table"></a>A. Свойство IDENTITY в инструкции CREATE TABLE  
 В следующем примере производится создание новой таблицы со свойством `IDENTITY` для получения автоматически увеличивающегося идентификационного номера.  
  
```sql  
USE AdventureWorks2012;  
  
IF OBJECT_ID ('dbo.new_employees', 'U') IS NOT NULL  
   DROP TABLE new_employees;  
GO  
CREATE TABLE new_employees  
(  
 id_num int IDENTITY(1,1),  
 fname varchar (20),  
 minit char(1),  
 lname varchar(30)  
);  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Karin', 'F', 'Josephs');  
  
INSERT new_employees  
   (fname, minit, lname)  
VALUES  
   ('Pirkko', 'O', 'Koskitalo');  
```  
  
### <a name="b-using-generic-syntax-for-finding-gaps-in-identity-values"></a>Б. Базовый синтаксис для поиска промежутков в нумерации идентификаторов  
 Следующий пример демонстрирует базовый синтаксис для поиска разрывов в нумерации идентификаторов, возникающих при удалении данных.  
  
> [!NOTE]  
>  Первая часть данного скрипта [!INCLUDE[tsql](../../includes/tsql-md.md)] приведена только в учебных целях. Вы можете запустить скрипт [!INCLUDE[tsql](../../includes/tsql-md.md)], который начинается с комментария: `-- Create the img table`.  
  
```sql 
-- Here is the generic syntax for finding identity value gaps in data.  
-- The illustrative example starts here.  
SET IDENTITY_INSERT tablename ON;  
DECLARE @minidentval column_type;  
DECLARE @maxidentval column_type;  
DECLARE @nextidentval column_type;  
SELECT @minidentval = MIN($IDENTITY), @maxidentval = MAX($IDENTITY)  
    FROM tablename  
IF @minidentval = IDENT_SEED('tablename')  
   SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('tablename')  
   FROM tablename t1  
   WHERE $IDENTITY BETWEEN IDENT_SEED('tablename') AND   
      @maxidentval AND  
      NOT EXISTS (SELECT * FROM tablename t2  
         WHERE t2.$IDENTITY = t1.$IDENTITY +   
            IDENT_INCR('tablename'))  
ELSE  
   SELECT @nextidentval = IDENT_SEED('tablename');  
SET IDENTITY_INSERT tablename OFF;  
-- Here is an example to find gaps in the actual data.  
-- The table is called img and has two columns: the first column   
-- called id_num, which is an increasing identification number, and the   
-- second column called company_name.  
-- This is the end of the illustration example.  
  
-- Create the img table.  
-- If the img table already exists, drop it.  
-- Create the img table.  
IF OBJECT_ID ('dbo.img', 'U') IS NOT NULL  
   DROP TABLE img;  
GO  
CREATE TABLE img (id_num INT IDENTITY(1,1), company_name sysname);  
INSERT img(company_name) VALUES ('New Moon Books');  
INSERT img(company_name) VALUES ('Lucerne Publishing');  
-- SET IDENTITY_INSERT ON and use in img table.  
SET IDENTITY_INSERT img ON;  
  
DECLARE @minidentval SMALLINT;  
DECLARE @nextidentval SMALLINT;  
SELECT @minidentval = MIN($IDENTITY) FROM img  
 IF @minidentval = IDENT_SEED('img')  
    SELECT @nextidentval = MIN($IDENTITY) + IDENT_INCR('img')  
    FROM img t1  
    WHERE $IDENTITY BETWEEN IDENT_SEED('img') AND 32766 AND  
      NOT    EXISTS (SELECT * FROM img t2  
          WHERE t2.$IDENTITY = t1.$IDENTITY + IDENT_INCR('img'))  
 ELSE  
    SELECT @nextidentval = IDENT_SEED('img');  
SET IDENTITY_INSERT img OFF;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [DBCC CHECKIDENT (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [IDENT_INCR (Transact-SQL)](../../t-sql/functions/ident-incr-transact-sql.md)   
 [@@IDENTITY (Transact-SQL)](../../t-sql/functions/identity-transact-sql.md)   
 [IDENTITY (функция) (Transact-SQL)](../../t-sql/functions/identity-function-transact-sql.md)   
 [IDENT_SEED (Transact-SQL)](../../t-sql/functions/ident-seed-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [SET IDENTITY_INSERT (Transact-SQL)](../../t-sql/statements/set-identity-insert-transact-sql.md)   
 [Репликация столбцов идентификаторов](../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  

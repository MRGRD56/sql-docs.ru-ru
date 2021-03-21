---
description: OBJECT_ID (Transact-SQL)
title: OBJECT_ID (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_ID
- OBJECT_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], IDs
- identification numbers [SQL Server], database objects
- checking object exists
- IDs [SQL Server], database objects
- OBJECT_ID function
- database objects [SQL Server], IDs
- displaying object IDs
- viewing object IDs
- verifying object exists
ms.assetid: f89286db-440f-4218-a828-30881ce3077a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6039bcbf036093af3fcb3c22e7a3845afb485574
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735100"
---
# <a name="object_id-transact-sql"></a>OBJECT_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает идентификационный номер объекта базы данных для объекта области схемы.  
  
> [!IMPORTANT]  
>  Запросы на объекты, не относящиеся к области схемы, например триггеры DDL, не могут выполняться с использованием OBJECT_ID. Для объектов, не найденных в представлении каталога [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md), идентификационный номер проставляется с помощью запроса к соответствующему представлению каталога. Например, чтобы получить идентификационный номер триггера DDL, используйте инструкцию `SELECT OBJECT_ID FROM sys.triggers WHERE name = 'DatabaseTriggerLog``'`.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
OBJECT_ID ( '[ database_name . [ schema_name ] . | schema_name . ]   
  object_name' [ ,'object_type' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 **'** *object_name* **'**  
 Объект, который должен использоваться. Аргумент *object_name* имеет тип **varchar** или **nvarchar**. Если значение *object_name* имеет тип **varchar**, оно неявно преобразовывается в тип **nvarchar**. Не обязательно указывать имена базы данных и схемы.  
  
 **'** *object_type* **'**  
 Объект области схемы. Аргумент *object_type* имеет тип **varchar** или **nvarchar**. Если значение *object_type* имеет тип **varchar**, оно неявно преобразовывается в тип **nvarchar**. Список типов объектов см. в столбце **type** представления [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
## <a name="exceptions"></a>Исключения  
 Для пространственного индекса функция OBJECT_ID возвращает значение NULL.  
  
 В случае ошибки возвращает значение NULL.  
  
 Пользователь может просматривать только метаданные защищаемых объектов, которыми он владеет или на которые пользователю были предоставлены разрешения. Это означает, что встроенные функции, создающие метаданные, такие как OBJECT_ID, могут вернуть значение NULL в случае, если пользователь не имеет разрешений на объект. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="remarks"></a>Комментарии  
 Если параметр системной функции является необязательным, то предполагаются текущие база данных, главный компьютер, пользователь сервера или пользователь базы данных. За встроенными функциями всегда должны следовать круглые скобки.  
  
 Если указано имя временной таблицы, то имя базы данных должно стоять перед именем временной таблицы, если только текущей не является база данных **tempdb**. Например: `SELECT OBJECT_ID('tempdb..#mytemptable')`.  
  
 Системные функции можно использовать в списке выбора, в предложении WHERE и в любом месте, где разрешается использование выражений. Дополнительные сведения см. в статьях [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md) и [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-the-object-id-for-a-specified-object"></a>A. Получение идентификатора указанного объекта  
 Следующий пример возвращает идентификатор объекта для таблицы `Production.WorkOrder` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE master;  
GO  
SELECT OBJECT_ID(N'AdventureWorks2012.Production.WorkOrder') AS 'Object ID';  
GO  
```  
  
### <a name="b-verifying-that-an-object-exists"></a>Б. Проверка существования объекта  
 Следующий пример проверяет существование указанной таблицы, проверяя наличие у таблицы идентификатора объекта. Если таблица существует, то она удаляется. Если таблица не существует, то инструкция `DROP TABLE` не выполняется.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID (N'dbo.AWBuildVersion', N'U') IS NOT NULL  
DROP TABLE dbo.AWBuildVersion;  
GO  
```  
  
### <a name="c-using-object_id-to-specify-the-value-of-a-system-function-parameter"></a>В. Использование функции OBJECT_ID для указания параметра системной функции  
 В приведенном ниже примере возвращаются сведения для всех индексов и секций таблицы `Person.Address` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] с помощью функции [sys.dm_db_index_operational_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md).  
 
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
> [!IMPORTANT]  
>  При использовании для возврата значений параметров функций DB_ID и OBJECT_ID языка [!INCLUDE[tsql](../../includes/tsql-md.md)] необходимо убедиться в допустимости возвращаемого идентификатора. Если имя базы данных или объекта не может быть найдено, например если база данных или объект не существуют или неправильно записаны, то обе функции возвратят значение NULL. Функция **sys.dm_db_index_operational_stats** интерпретирует значение NULL как значение шаблона, указывающего все базы данных или все объекты. Так как эта операция может быть непреднамеренной, примеры в этом разделе демонстрируют безопасный способ определения идентификаторов базы данных и объекта.
  
```sql  
DECLARE @db_id INT;  
DECLARE @object_id INT;  
SET @db_id = DB_ID(N'AdventureWorks2012');  
SET @object_id = OBJECT_ID(N'AdventureWorks2012.Person.Address');  
IF @db_id IS NULL   
  BEGIN;  
    PRINT N'Invalid database';  
  END;  
ELSE IF @object_id IS NULL  
  BEGIN;  
    PRINT N'Invalid object';  
  END;  
ELSE  
  BEGIN;  
    SELECT * FROM sys.dm_db_index_operational_stats(@db_id, @object_id, NULL, NULL);  
  END;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-returning-the-object-id-for-a-specified-object"></a>Г. Получение идентификатора указанного объекта  
 Следующий пример возвращает идентификатор объекта для таблицы `FactFinance` в базе данных [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].  
  
```sql  
SELECT OBJECT_ID('AdventureWorksPDW2012.dbo.FactFinance') AS 'Object ID';  
```  
  
## <a name="see-also"></a>См. также  
 [Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_NAME (Transact-SQL)](../../t-sql/functions/object-name-transact-sql.md)  
  
  


---
description: OBJECT_NAME (Transact-SQL)
title: OBJECT_NAME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OBJECT_NAME
- OBJECT_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OBJECT_NAME function
- viewing database object names
- objects [SQL Server], names
- database objects [SQL Server], names
- displaying database object names
- database objects [SQL Server]
- names [SQL Server], database objects
ms.assetid: 7d5b923f-0c3e-4af9-b39b-132807a6d5b3
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2aec001f05dcc4e90c251298235e158e64c36b3d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734687"
---
# <a name="object_name-transact-sql"></a>OBJECT_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает имя объекта базы данных в области схемы. Список объектов области схемы см. в статье [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
OBJECT_NAME ( object_id [, database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *object_id*  
 Идентификатор объекта, который будет использован. Аргумент *object_id* имеет тип **int**. Предполагается, что это объект в области схемы указанной базы данных или в контексте текущей базы данных.  
  
 *database_id*  
 Идентификатор базы данных, где будет выполняться поиск объекта. Аргумент *database_id* имеет тип **int**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **sysname**  
  
## <a name="exceptions"></a>Исключения  
 Возвращает значение NULL в случае ошибки или если участник не имеет разрешений для просмотра объекта. Если параметр AUTO_CLOSE целевой базы данных имеет значение ON, то функция откроет базу данных.  
  
 Пользователь может просматривать только метаданные защищаемых объектов, которыми он владеет или на которые пользователю были предоставлены разрешения. Это означает, что встроенные функции, создающие метаданные, такие как OBJECT_NAME, могут вернуть значение NULL в случае, если пользователь не имеет разрешений на объект. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ANY на данный объект. Чтобы указать идентификатор базы данных, также требуется разрешение CONNECT на базу данных или необходимо включить гостевую учетную запись.  
  
## <a name="remarks"></a>Комментарии  
 Системные функции можно использовать в списке выбора, в предложении WHERE и в любом месте, где разрешается использование выражений. Дополнительные сведения см. в разделах [Выражения](../../t-sql/language-elements/expressions-transact-sql.md) и [WHERE](../../t-sql/queries/where-transact-sql.md).  
  
 Значение, возвращаемое системной функцией, использует параметры сортировки текущей базы данных.  
  
 По умолчанию компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] предполагает, что идентификатор *object_id* находится в контексте текущей базы данных. Запрос, ссылающийся на *object_id* в другой базе данных, возвращает значение NULL или неправильные результаты. Например, в следующем запросе текущий контекст базы данных — база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] пытается возвратить имя объекта для заданного идентификатора объекта в этой базе данных, а не в базе данных, указанной в предложении FROM запроса. Поэтому возвращаются неверные сведения.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT OBJECT_NAME(object_id)  
FROM master.sys.objects;  
GO  
```  
  
 Можно разрешать имена объектов в контексте другой базы данных, указав идентификатор базы данных. Следующий пример указывает идентификатор базы данных для базы данных `master` в базе данных в функции `OBJECT_SCHEMA_NAME` и приводит к правильным результатам.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT OBJECT_SCHEMA_NAME(object_id, 1) AS schema_name  
FROM master.sys.objects;  
GO  
```  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-object_name-in-a-where-clause"></a>A. Использование параметра OBJECT_NAME в предложении WHERE  
 Следующий пример возвращает столбцы из представления каталога `sys.objects` для объекта, указанного параметром `OBJECT_NAME` в предложении `WHERE` инструкции `SELECT`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyID INT;  
SET @MyID = (SELECT OBJECT_ID('AdventureWorks2012.Production.Product',  
    'U'));  
SELECT name, object_id, type_desc  
FROM sys.objects  
WHERE name = OBJECT_NAME(@MyID);  
GO  
```  
  
### <a name="b-returning-the-object-schema-name-and-object-name"></a>Б. Возвращение имени схемы объекта и имени объекта  
 Следующий пример возвращает имя схемы объекта, имя объекта и SQL-текст для всех кэшированных планов запроса, которые не являются нерегламентированными или подготовленными инструкциями.  
  
```sql  
SELECT DB_NAME(st.dbid) AS database_name,   
    OBJECT_SCHEMA_NAME(st.objectid, st.dbid) AS schema_name,  
    OBJECT_NAME(st.objectid, st.dbid) AS object_name,   
    st.text AS query_text  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
WHERE st.objectid IS NOT NULL;  
GO  
```  
  
### <a name="c-returning-three-part-object-names"></a>В. Возвращение трехсоставных имен объекта  
 Следующий пример возвращает имя базы данных, схемы и объекта вместе со всеми остальными столбцами в динамическом административном представлении `sys.dm_db_index_operational_stats` для всех объектов во всех базах данных.  
  
```sql  
SELECT QUOTENAME(DB_NAME(database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_SCHEMA_NAME(object_id, database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_NAME(object_id, database_id))  
    , *   
FROM sys.dm_db_index_operational_stats(NULL, NULL, NULL, NULL);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-object_name-in-a-where-clause"></a>Г. Использование параметра OBJECT_NAME в предложении WHERE  
 Следующий пример возвращает столбцы из представления каталога `sys.objects` для объекта, указанного параметром `OBJECT_NAME` в предложении `WHERE` инструкции `SELECT`. (В вашем случае номер объекта (274100017 в примере ниже) будет иным.  Чтобы выполнить пример, найдите допустимый номер объекта, выполнив `SELECT name, object_id FROM sys.objects;` в базе данных.)  
  
```sql  
SELECT name, object_id, type_desc  
FROM sys.objects  
WHERE name = OBJECT_NAME(274100017);  
```  
  
## <a name="see-also"></a>См. также  
 [Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID (Transact-SQL)](../../t-sql/functions/object-id-transact-sql.md)  
  
  


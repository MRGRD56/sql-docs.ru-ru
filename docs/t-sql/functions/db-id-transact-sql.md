---
description: DB_ID (Transact-SQL)
title: DB_ID (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/13/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DB_ID_TSQL
- DB_ID
dev_langs:
- TSQL
helpviewer_keywords:
- viewing database ID numbers
- IDs [SQL Server], databases
- database IDs [SQL Server]
- identification numbers [SQL Server], databases
- displaying database ID numbers
- DB_ID function
ms.assetid: 7b3aef89-a6fd-4144-b468-bf87ebf381b8
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 77ee79ff9c1090872030391a2b029f2411b0cdac
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752084"
---
# <a name="db_id-transact-sql"></a>DB_ID (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает идентификационный номер указанной базы данных.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DB_ID ( [ 'database_name' ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
'*database_name*'  
Имя базы данных, идентификационный номер которой вернет функция `DB_ID`. Если в вызове `DB_ID` аргумент *database_name* не указан, функция `DB_ID` возвращает идентификатор текущей базы данных.
  
## <a name="return-types"></a>Типы возвращаемых данных
**int**

## <a name="remarks"></a>Remarks
`DB_ID` может использоваться только для возврата идентификатора текущей базы данных в Базе данных SQL Azure. Если указанное имя базы данных отличается от текущей базы данных, возвращается значение NULL.

> [!NOTE]
> При использовании с Базой данных SQL Azure `DB_ID` может вернуть не такой результат, как при запросе `database_id` из представления **sys.databases**. Если объект, вызывающий `DB_ID`, сравнивает результат с другими представлениями **sys**, то вместо этого следует отправить запрос к представлению **sys. databases**.
  
## <a name="permissions"></a>Разрешения  
Если участник, вызывающий `DB_ID`, не является владельцем конкретной базы данных, отличной от базы данных **master** или **tempdb**, то минимальными разрешениями, необходимыми для просмотра соответствующей строки `DB_ID`, являются разрешения уровня сервера `ALTER ANY DATABASE` или `VIEW ANY DATABASE`. Для базы данных **master** функция `DB_ID` требует по крайней мере разрешения `CREATE DATABASE`. База данных, к которой подключается вызывающий участник, всегда отображается в представлении **sys.databases**.
  
> [!IMPORTANT]  
>  По умолчанию общедоступная роль имеет разрешение `VIEW ANY DATABASE`, что позволяет всем именам для входа просматривать информацию в базе данных. Чтобы имя для входа не могло обнаруживать базу данных, отзовите общедоступное разрешение `VIEW ANY DATABASE` с помощью инструкции `REVOKE` или отмените разрешение `VIEW ANY DATABASE` для отдельных имен для входа с помощью инструкции `DENY`.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-the-database-id-of-the-current-database"></a>A. Возвращение идентификатора текущей базы данных  
В приведенном ниже примере возвращается идентификатор текущей базы данных.
  
```sql
SELECT DB_ID() AS [Database ID];  
GO  
```  
  
### <a name="b-returning-the-database-id-of-a-specified-database"></a>Б. Возвращение идентификатора указанной базы данных  
В приведенном ниже примере возвращается идентификатор базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].
  
```sql
SELECT DB_ID(N'AdventureWorks2008R2') AS [Database ID];  
GO  
```  
  
### <a name="c-using-db_id-to-specify-the-value-of-a-system-function-parameter"></a>В. Использование DB_ID для указания значения параметра системной функции  
В приведенном ниже примере функция `DB_ID` используется для получения идентификатора базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] в системной функции `sys.dm_db_index_operational_stats`. Эта функция принимает идентификатор базы данных в качестве первого параметра.
  
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
  
### <a name="d-return-the-id-of-the-current-database"></a>Г. Получение идентификатора текущей базы данных  
В приведенном ниже примере возвращается идентификатор текущей базы данных.
  
```sql
SELECT DB_ID();  
```  
  
### <a name="e-return-the-id-of-a-named-database"></a>Д. Получение идентификатора именованной базы данных  
В приведенном ниже примере возвращается идентификатор базы данных AdventureWorksDW2012.
  
```sql
SELECT DB_ID('AdventureWorksPDW2012');  
```  
  
## <a name="see-also"></a>См. также
[DB_NAME (Transact-SQL)](../../t-sql/functions/db-name-transact-sql.md)  
[Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
[sys.dm_db_index_operational_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)
  
  


---
description: DB_NAME (Transact-SQL)
title: DB_NAME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DB_NAME
- DB_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database names [SQL Server], DB_NAME
- names [SQL Server], databases
- viewing database names
- displaying database names
- DB_NAME function
ms.assetid: e21fb33a-a3ea-49b0-bb6b-8f789a675a0e
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 111827a33c9e7b65a729f09616ce863e1c658cf4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752054"
---
# <a name="db_name-transact-sql"></a>DB_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает имя указанной базы данных.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DB_NAME ( [ database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*database_id*  

Идентификационный номер базы данных, имя которой вернет функция `DB_NAME`. Если в вызове `DB_NAME` аргумент *database_id* не указан, функция `DB_NAME` возвращает имя текущей базы данных.
  
## <a name="return-types"></a>Типы возвращаемых данных
**nvarchar(128)**
  
## <a name="permissions"></a>Разрешения  

Если участник, вызывающий `DB_NAME`, не является владельцем конкретной базы данных, отличной от базы данных **master** или **tempdb**, то минимальными разрешениями, необходимыми для просмотра соответствующей строки `DB_ID`, являются разрешения уровня сервера `ALTER ANY DATABASE` или `VIEW ANY DATABASE`. Для базы данных **master** функция `DB_ID` требует по крайней мере разрешения `CREATE DATABASE`. База данных, к которой подключается вызывающий участник, всегда отображается в представлении **sys.databases**.
  
> [!IMPORTANT]  
>  По умолчанию общедоступная роль имеет разрешение `VIEW ANY DATABASE`, что позволяет всем именам для входа просматривать информацию в базе данных. Чтобы имя для входа не могло обнаруживать базу данных, отзовите общедоступное разрешение `VIEW ANY DATABASE` с помощью инструкции `REVOKE` или отмените разрешение `VIEW ANY DATABASE` для отдельных имен для входа с помощью инструкции `DENY`.
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-the-current-database-name"></a>A. Возврат имени текущей базы данных  
В приведенном ниже примере возвращается имя текущей базы данных.
  
```sql
SELECT DB_NAME() AS [Current Database];  
GO  
```  
  
### <a name="b-returning-the-database-name-of-a-specified-database-id"></a>Б. Возврат имени базы данных с указанным идентификатором базы данных  
В приведенном ниже примере возвращается имя базы данных с идентификатором `3`.
  
```sql
USE master;  
GO  
SELECT DB_NAME(3) AS [Database Name];  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-return-the-current-database-name"></a>В. Получение имени текущей базы данных  
  
```sql
SELECT DB_NAME() AS [Current Database];  
```  
  
### <a name="d-return-the-name-of-a-database-by-using-the-database-id"></a>Г. Получение имени базы данных по ее идентификатору  
В приведенном ниже примере возвращаются имя и идентификатор каждой базы данных.
  
```sql
SELECT DB_NAME(database_id) AS [Database], database_id  
FROM sys.databases;  
```  
  
## <a name="see-also"></a>См. также
[DB_ID (Transact-SQL)](../../t-sql/functions/db-id-transact-sql.md)  
[Функции метаданных (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)  
[sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
  
  


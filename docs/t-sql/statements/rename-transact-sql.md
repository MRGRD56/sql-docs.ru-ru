---
description: RENAME (Transact-SQL)
title: RENAME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/21/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
ms.assetid: 0907cfd9-33a6-4fa6-91da-7d6679fee878
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 70c72d0d9b884431a314290de4c4514c720e85ee
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186486"
---
# <a name="rename-transact-sql"></a>RENAME (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

Переименовывает созданную пользователем таблицу в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Переименовывает созданную пользователем таблицу, столбец в созданной пользователем таблице или базе данных в [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].

> [!NOTE]
> Чтобы переименовать базу данных в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], используйте [ALTER DATABASE ([!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)])](alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true). Чтобы переименовать базу данных в службе базы данных SQL Azure, используйте инструкцию [ALTER DATABASE (база данных SQL Azure)](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true). Чтобы переименовать базу данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], используйте хранимую процедуру [sp_renamedb](../../relational-databases/system-stored-procedures/sp-renamedb-transact-sql.md).

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for Azure Synapse Analytics

-- Rename a table.
RENAME OBJECT [::] [ [ database_name . [schema_name ] ] . ] | [schema_name . ] ] table_name TO new_table_name
[;]

```

```syntaxsql
-- Syntax for Analytics Platform System

-- Rename a table
RENAME OBJECT [::] [ [ database_name . [ schema_name ] . ] | [ schema_name . ] ] table_name TO new_table_name
[;]

-- Rename a database
RENAME DATABASE [::] database_name TO new_database_name
[;]

-- Rename a column 
RENAME OBJECT [::] [ [ database_name . [schema_name ] ] . ] | [schema_name . ] ] table_name COLUMN column_name TO new_column_name [;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Аргументы

RENAME OBJECT [::] [ [*database_name* . [ *schema_name* ] . ] | [ *schema_name* . ] ]*table_name* TO *new_table_name* — 
**применимо к**[!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Изменение имени определяемой пользователем таблицы. Указание таблицы для переименования с именем, состоящим из одной, двух или трех частей. Указание *имени_новой_таблицы*, состоящего из одной части.

RENAME DATABASE [::] [ *имя_базы_данных* TO *новое_имя_базы_данных* — 
**применимо к** [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Измените имя пользовательской базы данных с *имени_базы_данных* на *новое_имя_базы_данных*. Недопустимо переименование в следующие [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]зарезервированные имена баз данных.

- master
- model
- msdb
- tempdb
- pdwtempdb1
- pdwtempdb2
- DWConfiguration
- DWDiagnostics
- DWQueue


RENAME OBJECT [::] [ [*database_name* . [ *schema_name* ] . ] | [ *schema_name* . ] ]*table_name* COLUMN *column_name* TO *new_column_name*
**Область применения:** [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Изменение имени столбца в таблице. 

## <a name="permissions"></a>Разрешения

Для выполнения этой команды требуется следующее разрешение:

- Разрешение **ALTER** на таблицу

## <a name="limitations-and-restrictions"></a>Ограничения

### <a name="cannot-rename-an-external-table-indexes-or-views"></a>Невозможно переименовать внешние таблицы, индексы и представления

Вы не можете переименовать внешние таблицы, индексы и представления. Вместо переименования можно удалить внешнюю таблицу, индекс или представление и затем создать этот объект повторно с новым именем.

### <a name="cannot-rename-a-table-in-use"></a>Невозможно переименовать используемую таблицу

Вы не можете переименовать таблицу или базу данных во время использования. Для переименования таблицы требуется монопольная блокировка таблицы. Если таблица используется, может потребоваться завершить сеансы, которые используют таблицу. Для завершения сеанса можно использовать команду KILL. Используйте инструкцию KILL осторожно, так как при завершении сеанса для всей незафиксированной работы будет выполнен откат. Сеансы в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] имеют префикс "SID". Префикс "SID" и номер сеанса необходимо указать при вызове команды KILL. В этом примере мы получаем список активных или неактивных сеансов и затем завершаем сеанс "SID1234".

### <a name="rename-column-restrictions"></a>Ограничения переименования столбцов

Вы не можете переименовать столбец, используемый для распределения таблицы. Кроме того, невозможно переименовывать столбцы во внешней или временной таблице. 

### <a name="views-are-not-updated"></a>Представления не обновляются

При переименовании базы данных все представления, в которых используется предыдущее имя базы данных, станут недействительными. Это поведение относится к представлениям как внутри, так и вне базы данных. Например, при переименовании базы данных Sales представления, содержащие `SELECT * FROM Sales.dbo.table1`, станут недействительными. Чтобы устранить эту проблему, старайтесь не использовать имена из трех частей в представлениях или обновите представления так, чтобы в них использовалось новое имя базы данных.

При переименовании таблицы обновления имени таблицы в представлениях не происходит. Все представления внутри или вне базы данных, которые ссылаются на предыдущее имя таблицы, станут недействительными. Чтобы устранить эту проблему, обновите представления так, чтобы в них использовалось новое имя базы данных.

При переименовании столбца обновление представлений для их ссылки на это новое имя столбца не выполняется. Представления продолжат отображать старое имя столбца до выполнения инструкции ALTER VIEW. В некоторых случаях представления могут стать недействительными, в результате чего потребуется удалить их и создать заново.

## <a name="locking"></a>Блокировка

Для переименования таблицы необходима совмещаемая блокировка для объекта базы данных, совмещаемая блокировка для объекта СХЕМЫ и монопольная блокировка таблицы.

## <a name="examples"></a>Примеры

### <a name="a-rename-a-database"></a>A. Переименование базы данных

**Применимо к** только [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В этом примере мы переименовываем пользовательскую базу данных AdWorks в AdWorks2.

```sql
-- Rename the user defined database AdWorks
RENAME DATABASE AdWorks to AdWorks2;

```

 При переименовании таблицы все объекты и свойства, связанные с этой таблицей, обновляются, так чтобы в них использовалось новое имя таблицы. Например, обновляются определения таблиц, индексы, ограничения и разрешения. Представления не обновляются.

### <a name="b-rename-a-table"></a>Б. Переименование таблицы

**Применимо к**: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В этом примере мы переименовываем таблицу Customer в Customer1.

```sql
-- Rename the customer table
RENAME OBJECT Customer TO Customer1;

RENAME OBJECT mydb.dbo.Customer TO Customer1;
```

При переименовании таблицы все объекты и свойства, связанные с этой таблицей, обновляются, так чтобы в них использовалось новое имя таблицы. Например, обновляются определения таблиц, индексы, ограничения и разрешения. Представления не обновляются.

### <a name="c-move-a-table-to-a-different-schema"></a>В. Перемещение таблицы в другую схему

**Применимо к**: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Если вы хотите переместить объект в другую схему, используйте инструкцию [ALTER SCHEMA](../../t-sql/statements/alter-schema-transact-sql.md). Например, следующая инструкция перемещает элемент таблицы из схемы product в схему dbo.

```sql
ALTER SCHEMA dbo TRANSFER OBJECT::product.item;
```

### <a name="d-terminate-sessions-before-renaming-a-table"></a>Г. Завершение сеансов перед переименованием таблицы

**Применимо к**: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Переименовать таблицу, которая сейчас используется, невозможно. Для переименования таблицы требуется монопольная блокировка таблицы. Если таблица используется, может потребоваться завершить сеансы, которые используют таблицу. Для завершения сеанса можно использовать команду KILL. Используйте инструкцию KILL осторожно, так как при завершении сеанса для всей незафиксированной работы будет выполнен откат. Сеансы в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] имеют префикс "SID". Префикс "SID" и номер сеанса потребуется указать при вызове команды KILL. В этом примере мы получаем список активных или неактивных сеансов и затем завершаем сеанс "SID1234".

```sql
-- View a list of the current sessions
SELECT session_id, login_name, status
FROM sys.dm_pdw_exec_sessions
WHERE status='Active' OR status='Idle';

-- Terminate a session using the session_id.
KILL 'SID1234';
```

### <a name="e-rename-a-column"></a>Д. Переименование столбца 

**Область применения:** [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

В этом примере столбец FName таблицы Customer переименовывается в FirstName.

```sql
-- Rename the Fname column of the customer table
RENAME OBJECT::Customer COLUMN FName TO FirstName;

RENAME OBJECT mydb.dbo.Customer COLUMN FName TO FirstName;
```

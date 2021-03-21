---
description: USER (Transact-SQL)
title: USER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER
- USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- system-supplied usernames [SQL Server]
- USER function
- users [SQL Server], database names
- names [SQL Server], database users
- database usernames [SQL Server]
ms.assetid: 82bbbd94-870c-4c43-9ed9-d9abc767a6be
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3cf9ed38befaffa3a717336c38dc8e59f543eb5b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750494"
---
# <a name="user-transact-sql"></a>USER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Позволяет вставлять в таблицу предоставляемое системой имя текущего пользователя базы данных, если не указано значение по умолчанию.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
USER  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 **nvarchar(128)**  
  
## <a name="remarks"></a>Комментарии  
 Функция USER выполняет то же действие, что и системная функция USER_NAME.  
  
 Функцию USER можно указывать в ограничениях DEFAULT инструкций CREATE TABLE и ALTER TABLE либо использовать ее как любую другую стандартную функцию.  
  
 Функция USER всегда возвращает имя текущего контекста. При вызове после инструкции EXECUTE AS функция USER возвращает имя олицетворяемого контекста.  
  
 Если участник Windows производит доступ к базе данных посредством членства в группе, функция USER возвращает имя участника Windows, а не имя группы.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-user-to-return-the-database-user-name"></a>A. Получение имени пользователя базы данных функцией USER  
 В следующем примере объявляется переменная типа `char`, ей присваивается текущее значение функции USER, а затем производится печать этой переменной вместе с текстовым описанием.  
  
```sql
DECLARE @usr CHAR(30)  
SET @usr = user  
SELECT 'The current user''s database username is: '+ @usr  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
-----------------------------------------------------------------------  
The current user's database username is: dbo  
  
(1 row(s) affected)
```  
  
### <a name="b-using-user-with-default-constraints"></a>Б. Применение функции USER в ограничении DEFAULT  
 В следующем примере создается таблица с использованием функции `USER` в ограничении `DEFAULT` для столбца менеджеров по продажам.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE inventory22  
(  
 part_id INT IDENTITY(100, 1) NOT NULL,  
 description VARCHAR(30) NOT NULL,  
 entry_person VARCHAR(30) NOT NULL DEFAULT USER   
)  
GO  
INSERT inventory22 (description)  
VALUES ('Red pencil')  
INSERT inventory22 (description)  
VALUES ('Blue pencil')  
INSERT inventory22 (description)  
VALUES ('Green pencil')  
INSERT inventory22 (description)  
VALUES ('Black pencil')  
INSERT inventory22 (description)  
VALUES ('Yellow pencil')  
GO  
```  
  
 Ниже приведен запрос для получения всех данных из таблицы `inventory22`:  
  
```sql
SELECT * FROM inventory22 ORDER BY part_id;  
GO  
```  
  
 Результирующий набор (обратите внимание на значение столбца `entry-person`):  
  
 ```
part_id     description                    entry_person
----------- ------------------------------ -------------------------
100         Red pencil                     dbo
101         Blue pencil                    dbo
102         Green pencil                   dbo
103         Black pencil                   dbo
104         Yellow pencil                  dbo
  
(5 row(s) affected)
```  
  
### <a name="c-using-user-in-combination-with-execute-as"></a>В. Применение функции USER в сочетании с предложением EXECUTE AS  
 Следующий пример иллюстрирует работу функции `USER` при вызове из сеанса олицетворения.  
  
```sql
SELECT USER;  
GO  
EXECUTE AS USER = 'Mario';  
GO  
SELECT USER;  
GO  
REVERT;  
GO  
SELECT USER;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO
Mario
DBO
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP (Transact-SQL)](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER (Transact-SQL)](../../t-sql/functions/current-user-transact-sql.md)   
 [Функции безопасности (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)   
 [SESSION_USER (Transact-SQL)](../../t-sql/functions/session-user-transact-sql.md)   
 [SYSTEM_USER (Transact-SQL)](../../t-sql/functions/system-user-transact-sql.md)   
 [USER_NAME (Transact-SQL)](../../t-sql/functions/user-name-transact-sql.md)  
  
  


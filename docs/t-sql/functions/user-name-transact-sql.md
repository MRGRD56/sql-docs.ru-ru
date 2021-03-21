---
description: USER_NAME (Transact-SQL)
title: USER_NAME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- USER_NAME
- USER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- IDs [SQL Server], databases
- USER_NAME function
- users [SQL Server], database username
- names [SQL Server], database users
- identification numbers [SQL Server], databases
- database usernames [SQL Server]
ms.assetid: ab32d644-4228-449a-9ef0-5a975c305775
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b794ed88f9375a0b7f018db72c43a0406f7651c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756404"
---
# <a name="user_name-transact-sql"></a>USER_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает имя пользователя базы данных по указанному идентификационному номеру.  
  
 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на статью") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
USER_NAME ( [ id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *идентификатор*  
 Номер идентификатора, сопоставленный с пользователем базы данных. Аргумент *id* имеет тип **int**. Необходимо поставить скобки.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Комментарии  
 Когда аргумент *id* не указывается, подразумевается контекст текущего пользователя. Если параметр содержит слово NULL, то возвращается NULL. При использовании функции USER_NAME без указания *id* после инструкции EXECUTE AS функция USER_NAME возвращает имя олицетворенного пользователя. Если пользователь Windows попытается получить доступ к базе данных в качестве члена группы, функция USER_NAME вернет имя этого пользователя, а не имя группы.  
 
> [!NOTE]
> Хотя функция USER_NAME поддерживается в базе данных SQL Azure, использование *Выполнить как* с USER_NAME не поддерживается в базе данных SQL Azure. 
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-user_name"></a>A. Использование USER_NAME.  
 Следующий пример возвращает имя пользователя по его идентификатору `13`.  
  
```sql  
SELECT USER_NAME(13);  
GO  
```  
  
### <a name="b-using-user_name-without-an-id"></a>Б. Использование USER_NAME без идентификатора  
 Следующий пример демонстрирует поиск имени текущего пользователя без указания его идентификатора.  
  
```sql  
SELECT USER_NAME();  
GO  
```  
  
 Далее приведен результирующий набор для пользователя, который является членом предопределенной роли сервера sysadmin.  
  
 ```
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="c-using-user_name-in-the-where-clause"></a>В. Использование USER_NAME в предложении WHERE  
 Следующий пример иллюстрирует поиск в таблице `sysusers` строки, имя которой равняется результату работы системной функции `USER_NAME` для пользователя с идентификационным номером, равным `1`.  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
name  
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="d-calling-user_name-during-impersonation-with-execute-as"></a>Г. Вызов USER_NAME во время олицетворения пользователя с помощью EXECUTE AS  
 Следующий пример показывает, как `USER_NAME` ведет себя во время олицетворения пользователя.  
  
```sql  
SELECT USER_NAME();  
GO  
EXECUTE AS USER = 'Zelig';  
GO  
SELECT USER_NAME();  
GO  
REVERT;  
GO  
SELECT USER_NAME();  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO  
Zelig  
DBO
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-user_name-without-an-id"></a>Д. Использование USER_NAME без идентификатора  
 Следующий пример демонстрирует поиск имени текущего пользователя без указания его идентификатора.  
  
```sql  
SELECT USER_NAME();  
```  
  
 Ниже приведен результирующий набор для пользователя, вошедшего в систему.  
  
```  
------------------------------   
User7                              
```  
  
### <a name="f-using-user_name-in-the-where-clause"></a>Е. Использование USER_NAME в предложении WHERE  
 Следующий пример иллюстрирует поиск в таблице `sysusers` строки, имя которой равняется результату работы системной функции `USER_NAME` для пользователя с идентификационным номером, равным `1`.  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
name                             
------------------------------   
User7                              
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP (Transact-SQL)](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER (Transact-SQL)](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER (Transact-SQL)](../../t-sql/functions/session-user-transact-sql.md)   
 [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SYSTEM_USER (Transact-SQL)](../../t-sql/functions/system-user-transact-sql.md)  
  

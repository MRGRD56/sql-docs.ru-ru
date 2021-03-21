---
description: SYSTEM_USER (Transact-SQL)
title: SYSTEM_USER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SYSTEM_USER_TSQL
- SYSTEM_USER
dev_langs:
- TSQL
helpviewer_keywords:
- current user names
- system-supplied user names [SQL Server]
- users [SQL Server], logins
- logins [SQL Server], identification name
- current system user names
- SYSTEM_USER function
- inserting system user name into table
- system usernames [SQL Server]
- users [SQL Server], names
ms.assetid: 565984cd-60c6-4df7-83ea-2349b838ccb2
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8a718ab0e86ed0fa8bafdca88968760a3c103cca
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754504"
---
# <a name="system_user-transact-sql"></a>SYSTEM_USER (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Позволяет вставить в таблицу значение текущего имени входа, назначенного системой, если не задано значение по умолчанию.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SYSTEM_USER  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных  
 **nvarchar(128)**  
  
## <a name="remarks"></a>Комментарии  
 Функцию SYSTEM_USER можно использовать с ограничениями DEFAULT в инструкциях CREATE TABLE и ALTER TABLE. Она также может использоваться как стандартная функция.  
  
 Если имя пользователя и имя входа различаются, функция SYSTEM_USER возвращает имя входа.  
  
 Если текущий пользователь подключается к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], используя проверку подлинности Windows, функция SYSTEM_USER возвращает идентификационное имя входа в Windows в следующем формате: *ДОМЕН*\\*имя_входа_пользователя*. Однако, если текущий пользователь подключается к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с проверкой подлинности SQL Server, функция SYSTEM_USER возвращает имя входа в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], например `WillisJo`, для пользователя, подключившегося как `WillisJo`.  
  
 Функция SYSTEM_USER возвращает имя текущего выполняемого контекста. Если контекст был переключен с помощью инструкции EXECUTE AS, функция SYSTEM_USER возвращает имя олицетворяемого контекста.  

 Использовать EXECUTE AS от имени SYSTEM_USER невозможно.

## <a name="examples"></a>Примеры  
  
### <a name="a-using-system_user-to-return-the-current-system-user-name"></a>A. Использование функции SYSTEM_USER для получения текущего системного имени пользователя  
 В следующем примере объявляется переменная `char`, в которой сохраняется текущее значение `SYSTEM_USER`, а затем значение этой переменной выводится на печать.  
  
```sql
DECLARE @sys_usr CHAR(30);  
SET @sys_usr = SYSTEM_USER;  
SELECT 'The current system user is: '+ @sys_usr;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
----------------------------------------------------------
The current system user is: WillisJo

(1 row(s) affected)
 ```  
  
### <a name="b-using-system_user-with-default-constraints"></a>Б. Использование функции SYSTEM_USER с ограничениями DEFAULT  
 В следующем примере создается таблица с функцией `SYSTEM_USER` в качестве ограничения `DEFAULT` для столбца `SRep_tracking_user`.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE Sales.Sales_Tracking  
(  
    Territory_id INT IDENTITY(2000, 1) NOT NULL,  
    Rep_id INT NOT NULL,  
    Last_sale DATETIME NOT NULL DEFAULT GETDATE(),  
    SRep_tracking_user VARCHAR(30) NOT NULL DEFAULT SYSTEM_USER  
);  
GO  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (151);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (293, '19980515');  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (27882, '19980620');  
INSERT Sales.Sales_Tracking (Rep_id)  
VALUES (21392);  
INSERT Sales.Sales_Tracking (Rep_id, Last_sale)  
VALUES (24283, '19981130');  
GO  
```  
  
 Следующий запрос позволяет выбрать все данные из таблицы `Sales_Tracking`:  
  
```sql
SELECT * FROM Sales_Tracking ORDER BY Rep_id;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Territory_id Rep_id Last_sale            SRep_tracking_user
-----------  ------ -------------------- ------------------
2000         151    Mar 4 1998 10:36AM   ArvinDak
2001         293    May 15 1998 12:00AM  ArvinDak
2003         21392  Mar 4 1998 10:36AM   ArvinDak
2004         24283  Nov 3 1998 12:00AM   ArvinDak
2002         27882  Jun 20 1998 12:00AM  ArvinDak
  
(5 row(s) affected)
 ```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-system_user-to-return-the-current-system-user-name"></a>В. Использование функции SYSTEM_USER для получения текущего системного имени пользователя  
 В приведенном ниже примере возвращается текущее значение `SYSTEM_USER`.  
  
```sql
SELECT SYSTEM_USER;  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP (Transact-SQL)](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER (Transact-SQL)](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER (Transact-SQL)](../../t-sql/functions/session-user-transact-sql.md)   
 [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [USER (Transact-SQL)](../../t-sql/functions/user-transact-sql.md)  
  
  


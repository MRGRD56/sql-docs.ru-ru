---
description: CREATE ROLE (Transact-SQL)
title: CREATE ROLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE ROLE
- DATABASE ROLE
- ROLE_TSQL
- DATABASE_ROLE_TSQL
- CREATE_ROLE_TSQL
- CREATE DATABASE ROLE
- ROLE
- CREATE_DATABASE_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], creating
- CREATE DATABASE ROLE statement
- roles [SQL Server], creating
- CREATE ROLE statement
ms.assetid: b0cd54ad-e81d-4d71-acec-8a6d7261ca08
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7741884929c7d682bee55067f9d8e64ade999d42
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749224"
---
# <a name="create-role-transact-sql"></a>CREATE ROLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Создает новую роль базы данных в текущей базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
CREATE ROLE role_name [ AUTHORIZATION owner_name ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *role_name*  
 Имя создаваемой роли.  
  
 AUTHORIZATION *owner_name*  
 Пользователь (или роль) базы данных, который станет владельцем новой роли. Если пользователь не указан, владельцем роли станет пользователь, выполнивший инструкцию CREATE ROLE. Владелец роли или любой элемент роли-владельца может добавлять или удалять элементы роли.
  
## <a name="remarks"></a>Комментарии  
 Роли — это сущности, защищаемые на уровне базы данных. После создания роли необходимо настроить для нее разрешения уровня базы данных при помощи инструкций GRANT, DENY и REVOKE. Для добавления членов в роль базы данных следует использовать инструкцию [ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md). Дополнительные сведения см. в статье [Роли уровня базы данных](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 Роли базы данных видны в представлениях каталога sys.database_role_members и sys.database_principals.  
  
 Сведения о проектировании системы разрешений см. в статье [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение **CREATE ROLE** для базы данных или членство в предопределенной роли базы данных **db_securityadmin**. Если указывается параметр **AUTHORIZATION**, необходимы также следующие разрешения:  
  
-   Для передачи роли во владение другому пользователю необходимо связанное с этим пользователем разрешение IMPERSONATE.  
  
-   Для передачи роли во владение другой роли необходимо членство в роли-получателе или связанное с этой ролью разрешение ALTER.  
  
-   Для передачи роли во владение роли приложения необходимо связанное с прикладной ролью разрешение ALTER.  
  
## <a name="examples"></a>Примеры  
Во всех приведенных ниже примерах используется база данных AdventureWorks.   

### <a name="a-creating-a-database-role-that-is-owned-by-a-database-user"></a>A. Создание роли базы данных, принадлежащей пользователю базы данных  
 Следующий пример создает роль базы данных `buyers`, принадлежащую пользователю `BenMiller`.  
  
```sql  
CREATE ROLE buyers AUTHORIZATION BenMiller;  
GO  
```  
  
### <a name="b-creating-a-database-role-that-is-owned-by-a-fixed-database-role"></a>Б. Создание роли базы данных, принадлежащей предопределенной роли базы данных  
 Следующий пример создает роль базы данных `auditors`, принадлежащую предопределенной роли базы данных `db_securityadmin`.  
  
```sql  
CREATE ROLE auditors AUTHORIZATION db_securityadmin;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE (Transact-SQL)](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Хранимая процедура sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Приступая к работе с разрешениями Database Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  



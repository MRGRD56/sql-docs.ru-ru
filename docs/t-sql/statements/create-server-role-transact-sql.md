---
description: CREATE SERVER ROLE (Transact-SQL)
title: CREATE SERVER ROLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SERVER_ROLE_TSQL
- CREATE SERVER ROLE
- SERVER ROLE
- CREATE_SERVER_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE
- SERVER ROLE, CREATE
- CREATE SERVER ROLE statement
- ROLE
- user-defined server roles [SQL Server]
- roles, server
ms.assetid: 30c92f80-f7f6-4a84-ae89-16e69add0de6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: cfaf0335c2ba61e093764352b16847f0f96b295c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192648"
---
# <a name="create-server-role-transact-sql"></a>CREATE SERVER ROLE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает новую, определяемую пользователем роль сервера.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
CREATE SERVER ROLE role_name [ AUTHORIZATION server_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *role_name*  
 Имя создаваемой роли сервера.  
  
 AUTHORIZATION *server_principal*  
 Является именем входа, которому будет принадлежать новая роль сервера. Если имя входа не указано, владельцем роли сервера станет имя входа, выполнившее инструкцию CREATE SERVER ROLE.  
  
## <a name="remarks"></a>Комментарии  
 Роли сервера являются защищаемыми объектами уровня сервера. После создания роли сервера необходимо настроить для нее разрешения уровня сервера при помощи инструкций GRANT, DENY и REVOKE. Для добавления или удаления имен входа из роли сервера используется инструкция [ALTER SERVER ROLE (Transact-SQL)](../../t-sql/statements/alter-server-role-transact-sql.md). Чтобы удалить роль сервера, используйте инструкцию [DROP SERVER ROLE (Transact-SQL)](../../t-sql/statements/drop-server-role-transact-sql.md). Дополнительные сведения см. в разделе [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Для просмотра ролей сервера выполните запрос к представлениям каталога [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) и [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Ролям сервера нельзя предоставить разрешение на защищаемые объекты уровня базы данных. Сведения о создании ролей базы данных см. в статье [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md).  
  
 Сведения о проектировании системы разрешений см. в статье [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения CREATE SERVER ROLE или членства в предопределенной роли сервера.  
  
 Также требует разрешения IMPERSONATE на *server_principal* для имен входа, разрешения ALTER для ролей сервера, которые используются в качестве *server_principal*, или членства в группе Windows, которая используется в качестве server_principal.  
  
 Это вызовет событие Audit Server Principal Management с типом объекта, установленным как роль сервера, и типом добавляемого события.  
  
 Если для назначения владельца роли сервера указывается параметр AUTHORIZATION, необходимы также следующие разрешения.  
  
-   Для передачи роли сервера во владение другому имени входа необходимо связанное с этим именем входа разрешение IMPERSONATE.  
  
-   Для передачи роли сервера во владение другой роли сервера необходимо членство в роли сервера-получателя или связанное с этой ролью сервера разрешение ALTER.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-creating-a-server-role-that-is-owned-by-a-login"></a>A. Создание роли сервера, принадлежащей имени входа  
 Следующий пример создает роль сервера `buyers`, принадлежащую имени входа `BenMiller`.  
  
```sql  
USE master;  
CREATE SERVER ROLE buyers AUTHORIZATION BenMiller;  
GO  
```  
  
### <a name="b-creating-a-server-role-that-is-owned-by-a-fixed-server-role"></a>Б. Создание роли сервера, принадлежащей предопределенной роли сервера  
 Следующий пример создает роль сервера `auditors`, принадлежащую предопределенной роли сервера `securityadmin`.  
  
```sql  
USE master;  
CREATE SERVER ROLE auditors AUTHORIZATION securityadmin;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [DROP SERVER ROLE (Transact-SQL)](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Хранимая процедура sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [Приступая к работе с разрешениями Database Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
  

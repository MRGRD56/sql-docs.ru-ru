---
title: GRANT (разрешения на субъект базы данных)
description: Предоставляет разрешения для пользователя базы данных, роли базы данных или роли приложения.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 03/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], permissions
- permissions [SQL Server], database roles
- granting permissions [SQL Server], database users
- granting permissions [SQL Server], application roles
- granting permissions [SQL Server], database roles
- database user permissions [SQL Server]
- permissions [SQL Server], application roles
- permissions [SQL Server], database users
- GRANT statement, users
- GRANT statement, roles
- application roles [SQL Server], permissions
ms.assetid: 012588a2-cbe1-48f0-a731-b4a2b83203d5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3eabbbcbe3842ba795c666a56d9d6c87216187e5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750454"
---
# <a name="grant-database-principal-permissions-transact-sql"></a>GRANT, предоставление разрешений на участника базы данных (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Предоставление разрешений пользователям базы данных, ролям базы данных или ролям приложения в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
GRANT permission [ ,...n ]    
    ON   
    {  [ USER :: database_user ]  
     | [ ROLE :: database_role ]  
     | [ APPLICATION ROLE :: application_role ]  
    }  
    TO <database_principal> [ ,...n ]  
       [ WITH GRANT OPTION ]  
       [ AS <database_principal> ]  
  
<database_principal> ::=  
    Database_user   
  | Database_role   
  | Application_role   
  | Database_user_mapped_to_Windows_User   
  | Database_user_mapped_to_Windows_Group   
  | Database_user_mapped_to_certificate   
  | Database_user_mapped_to_asymmetric_key   
  | Database_user_with_no_login   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *permission*  
 Указывает разрешение, которое может быть предоставлено участнику базы данных. Список разрешений см. в подразделе "Примечания" далее в этом разделе.  
  
 USER ::*database_user*  
 Указывает класс и имя пользователя, которому предоставляется разрешение. Квалификатор области (::) является обязательным.  
  
 ROLE ::*database_role*  
 Указывает класс и имя роли, которой предоставляется разрешение. Квалификатор области (::) является обязательным.  
  
 APPLICATION ROLE ::*application_role*  
   
 Указывает класс и имя роли приложения, которой предоставляется разрешение. Квалификатор области (::) является обязательным.  
  
 WITH GRANT OPTION  
 Показывает, что участнику будет дана возможность предоставлять указанное разрешение другим участникам.  
  
 AS \<database_principal>  
 Указывает участника, от которого участник, выполняющий данный запрос, наследует право на предоставление разрешения.  
  
 *Database_user*  
 Указывает пользователя базы данных.  
  
 *Database_role*  
 Указывает роль базы данных.  
  
 *Application_role*  
 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Указывает роль приложения.  
  
 *Database_user_mapped_to_Windows_User*  
 Указывает пользователя базы данных, сопоставленного с пользователем Windows.  
  
 *Database_user_mapped_to_Windows_Group*  
  
 Указывает пользователя базы данных, сопоставленного с группой Windows.  
  
 *Database_user_mapped_to_certificate*  
  
 Указывает пользователя базы данных, сопоставленного с сертификатом.  
  
 *Database_user_mapped_to_asymmetric_key*  
  
 Указывает пользователя базы данных, сопоставленного с асимметричным ключом.  
  
 *Database_user_with_no_login*  
 Указывает пользователя базы данных, не сопоставленного с субъектом серверного уровня.  
  
## <a name="remarks"></a>Remarks  
 Данные о субъектах базы данных отображаются в представлении каталога [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md). Данные о разрешениях уровня базы данных отображаются в представлении каталога [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md).  
  
## <a name="database-user-permissions"></a>Разрешения пользователя базы данных  
 Пользователь базы данных — это защищаемый объект уровня базы данных, содержащийся в базе данных, являющейся родительским элементом в иерархии разрешений. Наиболее специфичные и ограниченные разрешения, которые можно предоставлять пользователю базы данных, перечислены в следующей таблице вместе с более общими разрешениями, неявно их содержащими.  
  
|Разрешение пользователя базы данных|Содержится в разрешении пользователя базы данных|Содержится в разрешении базы данных|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|IMPERSONATE|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY USER|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="database-role-permissions"></a>Разрешения роли базы данных  
 Роль базы данных — это защищаемый объект уровня базы данных, содержащийся в базе данных, являющейся родительским элементом в иерархии разрешений. Наиболее специфичные и ограниченные разрешения, которые можно предоставлять роли базы данных, перечислены в следующей таблице вместе с более общими разрешениями, неявно их содержащими.  
  
|Разрешение роли базы данных|Содержится в разрешении роли базы данных|Содержится в разрешении базы данных|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="application-role-permissions"></a>Разрешения роли приложения  
 Роль приложения — это защищаемый объект уровня базы данных, содержащийся в базе данных, являющейся родительским элементом в иерархии разрешений. Наиболее специфичные и ограниченные разрешения, которые можно предоставлять роли приложения, перечислены далее вместе с более общими разрешениями, неявно их содержащими.  
  
|Разрешение роли приложения|Содержится в разрешении роли приложения|Содержится в разрешении базы данных|  
|---------------------------------|--------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY APPLICATION ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Разрешения  
 Объект, предоставляющий разрешение (или участник, указанный параметром AS), должен иметь либо само разрешение, выданное с помощью параметра GRANT OPTION, либо разрешение более высокого уровня, которое неявно включает предоставляемое.  
  
 При использовании параметра AS налагаются следующие дополнительные требования.  
  
|AS *granting_principal*|Необходимо дополнительное разрешение|  
|------------------------------|------------------------------------|  
|пользователь базы данных;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|Пользователь базы данных сопоставлен с пользователем Windows|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|Пользователь базы данных, сопоставленный группе Windows|Членство в группе Windows, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с сертификатом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с асимметричным ключом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|Пользователь базы данных, не сопоставленный ни с одним участником на уровне сервера|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|роль базы данных;|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|  
|Роль приложения|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|  
  
 Участники, имеющие разрешение CONTROL на защищаемый объект, могут предоставлять разрешение на этот защищаемый объект.  
  
 Участники, получившие разрешение CONTROL на базу данных, такие как члены предопределенной роли базы данных db_owner, могут предоставлять любое разрешение на любой защищаемый объект в базе данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-granting-control-permission-on-a-user-to-another-user"></a>A. Предоставление разрешения CONTROL на пользователя другому пользователю  
 В следующем примере пользователю `CONTROL` предоставляется разрешение `AdventureWorks2012` на пользователя `Wanida` базы данных `RolandX`.  
  
```sql  
GRANT CONTROL ON USER::Wanida TO RolandX;  
GO  
```  
  
### <a name="b-granting-view-definition-permission-on-a-role-to-a-user-with-grant-option"></a>Б. Предоставление пользователю разрешения VIEW DEFINITION на роль с параметром GRANT OPTION  
 В следующем примере разрешение `VIEW DEFINITION` на роль `AdventureWorks2012` базы данных `SammamishParking`, с параметром `GRANT OPTION`, предоставляется пользователю этой базы данных, `JinghaoLiu`.  
  
```sql  
GRANT VIEW DEFINITION ON ROLE::SammamishParking   
    TO JinghaoLiu WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-impersonate-permission-on-a-user-to-an-application-role"></a>В. Предоставление роли приложения разрешения IMPERSONATE на пользователя  
 В следующем примере показано предоставление разрешения `IMPERSONATE` пользователю `HamithaL` на роль приложения `AdventureWorks2012``AccountsPayable17`.  
  
**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
```sql  
GRANT IMPERSONATE ON USER::HamithaL TO AccountsPayable17;  
GO    
```  
  
## <a name="see-also"></a>См. также:  
 [DENY, запрет разрешений на участника базы данных (Transact-SQL)](../../t-sql/statements/deny-database-principal-permissions-transact-sql.md)   
 [REVOKE, отмена разрешений на субъекта базы данных (Transact-SQL)](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Участники (компонент Database Engine)](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

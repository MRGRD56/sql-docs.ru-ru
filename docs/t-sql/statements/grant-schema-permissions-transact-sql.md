---
description: GRANT, предоставления разрешения на схему (Transact-SQL)
title: GRANT, предоставления разрешения на схему (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- schemas [SQL Server], permissions
- permissions [SQL Server], schemas
- GRANT statement, schemas
- granting permissions [SQL Server], schemas
ms.assetid: b2aa1fc8-e7af-45d2-9f80-737543c8aa95
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a2c11cca7a069a18fea4832d6a1221be3dcbecd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754334"
---
# <a name="grant-schema-permissions-transact-sql"></a>GRANT, предоставления разрешения на схему (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Предоставляет разрешения на работу со схемой.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
GRANT permission  [ ,...n ] ON SCHEMA :: schema_name  
    TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *permission*  
 Разрешение на работу со схемой, которое может быть предоставлено. Список разрешений см. в подразделе «Примечания» далее в этом разделе.  
  
 ON SCHEMA **::** schema *_name*  
 Схема, на работу с которой предоставляется разрешение. Квалификатор области **::** является обязательным.  
  
 *database_principal*  
 Участник, которому предоставляется разрешение. Это может быть:  
  
-   пользователь базы данных;  
-   роль базы данных;  
-   роль приложения;  
-   пользователь базы данных, сопоставленный с именем входа Windows;  
-   пользователь базы данных, сопоставленный с группой Windows;  
-   пользователь базы данных, сопоставленный с сертификатом;  
-   пользователь базы данных, сопоставленный с асимметричным ключом;  
-   пользователь базы данных, не сопоставленный с участником на уровне сервера.  
  
GRANT OPTION  
 Показывает, что участнику будет дана возможность предоставлять указанное разрешение другим участникам.  
  
AS *granting_principal*  
 Указывает участника, от которого участник, выполняющий данный запрос, наследует право на предоставление разрешения. Это может быть:  
  
-   пользователь базы данных;  
-   роль базы данных;  
-   роль приложения;  
-   пользователь базы данных, сопоставленный с именем входа Windows;  
-   пользователь базы данных, сопоставленный с группой Windows;  
-   пользователь базы данных, сопоставленный с сертификатом;  
-   пользователь базы данных, сопоставленный с асимметричным ключом;  
-   пользователь базы данных, не сопоставленный с участником на уровне сервера.  
  
## <a name="remarks"></a>Remarks  
  
> [!IMPORTANT]  
>  Сочетание разрешений ALTER и REFERENCE в некоторых случаях может позволить просматривать данные или выполнять несанкционированные функции. Пример: Пользователь с разрешением ALTER на таблицу и разрешением REFERENCE на функцию может создавать вычисляемый столбец на основе функции и в результате — выполнять ее. В этом случае пользователю также требуется разрешение SELECT на вычисляемый столбец.  
  
 Схема является защищаемым объектом уровня базы данных, содержащимся в базе данных, которая является материнской по отношению к нему в иерархии разрешений. Ниже перечислены наиболее специфичные и ограниченные разрешения (вместе с наиболее общими разрешениями, из которых они вытекают), которые могут быть предоставлены для схемы.  
  
|Разрешение схемы|Содержится в разрешении схемы|Содержится в разрешении базы данных|  
|-----------------------|----------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SCHEMA|  
|CONTROL|CONTROL|CONTROL|  
|CREATE SEQUENCE|ALTER|ALTER ANY SCHEMA|  
|DELETE|CONTROL|DELETE|  
|EXECUTE|CONTROL|EXECUTE|  
|INSERT|CONTROL|INSERT|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|CONTROL|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
> [!CAUTION]  
>  Пользователь с разрешением ALTER на схему может при помощи цепочки владения получить доступ к защищаемым сущностям других схем, включая такие сущности, к которым пользователю явно запрещен доступ. Это происходит потому, что цепочка владения обходит проверки разрешений на объекты, к которым выполняется обращение, когда они принадлежат участнику, владеющему объектами, которые на них ссылаются. Пользователь с разрешением ALTER на схему может создавать процедуры, синонимы и представления, принадлежащие владельцу схемы. Эти объекты получат доступ (через цепочку владения) к данным других схем, принадлежащих владельцу схемы. По возможности следует избегать предоставления разрешения ALTER на схему, если ее владельцу принадлежат также другие схемы.  
  
 Например, эта проблема может возникнуть в следующих сценариях. Предполагается, что у пользователя U1 есть разрешение ALTER на схему S1. Пользователю U1 запрещен доступ к объекту таблицы T1 в схеме S2. Схемы S1 и S2 принадлежат одному и тому же владельцу.  
  
 У пользователя U1 есть разрешения CREATE PROCEDURE на базу данных и EXECUTE на схему S1. Поэтому пользователь U1 может создать хранимую процедуру, а затем получить доступ к запрещенному объекту T1 из этой хранимой процедуры.  
  
 У пользователя U1 есть разрешения CREATE SYNONYM на базу данных и SELECT на схему S1. Поэтому пользователь U1 может создать в схеме S1 синоним для запрещенного объекта T1, а затем получить доступ к этому объекту с помощью синонима.  
  
 У пользователя U1 есть разрешения CREATE VIEW на базу данных и SELECT на схему S1. Поэтому пользователь U1 может создать в схеме S1 представления для запроса данных из запрещенного объекта T1, а затем получить доступ к этому объекту с помощью представления.
  
## <a name="permissions"></a>Разрешения  
 Объект, предоставляющий разрешение (или участник, указанный параметром AS), должен иметь либо само разрешение, выданное с помощью параметра GRANT OPTION, либо разрешение более высокого уровня, которое неявно включает предоставляемое.  
  
 Если используется параметр AS, налагаются следующие дополнительные требования.  
  
|AS *granting_principal*|Необходимо дополнительное разрешение|  
|------------------------------|------------------------------------|  
|пользователь базы данных;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с именем входа Windows;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с группой Windows;|Членство в группе Windows, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с сертификатом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с асимметричным ключом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|Пользователь базы данных, не сопоставленный ни с одним участником на уровне сервера|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|роль базы данных;|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|  
|Роль приложения|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|  
  
 Владельцы объектов могут предоставлять разрешения на объекты, которыми они владеют. Участники, имеющие разрешение CONTROL на защищаемый объект, могут предоставлять разрешение на этот защищаемый объект.  
  
 Участники, которым предоставлено разрешение CONTROL SERVER, такие как члены предопределенной роли сервера &lt;legacyBold&gt;sysadmin&lt;/legacyBold&gt;, могут предоставлять любое разрешение на любой защищаемый объект сервера. Участники, получившие разрешение CONTROL на базу данных, такие как члены предопределенной роли базы данных db_owner, могут предоставлять любое разрешение на любой защищаемый объект в базе данных. Владельцы разрешения CONTROL, связанного со схемой, могут предоставлять любые разрешения на работу с любыми объектами, содержащимися в данной схеме.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-granting-insert-permission-on-schema-humanresources-to-guest"></a>A. Предоставление пользователю guest разрешения INSERT на схему HumanResources  
  
```sql  
GRANT INSERT ON SCHEMA :: HumanResources TO guest;  
```  
  
### <a name="b-granting-select-permission-on-schema-person-to-database-user-wiljo"></a>Б. Предоставление пользователю базы данных WilJo разрешения SELECT на схему Person  
  
```sql  
GRANT SELECT ON SCHEMA :: Person TO WilJo WITH GRANT OPTION;  
```  
  
## <a name="see-also"></a>См. также  
 [DENY, запрет разрешений на схему (Transact-SQL)](../../t-sql/statements/deny-schema-permissions-transact-sql.md)   
 [REVOKE, отмена разрешений на схему (Transact-SQL)](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  

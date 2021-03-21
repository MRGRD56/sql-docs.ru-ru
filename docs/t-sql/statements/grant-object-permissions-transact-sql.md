---
description: GRANT, предоставление разрешений на объект (Transact-SQL)
title: GRANT, предоставление разрешений на объект (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], objects
- GRANT statement, objects
ms.assetid: c001c2e7-d092-43d4-8fa6-693b3ec4c3ea
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d24a9af512abfd789bc0d2ede28b9f4ca67fe680
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738984"
---
# <a name="grant-object-permissions-transact-sql"></a>GRANT, предоставление разрешений на объект (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Предоставляет разрешения на таблицу, представление, функцию с табличным значением, хранимую процедуру, расширенную хранимую процедуру, скалярную функцию, агрегатную функцию, очередь обслуживания или синоним.  
  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
GRANT <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
    TO <database_principal> [ ,...n ]   
    [ WITH GRANT OPTION ]  
    [ AS <database_principal> ]  
  
<permission> ::=  
    ALL [ PRIVILEGES ] | permission [ ( column [ ,...n ] ) ]  
  
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
 Указывает разрешение, которое может быть предоставлено на содержащийся в схеме объект. Список разрешений см. в подразделе "Примечания" далее в этом разделе.  
  
 ALL  
 Предоставление ALL не включает все возможные разрешения, оно эквивалентно предоставлению всех разрешений [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92, применимых к указанному объекту. Значение ALL различается для разных типов объектов  
  
- Разрешения на скалярные функции: EXECUTE, REFERENCES.  
- Разрешения на функции с табличным значением: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Разрешения на хранимые процедуры: EXECUTE.  
- Разрешения на таблицы: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
- Разрешения на представления: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
PRIVILEGES  
 Включено для обеспечения совместимости с [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]-92. Не изменяет работу ALL.  
  
*column*  
 Указывает имя столбца в таблице, представление или функции с табличным значением, на которых предоставляется разрешение. Указание круглых скобок ( ) обязательно. На столбец могут быть предоставлены только разрешения SELECT, REFERENCES и UPDATE. Аргумент *column* может быть указан в предложении PERMISSIONS или после имени защищаемого объекта.  
  
> [!CAUTION]  
>  Запрет (DENY) уровня таблицы имеет меньший приоритет, чем разрешение (GRANT) уровня столбца. Такая несогласованность в иерархии разрешений сохранена в целях обратной совместимости.  
  
 ON [ OBJECT :: ] [ *schema_name* ] . *object_name*  
 Указывает объект, на который предоставляется разрешение. Фраза OBJECT необязательна, если указан аргумент *schema_name*. Если же она указана, указание квалификатора области (::) обязательно. Если не указан аргумент *schema_name*, подразумевается схема по умолчанию. Если указан аргумент *schema_name*, обязательно указание квалификатора области схемы (.).  
  
 TO \<database_principal>  
 Участник, которому предоставляется разрешение.  
  
 WITH GRANT OPTION  
 Показывает, что участнику будет дана возможность предоставлять указанное разрешение другим участникам.  
  
 AS \<database_principal>. Указывает субъект, от которого субъект, выполняющий данный запрос, наследует право на предоставление разрешения.  
  
 *Database_user*  
 Указывает пользователя базы данных.  
  
 *Database_role*  
 Указывает роль базы данных.  
  
 *Application_role*  
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
  
> [!IMPORTANT]  
>  Сочетание разрешений ALTER и REFERENCE в некоторых случаях может позволить просматривать данные или выполнять несанкционированные функции. Пример: Пользователь с разрешением ALTER на таблицу и разрешением REFERENCE на функцию может создавать вычисляемый столбец на основе функции и в результате — выполнять ее. В этом случае пользователю также требуется разрешение SELECT на вычисляемый столбец.  
  
 Сведения об объектах доступны через различные представления каталога. Дополнительные сведения см. в разделе [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md).  
  
 Объект является защищаемым на уровне схемы. Он содержится в схеме, которая является его родителем в иерархии разрешений. В следующей таблице перечислен ряд отдельных разрешений, которые могут быть предоставлены на объект, а также наиболее общие разрешения, которые неявно их подразумевают.  
  
|Разрешение объекта|Содержится в разрешении объекта|Содержится в разрешении схемы|  
|-----------------------|----------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|DELETE|CONTROL|DELETE|  
|EXECUTE|CONTROL|EXECUTE|  
|INSERT|CONTROL|INSERT|  
|RECEIVE|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|RECEIVE|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|VIEW CHANGE TRACKING|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Разрешения  
 Объект, предоставляющий разрешение (или участник, указанный параметром AS), должен иметь либо само разрешение, выданное с помощью параметра GRANT OPTION, либо разрешение более высокого уровня, которое неявно включает предоставляемое.  
  
 При использовании параметра AS налагаются следующие дополнительные требования.  
  
|AS|Необходимо дополнительное разрешение|  
|--------|------------------------------------|  
|пользователь базы данных;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с именем входа Windows;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|Пользователь базы данных, сопоставленный группе Windows|Членство в группе Windows, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с сертификатом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|пользователь базы данных, сопоставленный с асимметричным ключом;|Членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|Пользователь базы данных, не сопоставленный ни с одним участником на уровне сервера|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных db_securityadmin, членство в предопределенной роли базы данных db_owner или членство в предопределенной роли сервера sysadmin.|  
|роль базы данных;|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|  
|Роль приложения|Разрешение ALTER на роль, членство в предопределенной роли базы данных db_securityadmin, предопределенной роли базы данных db_owner или предопределенной роли сервера sysadmin.|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-granting-select-permission-on-a-table"></a>A. Предоставление разрешения SELECT на таблицу  
 В следующем примере предоставляется разрешение `SELECT` пользователю `RosaQdM` на таблицу `Person.Address` в базе данных `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON OBJECT::Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="b-granting-execute-permission-on-a-stored-procedure"></a>Б. Предоставление разрешения EXECUTE на хранимую процедуру  
 В следующем примере предоставляется разрешение `EXECUTE` на хранимую процедуру `HumanResources.uspUpdateEmployeeHireInfo` роли приложения `Recruiting11`.  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO   
```  
  
### <a name="c-granting-references-permission-on-a-view-with-grant-option"></a>В. Предоставление разрешения REFERENCES на представление с параметром GRANT OPTION  
 В следующем примере предоставляется разрешение `REFERENCES` на столбец `BusinessEntityID` в представлении `HumanResources.vEmployee` пользователю `Wanida` с параметром `GRANT OPTION`.  
  
```sql  
GRANT REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    TO Wanida WITH GRANT OPTION;  
GO  
```  
  
### <a name="d-granting-select-permission-on-a-table-without-using-the-object-phrase"></a>Г. Предоставление разрешения SELECT на таблицу без использования фразы OBJECT  
 В следующем примере предоставляется разрешение `SELECT` пользователю `RosaQdM` на таблицу `Person.Address` в базе данных `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="e-granting-select-permission-on-a-table-to-a-domain-account"></a>Д. Предоставление учетной записи домена разрешения SELECT на таблицу  
 В следующем примере предоставляется разрешение `SELECT` пользователю `AdventureWorks2012\RosaQdM` на таблицу `Person.Address` в базе данных `AdventureWorks2012`.  
  
```sql  
GRANT SELECT ON Person.Address TO [AdventureWorks2012\RosaQdM];  
GO  
```  
  
### <a name="f-granting-execute-permission-on-a-procedure-to-a-role"></a>Е. Предоставление роли разрешения EXECUTE на процедуру  
 В следующем примере создается роль, а затем ей предоставляется разрешение `EXECUTE` на процедуру `uspGetBillOfMaterials` в базе данных `AdventureWorks2012`.  
  
```sql  
CREATE ROLE newrole ;  
GRANT EXECUTE ON dbo.uspGetBillOfMaterials TO newrole ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [DENY, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [REVOKE, отмена разрешений на объект (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  


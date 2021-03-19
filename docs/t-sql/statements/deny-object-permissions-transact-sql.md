---
description: DENY, запрет разрешений на объект (Transact-SQL)
title: DENY, запрет разрешений на объект (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, objects
- table permissions [SQL Server]
ms.assetid: 0b8d3ddc-38c0-4241-b7bb-ee654a5081aa
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 021cea9426fd6fb1c6e14c450fa37e69ef0048a6
ms.sourcegitcommit: 2cc2e4e17ce88ef47cda32a60a02d929e617738e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2021
ms.locfileid: "103473212"
---
# <a name="deny-object-permissions-transact-sql"></a>DENY, запрет разрешений на объект (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Запрещает разрешения на член класса OBJECT защищаемых объектов. Элементы класса OBJECT: таблицы, представления, функции с табличным значением, хранимые процедуры, расширенные хранимые процедуры, скалярные функции, агрегатные функции, очереди обслуживания и синонимы.  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DENY <permission> [ ,...n ] ON   
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]  
        TO <database_principal> [ ,...n ]   
    [ CASCADE ]  
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
 Обозначает разрешение, которое можно запретить для содержащегося в схеме объекта. Список разрешений см. в подразделе "Примечания" далее в этом разделе.  
  
 ALL  
 Запрет разрешения ALL не запрещает все возможные разрешения. Запрет ALL эквивалентен запрету всех разрешений ANSI-92, применимых к данному объекту. Значение ALL различается для разных типов объектов  
  
 - Разрешения на скалярные функции: EXECUTE, REFERENCES.  
 - Разрешения на функции с табличным значением: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
 - Разрешения на хранимые процедуры: EXECUTE.  
 - Разрешения на таблицы: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
 - Разрешения на представления: DELETE, INSERT, REFERENCES, SELECT, UPDATE.  
  
PRIVILEGES  
 Включено для совместимости с ANSI-92. Не изменяет работу ALL.  
  
*column*  
 Указывает имя столбца в таблице, представлении или функции с табличным значением, для которых запрещается разрешение. Указание круглых скобок **( )** обязательно. Для столбца можно запрещать только разрешения SELECT, REFERENCES и UPDATE. Аргумент *column* может быть указан в предложении PERMISSIONS или после имени защищаемого объекта.  
  
> [!CAUTION]  
>  Запрет (DENY) уровня таблицы имеет меньший приоритет, чем разрешение (GRANT) уровня столбца. Такая несогласованность в иерархии разрешений сохранена в целях обратной совместимости. В SQL Server такое поведение отличается, если сервер настроен для выполнения с [конфигурацией "Common Criteria Compliance Enabled"](/../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md). Но обычно ее следует использовать с осторожностью, а не во всех случаях.
  
 ON [ OBJECT **::** ] [ *schema_name* ] **.** *object_name*  
 Указывает объект, для которого запрещается разрешение. Фраза OBJECT необязательна, если указан аргумент *schema_name*. Если же она указана, указание квалификатора области ( **::** ) обязательно. Если не указан аргумент *schema_name*, подразумевается схема по умолчанию. Если указан аргумент *schema_name*, обязательно указание квалификатора области схемы ( **.** ).  
  
 TO \<database_principal>  
 Задает участника, для которого запрещается разрешение.  
  
 CASCADE  
 Указывает, что запрещаемое разрешение также запрещается для других участников, которым оно было предоставлено данным участником.  
  
 AS \<database_principal>  
 Задает участника, от которого участник, выполняющий данный запрос, получает право на запрет разрешения.  
  
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
 Сведения об объектах доступны через различные представления каталога. Дополнительные сведения см. в разделе [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md).  
  
 Объект является защищаемым на уровне схемы. Он содержится в схеме, которая является его родителем в иерархии разрешений. Наиболее специфичные и ограниченные разрешения, которые можно запретить для объекта, перечислены в следующей таблице вместе с общими разрешениями, неявно содержащими их.  
  
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
 Необходимо разрешение CONTROL для данного объекта.  
  
 При использовании предложения AS указанный участник должен быть владельцем объекта, разрешения на который запрещаются для этого участника.  
  
## <a name="examples"></a>Примеры  
В следующих примерах используется база данных AdventureWorks.
  
### <a name="a-denying-select-permission-on-a-table"></a>A. Запрет разрешения SELECT на таблицу  
 В следующем примере запрещается разрешение `SELECT` для пользователя `RosaQdM` на таблицу `Person.Address`.  
  
```sql  
DENY SELECT ON OBJECT::Person.Address TO RosaQdM;  
GO  
```  
  
### <a name="b-denying-execute-permission-on-a-stored-procedure"></a>Б. Запрет разрешения EXECUTE на хранимую процедуру  
 В следующем примере запрещается разрешение `EXECUTE` на хранимую процедуру `HumanResources.uspUpdateEmployeeHireInfo` для роли приложения под названием `Recruiting11`.  
  
```sql  
DENY EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO   
```  
  
### <a name="c-denying-references-permission-on-a-view-with-cascade"></a>В. Запрет разрешения REFERENCES на представление с CASCADE  
 В следующем примере запрещается разрешение `REFERENCES` на столбец `BusinessEntityID` в представлении `HumanResources.vEmployee` для пользователя `Wanida` с `CASCADE`.  
  
```sql  
DENY REFERENCES (BusinessEntityID) ON OBJECT::HumanResources.vEmployee   
    TO Wanida CASCADE;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [REVOKE, отмена разрешений на объект (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)  
  
  

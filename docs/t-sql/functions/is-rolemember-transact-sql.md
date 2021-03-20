---
description: IS_ROLEMEMBER (Transact-SQL)
title: IS_ROLEMEMBER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_ROLEMEMBER
- IS_ROLEMEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_ROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 73efa688-ae91-4014-98bc-1cabe47321f7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a71bdc75ed965e377ec0fd8e5e6a8ddfd60b477
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746794"
---
# <a name="is_rolemember-transact-sql"></a>IS_ROLEMEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Указывает, является ли данный участник базы данных членом заданной роли базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
IS_ROLEMEMBER ( 'role' [ , 'database_principal' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 **'** *role* **'**  
 Имя роли базы данных, проверяемой в данный момент. Аргумент *role* имеет тип **sysname**.  
  
 **'** *database_principal* **'**  
 Имя пользователя базы данных, роли базы данных или роли приложения для проверки. Аргумент *database_principal* имеет тип **sysname** и значение по умолчанию NULL. Если значение не указано, то результат основан на текущем контексте выполнения. Если параметр содержит слово NULL, то возвращается NULL.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
|Возвращаемое значение|Описание|  
|------------------|-----------------|  
|0|*database_principal* не является членом роли *role*.|  
|1|*database_principal* является членом роли *role*.|  
|NULL|Аргумент *database_principal* или *role* недопустим, либо у вас нет разрешения на просмотр членства в роли.|  
  
## <a name="remarks"></a>Комментарии  
 Чтобы определить, может ли текущий пользователь выполнить действие, требующее разрешений роли базы данных, воспользуйтесь функцией IS_ROLEMEMBER.  
  
 Если аргумент *database_principal* основан на имени входа Windows, например Contoso\Mary5, то функция IS_ROLEMEMBER возвращает значение NULL, если только участнику *database_principal* не предоставлен или не запрещен прямой доступ к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Если необязательный параметр *database_principal* не указан, а участник *database_principal* создан на основе имени входа домена Windows, то он может быть членом роли базы данных через членство в группе Windows. Чтобы решить проблему косвенного членства, функция IS_ROLEMEMBER запрашивает у контроллера домена сведения о членстве в группах Windows. Если контроллер домена недоступен или не отвечает, то функция IS_ROLEMEMBER возвращает сведения о членстве в роли, принимая во внимание только учетную запись пользователя и локальные группы. Если указанный пользователь не является текущим, то значение, возвращаемое функцией IS_ROLEMEMBER, может отличаться от значения в структуре проверки подлинности (например, Active Directory), полученного [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при последнем обновлении данных.  
  
 Если необязательный параметр *database_principal* указан, то участник базы данных, для которого выполняется запрос, должен присутствовать в таблице sys.database_principals. В противном случае функция IS_ROLEMEMBER возвратит значение NULL. Это означает, что *database_principal* является недопустимым в этой базе данных.  
  
 Если параметр *database_principal* основан на имени входа домена или на группе Windows и контроллер домена недоступен, то вызовы функции IS_ROLEMEMBER завершатся ошибкой и могут вернуть неверные или неполные данные.  
  
 Если контроллер домена недоступен, то вызов функции IS_ROLEMEMBER вернет правильные данные при условии, что проверку подлинности участника Windows можно выполнить локально (например, для локальной учетной записи Windows или имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
 **IS_ROLEMEMBER** всегда возвращает значение 0, если группа Windows используется как аргумент участника базы данных, а сама группа при этом является членом другой группы Windows, которая, в свою очередь, является членом указанной роли базы данных.  
  
 Контроль учетных записей в [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] и Windows Server 2008 также может приводить к разным результатам. Это зависит от того, обращался ли пользователь к серверу в качестве члена группы Windows или как определенный пользователь [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Эта функция вычисляет членство в роли, а не базовое разрешение. Например, предопределенная роль базы данных **db_owner** имеет разрешение **CONTROL DATABASE**. Если у пользователя есть разрешение **CONTROL DATABASE**, но он не является членом этой роли, то эта функция справедливо сообщает, что пользователь не является членом роли **db_owner**, даже несмотря на то, что имеет те же разрешения.  
  
## <a name="related-functions"></a>Связанные функции  
 Чтобы определить, является ли текущий пользователь членом указанной группы Windows или роли базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], воспользуйтесь функцией [IS_MEMBER (Transact-SQL)](../../t-sql/functions/is-member-transact-sql.md). Чтобы определить, является ли имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] членом роли сервера, воспользуйтесь функцией [IS_SRVROLEMEMBER (Transact-SQL)](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения VIEW DEFINITION для роли базы данных.  
  
## <a name="examples"></a>Примеры  
 В следующем примере определяется принадлежность имени входа текущего пользователя к предопределенной роли базы данных `db_datareader`.  
  
```sql  
IF IS_ROLEMEMBER ('db_datareader') = 1  
   print 'Current user is a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') = 0  
   print 'Current user is NOT a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') IS NULL  
   print 'ERROR: The database role specified is not valid.';  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE (Transact-SQL)](../../t-sql/statements/drop-role-transact-sql.md)   
 [CREATE SERVER ROLE (Transact-SQL)](../../t-sql/statements/create-server-role-transact-sql.md)   
 [ALTER SERVER ROLE (Transact-SQL)](../../t-sql/statements/alter-server-role-transact-sql.md)   
 [DROP SERVER ROLE (Transact-SQL)](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [IS_MEMBER (Transact-SQL)](../../t-sql/functions/is-member-transact-sql.md)   
 [Функция IS_SRVROLEMEMBER (Transact-SQL)](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Функции безопасности &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  

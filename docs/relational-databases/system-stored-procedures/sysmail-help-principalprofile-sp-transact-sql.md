---
description: sysmail_help_principalprofile_sp (Transact-SQL)
title: sysmail_help_principalprofile_sp (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_principalprofile_sp_TSQL
- sysmail_help_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_principalprofile_sp
ms.assetid: 0cfd6464-09c7-4f03-9d25-58001c096a9e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb4b5cd655bf4a401d530e4c9bd52cbc9660a65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181963"
---
# <a name="sysmail_help_principalprofile_sp-transact-sql"></a>sysmail_help_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Выводит сведения об взаимосвязях между профилями компонента Database Mail и участниками базы данных.  
  
 
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sysmail_help_principalprofile_sp [ {   [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ]  
    [ [ , ] {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @principal_id = ] principal_id` Идентификатор пользователя или роли базы данных в базе данных **msdb** для списка взаимосвязей. *principal_id* имеет **тип int** и значение по умолчанию NULL. Можно указать либо *principal_id* , либо *principal_name* .  
  
`[ @principal_name = ] 'principal_name'` Имя пользователя или роли базы данных в базе данных **msdb** для списка взаимосвязей. Аргумент *principal_name* имеет тип **sysname** и значение по умолчанию NULL. Можно указать либо *principal_id* , либо *principal_name* .  
  
`[ @profile_id = ] profile_id` Идентификатор профиля для списка взаимосвязей. *profile_id* имеет **тип int** и значение по умолчанию NULL. Можно указать либо *profile_id* , либо *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Имя профиля для списка взаимосвязей. Аргумент *profile_name* имеет тип **sysname** и значение по умолчанию NULL. Можно указать либо *profile_id* , либо *profile_name* .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
 Возвращает результирующий набор, содержащий столбцы, перечисленные в следующей таблице.  
  
| Имя столбца | Тип данных | Описание |
| ----------- | --------- | ----------- |
|**principal_id**|**int**|Идентификатор пользователя базы данных.|  
|**principal_name**|**sysname**|Имя пользователя базы данных.|  
|**profile_id**|**int**|Идентификатор профиля компонента Database Mail.|  
|**profile_name**|**sysname**|Имя профиля компонента Database Mail.|  
|**is_default**|**bit**|Флаг, который указывает, является ли профиль профилем по умолчанию для пользователя.|  
  
## <a name="remarks"></a>Замечания  
 Если **sysmail_help_principalprofile_sp** вызывается без параметров, то возвращаемый результирующий набор перечисляет все ассоциации в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . В противном случае результирующий набор содержит сведения об ассоциациях, которые удовлетворяют предоставленным параметрам. Например, если представлено имя профиля, процедура приводит список всех взаимосвязей для профиля.  
  
 **sysmail_help_principalprofile_sp** находится в базе данных **msdb** и принадлежит схеме **dbo** . Процедура должна быть выполнена с именем, сопоставленным с тремя частями, если текущей базой данных не является **msdb**.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо членство в предопределенной роли сервера **sysadmin** .  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-listing-information-for-a-specific-association"></a>A. Вывод сведений об указанной ассоциации  
 В следующем примере выводятся сведения обо всех взаимосвязях между профилем `AdventureWorks Administrator` и участником `ApplicationLogin` в базе данных `msdb`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp  
    @principal_name = 'danw',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 Образец результирующего набора, отформатированного по длине строки.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
5            danw               9           AdventureWorks Administrator   1  
```  
  
### <a name="b-listing-information-for-all-associations"></a>Б. Вывод сведений обо всех ассоциациях  
 В следующем примере выводятся сведения обо всех ассоциациях в экземпляре.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp ;  
```  
  
 Образец результирующего набора, отформатированного по длине строки.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
6            terrid             3           Product Update Profile         1  
5            danw               9           AdventureWorks Administrator   1  
```  
  
## <a name="see-also"></a>См. также:  
 [Database Mail](../../relational-databases/database-mail/database-mail.md)   
 [Database Mail хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  

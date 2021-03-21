---
description: sp_droprolemember (Transact-SQL)
title: sp_droprolemember (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droprolemember_TSQL
- sp_droprolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droprolemember
ms.assetid: c2f19ab1-e742-4d56-ba8e-8ffd40cf4925
ms.author: vanto
author: VanMSFT
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 98eff289464aa2bf6968ce8440f2f07c08f2ea31
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740824"
---
# <a name="sp_droprolemember-transact-sql"></a>sp_droprolemember (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет в текущей базе данных учетную запись безопасности из роли [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [инструкцию ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  

### <a name="syntax-for-both-sql-server-and-azure-sql-database"></a>Синтаксис как для SQL Server, так и для базы данных SQL Azure

```syntaxsql  
sp_droprolemember [ @rolename = ] 'role' ,   
     [ @membername = ] 'security_account'  
```  

### <a name="syntax-for-both-azure-synapse-analytics-and-parallel-data-warehouse"></a>Синтаксис для Azure синапсе Analytics и хранилища данных Parallel Data

```syntaxsql  
sp_droprolemember 'role' ,  
     'security_account'  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Аргументы  
`[ @rolename = ] 'role'` Имя роли, из которой удаляется элемент. Аргумент *Role* имеет тип **sysname** и не имеет значения по умолчанию. *роль* должна существовать в текущей базе данных.  
  
`[ @membername = ] 'security_account'` Имя учетной записи безопасности, удаляемой из роли. Аргумент *security_account* имеет тип **sysname** и не имеет значения по умолчанию. *security_account* может быть пользователь базы данных, другая роль базы данных, имя входа Windows или группа Windows. *security_account* должен существовать в текущей базе данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Remarks  
 sp_droprolemember исключает члена из роли базы данных, удаляя строку из таблицы sysmembers. При удалении члена из роли он теряет все разрешения, которые имел как член этой роли.  
  
 Для удаления пользователя из предопределенной роли сервера пользуйтесь хранимой процедурой sp_dropsrvrolemember. Пользователи не могут быть удалены из роли public, а dbo не может быть удален из какой-либо роли.  
  
 Используйте sp_helpuser для просмотра членов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] роли и с помощью инструкции ALTER ROLE добавьте член в роль.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER на эту роль.  
  
## <a name="examples"></a>Примеры  
 В следующем примере производится удаление пользователя `JonB` из роли `Sales`.  
  
```sql
EXEC sp_droprolemember 'Sales', 'Jonb';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В следующем примере производится удаление пользователя `JonB` из роли `Sales`.  
  
```sql
EXEC sp_droprolemember 'Sales', 'JonB'  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Хранимая процедура sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droprole-transact-sql.md)   
 [sp_dropsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)   
 [sp_helpuser &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  


---
description: sp_approlepassword (Transact-SQL)
title: sp_approlepassword (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_approlepassword
- sp_approlepassword_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_approlepassword
ms.assetid: 7967dc0b-bee2-4c63-b8e9-1c3ce2f5db2a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e2b2f109ca1e5abe94db0d4aa491c16b95cc713
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203214"
---
# <a name="sp_approlepassword-transact-sql"></a>sp_approlepassword (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет пароль роли приложения в текущей базе данных.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [ALTER Application Role](../../t-sql/statements/alter-application-role-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_approlepassword [ @rolename= ] 'role' , [ @newpwd = ] 'password'   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @rolename = ] 'role'` Имя роли приложения. Аргумент *Role* имеет тип **sysname** и не имеет значения по умолчанию. *роль* должна существовать в текущей базе данных.  
  
`[ @newpwd = ] 'password'` Новый пароль для роли приложения. Аргумент *Password* имеет тип **sysname** и не имеет значения по умолчанию. *пароль* не может иметь значение null.  
  
> [!IMPORTANT]  
>  Не используйте пароль со значением NULL. Выбирайте надежные пароли. Дополнительные сведения см. в разделе [Strong Passwords](../../relational-databases/security/strong-passwords.md).  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 **sp_approlepassword** не может быть выполнена в пользовательской транзакции.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо наличие разрешения ALTER ANY APPLICATION ROLE для этой базы данных.  
  
## <a name="examples"></a>Примеры  
 Следующий пример устанавливает пароль для роли приложения `PayrollAppRole` равным `B3r12-36`.  
  
```  
EXEC sp_approlepassword 'PayrollAppRole', '''B3r12-36';  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Роли приложений](../../relational-databases/security/authentication-access/application-roles.md)   
 [sp_addapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addapprole-transact-sql.md)   
 [sp_setapprole (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

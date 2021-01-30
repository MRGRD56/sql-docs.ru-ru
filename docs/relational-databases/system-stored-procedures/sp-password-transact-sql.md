---
description: sp_password (Transact-SQL)
title: sp_password (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_password
- sp_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_password
ms.assetid: 0ecbec81-e637-44a9-a61e-11bf060ef084
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f154661d7cd03edfde83dd7774685e73e79aeb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195432"
---
# <a name="sp_password-transact-sql"></a>sp_password (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Добавляет или изменяет пароль для [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имени входа.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [инструкцию ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_password [ [ @old = ] 'old_password' , ]  
     { [ @new =] 'new_password' }  
     [ , [ @loginame = ] 'login' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @old = ] 'old_password'` Старый пароль. Аргумент *old_password* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @new = ] 'new_password'` Новый пароль. Аргумент *new_password* имеет тип **sysname** и не имеет значения по умолчанию. необходимо указать *old_password* , если именованные параметры не используются.  
  
> [!IMPORTANT]  
>  Не используйте пароль со значением NULL. Выбирайте надежные пароли. Дополнительные сведения см. в разделе [Strong Passwords](../../relational-databases/security/strong-passwords.md).  
  
`[ @loginame = ] 'login'` Имя входа, затронутое изменением пароля. Аргумент *login* имеет тип **sysname** и значение по умолчанию NULL. *имя входа* должно уже существовать и может быть указано только членами предопределенных ролей сервера **sysadmin** или **администратора** .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 **sp_password** вызывает инструкцию ALTER LOGIN. Эта инструкция поддерживает дополнительные параметры. Сведения об изменении паролей см. в разделе [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md).  
  
 **sp_password** не может быть выполнена в пользовательской транзакции.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER ANY LOGIN. Также требуется разрешение CONTROL SERVER для сброса старого пароля без его ввода, или если изменяемое имя входа имеет разрешение CONTROL SERVER.  
  
 Участник всегда может изменить свой собственный пароль.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-changing-the-password-of-a-login-without-knowing-the-old-password"></a>A. Изменение пароля учетной записи без ввода старого пароля  
 Следующий пример показывает, как пользоваться `ALTER LOGIN` для смены пароля учетной записи `Victoria` на `B3r1000d#2-36`. Это является предпочтительным методом. Пользователь, который выполняет эту команду, должен иметь разрешение CONTROL SERVER.  
  
```  
ALTER LOGIN Victoria WITH PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
### <a name="b-changing-a-password"></a>Б. Изменение пароля  
 Следующий пример показывает, как пользоваться `ALTER LOGIN` для смены пароля пользователя `Victoria` с `B3r1000d#2-36` на `V1cteAmanti55imE`. Это является предпочтительным методом. Пользователь `Victoria` может использовать эту команду безо всяких дополнительных разрешений. Другим пользователям для этого требуется разрешение ALTER ANY LOGIN.  
  
```  
ALTER LOGIN Victoria WITH   
     PASSWORD = 'V1cteAmanti55imE'   
     OLD_PASSWORD = 'B3r1000d#2-36';  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN (Transact-SQL)](../../t-sql/statements/alter-login-transact-sql.md)   
 [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [Хранимая процедура sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Хранимая процедура sp_revokelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

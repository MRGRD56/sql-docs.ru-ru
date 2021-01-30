---
description: Хранимая процедура sp_revokelogin (Transact-SQL)
title: sp_revokelogin (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revokelogin_TSQL
- sp_revokelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revokelogin
ms.assetid: cb1ab102-1ae0-4811-9144-9a8121ef2d7e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 130ba750ed2e6dfadac111578ee9c42c6578c313
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161336"
---
# <a name="sp_revokelogin-transact-sql"></a>Хранимая процедура sp_revokelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет записи входа из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для пользователя или группы Windows, созданных с помощью инструкции CREATE LOGIN, **sp_grantlogin** или **sp_denylogin**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [инструкцию DROP LOGIN](../../t-sql/statements/drop-login-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_revokelogin [ @loginame= ] 'login'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @loginame = ] 'login'` Имя пользователя или группы Windows. Аргумент *Login* имеет тип **sysname** и не имеет значения по умолчанию. именем для *входа* может быть любое существующее имя пользователя или группа Windows в формате *компьютер имя* \\ *пользователь или домен* \\ *пользователь*.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 **sp_revokelogin** отключает соединения, используя учетную запись, указанную параметром *Login* . Но пользователи Windows, которым доступ к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] был предоставлен через членство в группе Windows, тем не менее, могут установить соединение после того, как был запрещен их индивидуальный доступ. Аналогично, если параметр *Login* указывает имя группы Windows, то члены этой группы, которым был предоставлен доступ к экземпляру по отдельности, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будут по-прежнему иметь возможность подключения.  
  
 Например, если пользователь **Адвворкс\жохн** Windows является членом группы Windows **адвворкс\админс**, а **sp_revokelogin** отменяет доступ к `ADVWORKS\john` :  
  
```  
sp_revokelogin [ADVWORKS\john]  
```  
  
 Пользователь **адвворкс\жохн** по-прежнему может подключиться, если **адвворкс\админс** предоставил доступ к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Аналогично, если доступ к группе Windows **адвворкс\админс** был отменен, но **адвворкс\жохн** получает доступ, **адвворкс\жохн** может подключиться.  
  
 Используйте **sp_denylogin** , чтобы явно запретить пользователям подключаться к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , независимо от членства в группе Windows.  
  
 **sp_revokelogin** не может быть выполнена в пользовательской транзакции.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER ANY LOGIN на сервере.  
  
## <a name="examples"></a>Примеры  
 В следующем примере удаляются записи входа для пользователя Windows `Corporate\MollyA` .  
  
```  
EXEC sp_revokelogin 'Corporate\MollyA';  
```  
  
 либо  
  
```  
EXEC sp_revokelogin [Corporate\MollyA];  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DROP LOGIN &#40;&#41;Transact-SQL ](../../t-sql/statements/drop-login-transact-sql.md)   
 [Хранимая процедура sp_denylogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_droplogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [Хранимая процедура sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

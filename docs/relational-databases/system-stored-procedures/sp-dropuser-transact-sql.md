---
description: sp_dropuser (Transact-SQL)
title: sp_dropuser (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropuser
- sp_dropuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropuser
ms.assetid: e28f18f9-7ecf-4568-89f4-fe5c520df386
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba5b9eb814f48003d93bf6d7b05c2b1c37c1ea76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157146"
---
# <a name="sp_dropuser-transact-sql"></a>sp_dropuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет пользователя из текущей базы данных. **sp_dropuser** обеспечивает совместимость с более ранними версиями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [DROP USER](../../t-sql/statements/drop-user-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_dropuser [ @name_in_db = ] 'user'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @name_in_db = ] 'user'` Имя удаляемого пользователя. *пользователь* имеет тип **sysname** и не имеет значения по умолчанию. *пользователь* должен существовать в текущей базе данных. Указывая имя входа Windows, используйте имя, по которому база данных сможет его определить.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 **sp_dropuser** выполняет **sp_revokedbaccess** , чтобы удалить пользователя из текущей базы данных.  
  
 Для вывода списка имен пользователей, которые могут быть удалены из текущей базы данных, используется **sp_helpuser** .  
  
 Когда удаляется пользователь базы данных, также удаляются все псевдонимы этого пользователя. Если пользователь является владельцем пустой схемы с тем же именем, что и у пользователя, эта схема будет удалена. Если пользователь владеет любыми другими защищаемыми объектами базы данных, то он не будет удален. Необходимо сначала передать права владения объектами другому участнику. Дополнительные сведения см. в разделе [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md). При удалении пользователя базы данных автоматически удаляются разрешения, связанные с пользователем, а пользователь удаляется из всех ролей базы данных, членом которых он является.  
  
 **sp_dropuser** нельзя использовать для удаления владельца базы данных (**dbo**) **INFORMATION_SCHEMA** пользователей или **гостевых** пользователей из баз данных **master** или **tempdb** . В несистемных базах данных `EXEC sp_dropuser 'guest'` будет отозвано разрешение CONNECT для пользователя **Guest**. Однако сам пользователь не будет удален.  
  
 **sp_dropuser** не может быть выполнена в пользовательской транзакции.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение ALTER ANY USER для базы данных.  
  
## <a name="examples"></a>Примеры  
 В следующем примере удаляется пользователь `Albert` из текущей базы данных.  
  
```  
EXEC sp_dropuser 'Albert';  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [DROP USER (Transact-SQL)](../../t-sql/statements/drop-user-transact-sql.md)   
 [sp_revokedbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

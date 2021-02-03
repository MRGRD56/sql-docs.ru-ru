---
description: SETUSER (Transact-SQL)
title: SETUSER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SETUSER_TSQL
- SETUSER
dev_langs:
- TSQL
helpviewer_keywords:
- delegation [SQL Server]
- impersonation [SQL Server]
- SETUSER statement
- user impersonation [SQL Server]
ms.assetid: 7acfac5c-9ad6-4226-b874-7add36c4ea43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3cfa4dfcd316a91fa6224ef4ea9a8807740cca74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189402"
---
# <a name="setuser-transact-sql"></a>SETUSER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Позволяет члену предопределенной роли сервера **sysadmin** или владельцу базы данных олицетворять другого пользователя.  
  
> [!IMPORTANT]  
>  Инструкция SETUSER включена только для обратной совместимости. Инструкция SETUSER может не поддерживаться в следующих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Рекомендуется использовать вместо нее инструкцию [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SETUSER [ 'username' [ WITH NORESET ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 **'** *username* **'**  
 Имя пользователя Windows или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], олицетворение которого выполняется в текущей базе данных. Если аргумент *username* не определен, то восстанавливаются первоначальные учетные данные системного администратора или владельца базы данных, выполняющего олицетворение пользователя.  
  
 WITH NORESET  
 Указывает, что последующие инструкции SETUSER (без указания аргумента *username*) не должны восстанавливать вместо учетных данных пользователя учетные данные системного администратора или владельца базы данных.  
  
## <a name="remarks"></a>Комментарии  
 Инструкция SETUSER может быть использована членом предопределенной роли сервера **sysadmin** или владельцем базы данных для использования учетных данных одного пользователя с целью проверки разрешений другого. Членства в предопределенной роли базы данных db_owner недостаточно.  
  
 Инструкцию SETUSER следует использовать только с пользователями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. SETUSER не поддерживается с пользователями Windows. Когда инструкция SETUSER используется для использования идентификатора другого пользователя, то все объекты, созданные олицетворяющим пользователем, переходят во владение пользователя, олицетворение которого выполняется. Например, если владелец базы данных использует идентификатор пользователя **Margaret** и создает таблицу с названием **orders**, то владельцем таблицы **orders** будет пользователь **Margaret**, а не системный администратор.  
  
 Инструкция SETUSER остается в действии до тех пор, пока не будет выполнена другая инструкция SETUSER или пока текущая база данных не будет изменена оператором USE.  
  
> [!NOTE]  
>  Если используется инструкция SETUSER WITH NORESET, то владелец базы данных или системный администратор должен выйти и снова войти в систему для восстановления своих собственных прав.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом предопределенной роли сервера **sysadmin** или владельцем базы данных. Членства в предопределенной роли базы данных **db_owner** недостаточно  
  
## <a name="examples"></a>Примеры  
 Следующий пример показывает, как владелец базы данных может использовать учетные данные другого пользователя. Пользователь `mary` создала таблицу под названием `computer_types`. Используя SETUSER, владелец базы данных производит олицетворение пользователя `mary` для предоставления пользователю `joe` права доступа к таблице `computer_types`, а затем восстанавливает свои учетные данные.  
  
```sql
SETUSER 'mary';  
GO  
GRANT SELECT ON computer_types TO joe;  
GO  
--To revert to the original user  
SETUSER;  
```  
  
## <a name="see-also"></a>См. также:  
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)  
  
  

---
description: sp_helpsrvrolemember (Transact-SQL)
title: sp_helpsrvrolemember (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpsrvrolemember
- sp_helpsrvrolemember_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpsrvrolemember
ms.assetid: d0714913-8d6b-4de3-b042-3ae9934f839d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f17a4be7d5e9b1511a840857c933ed240d0e3d31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198696"
---
# <a name="sp_helpsrvrolemember-transact-sql"></a>sp_helpsrvrolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о членах предопределенной роли сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helpsrvrolemember [ [ @srvrolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @srvrolename = ] 'role'` Имя предопределенной роли сервера. Аргумент *Role* имеет тип **sysname** и значение по умолчанию NULL. Если *роль* не указана, результирующий набор содержит сведения обо всех предопределенных ролях сервера.  
  
 *роль* может иметь любое из следующих значений.  
  
|Предопределенная роль сервера|Описание|  
|-----------------------|-----------------|  
|sysadmin|Системные администраторы|  
|securityadmin|Администраторы безопасности.|  
|serveradmin|Администраторы сервера.|  
|setupadmin|Администраторы установки.|  
|processadmin|Администраторы процесса.|  
|diskadmin|Администраторы диска.|  
|dbcreator|Создатели баз данных.|  
|bulkadmin|Имеющие разрешение на выполнение инструкции BULK INSERT.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|ServerRole|**sysname**|Имя роли сервера|  
|MemberName|**sysname**|Имя члена роли ServerRole.|  
|MemberSID|**varbinary(85)**|Идентификатор защиты члена роли MemberName.|  
  
## <a name="remarks"></a>Замечания  
 Хранимая процедура sp_helprolemember используется для отображения членов ролей базы данных.  
  
 Все имена входа являются членами общедоступной версии. sp_helpsrvrolemember не распознает общую роль, так как, внутри, не [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] реализует Public как роль.  
  
 Сведения о добавлении или удалении членов из ролей сервера см. в разделе [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 sp_helpsrvrolemember не принимает определяемую пользователем роль сервера в качестве аргумента. Чтобы определить членов определяемой пользователем роли сервера, см. примеры в статье [ALTER Server role &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли public.  
  
## <a name="examples"></a>Примеры  
 В следующем примере перечисляются члены предопределенной роли сервера `sysadmin`.  
  
```  
EXEC sp_helpsrvrolemember 'sysadmin';  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_helprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Функции безопасности &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  

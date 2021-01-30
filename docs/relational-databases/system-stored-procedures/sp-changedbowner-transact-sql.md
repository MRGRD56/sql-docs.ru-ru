---
description: sp_changedbowner (Transact-SQL)
title: sp_changedbowner (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changedbowner
- sp_changedbowner_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changedbowner
ms.assetid: 516ef311-e83b-45c9-b9cd-0e0641774c04
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c11630e73b50eae7c1f972ac7bff1e6d809a464f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159032"
---
# <a name="sp_changedbowner-transact-sql"></a>sp_changedbowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет владельца текущей базы данных.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [инструкцию ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_changedbowner [ @loginame = ] 'login'  
     [ , [ @map = ] remap_alias_flag ]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @loginame =] "*Login*"  
 Идентификатор имени входа нового владельца текущей базы данных. Аргумент *Login* имеет тип **sysname** и не имеет значения по умолчанию. *имя для входа* должно быть уже существующим [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] именем входа или пользователем Windows. *имя входа* не может стать владельцем текущей базы данных, если у нее уже есть доступ к базе данных через существующую учетную запись безопасности пользователя в базе данных. Чтобы избежать этой ситуации, сначала удалите данного пользователя в текущей базе данных.  
  
 [ @map =] *remap_alias_flag*  
 Параметр *remap_alias_flag* является устаревшим, так как псевдонимы для входа были удалены из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Использование параметра *remap_alias_flag* не приводит к ошибке, но не оказывает никакого влияния.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 После выполнения процедуры sp_changedbowner новый владелец становится известным в базе данных как пользователь dbo. Пользователь dbo имеет неявные разрешения на выполнение любых действий в базе данных.  
  
 Владельца системных баз данных master, model или tempdb нельзя изменить.  
  
 Чтобы отобразить список допустимых значений для *входа* , выполните хранимую процедуру sp_helplogins.  
  
 При запуске sp_changedbowner только с параметром *Login* владелец базы данных изменяется на *имя для входа*.  
  
 Можно изменить владельца любого защищаемого объекта с помощью инструкции ALTER AUTHORIZATION. Дополнительные сведения см. в разделе [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение TAKE OWNERSHIP для базы данных. Если новый владелец имеет соответствующего пользователя в базе данных, требуется разрешение IMPERSONATE для имени входа, в противном случае необходимо разрешение CONTROL SERVER для сервера.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано, как сделать имя входа `Albert` владельцем текущей базы данных.  
  
```  
EXEC sp_changedbowner 'Albert';  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [sp_dropalias &#40;Transact-SQL&#41;](./system-stored-procedures-transact-sql.md)   
 [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helpdb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helplogins &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

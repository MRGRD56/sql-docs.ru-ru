---
description: sp_msx_set_account (Transact-SQL)
title: sp_msx_set_account (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_set_account
- sp_msx_set_account_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_set_account
ms.assetid: 314ec720-3a37-48f7-bb6b-8d5b894bf843
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29a92920e81f65beabdc0837b85d19484d2e35f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192004"
---
# <a name="sp_msx_set_account-transact-sql"></a>sp_msx_set_account (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Задает имя и пароль учетной записи агента главного сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на целевом сервере.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_msx_set_account [ @credential_name = ] 'credential_name'  | [ @credential_id = ] credential_id  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @credential_name = ] 'credential_name'` Имя учетных данных, используемых для входа на главный сервер. Указанное имя должно быть именем существующей учетной записи. Необходимо указать либо *credential_name* , либо *credential_id* .  
  
`[ @credential_id = ] credential_id` Идентификатор учетных данных, используемый для входа на главный сервер. Идентификатор должен быть идентификатором существующей учетной записи. Необходимо указать либо *credential_name* , либо *credential_id* .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
 Нет.  
  
## <a name="remarks"></a>Remarks  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует учетные данные для хранения сведений об имени пользователя и пароле, которые целевой сервер использует для подключения к главному серверу. Эта процедура задает учетные данные, которые агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует для входа на главный сервер для этого целевого сервера.  
  
 Указанные учетные данные должны существовать. Дополнительные сведения о создании учетных данных см. в статье [Создание учетных данных &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Разрешения на выполнение **sp_msx_set_account** по умолчанию для членов предопределенной роли сервера **sysadmin** .  
  
## <a name="examples"></a>Примеры  
 Следующий пример устанавливает для сервера учетные данные `MsxAccount` для соединения с главным сервером.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sp_msx_set_account @credential_name = MsxAccount ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Агент SQL Server хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [CREATE CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_msx_get_account &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-msx-get-account-transact-sql.md)  
  
  

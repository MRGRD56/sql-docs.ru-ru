---
description: Хранимая процедура sp_add_proxy (Transact-SQL)
title: sp_add_proxy (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_proxy
- sp_add_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE PROXY statement
- sp_add_proxy
ms.assetid: cb59df37-f103-439b-bec1-2871fb669a8b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c60c3bb1ea9f96352662b72080afbfa1e76b1dce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192401"
---
# <a name="sp_add_proxy-transact-sql"></a>Хранимая процедура sp_add_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Добавляет указанную учетную запись-посредник агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_add_proxy  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @enabled = ] is_enabled ,  
    [ @description = ] 'description' ,  
    [ @credential_name = ] 'credential_name' ,  
    [ @credential_id = ] credential_id ,  
    [ @proxy_id = ] id OUTPUT   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @proxy_name = ] 'proxy_name'` Имя создаваемого прокси-сервера. Аргумент *proxy_name* имеет тип **sysname** и значение по умолчанию NULL. Если *proxy_name* имеет значение null или является пустой строкой, имя прокси-сервера по умолчанию принимает значение *user_name* .  
  
`[ @enabled = ] is_enabled` Указывает, включен ли прокси-сервер. Флаг *is_enabled* имеет тип **tinyint** и значение по умолчанию 1. Если значение *is_enabled* равно **0**, то прокси-сервер не включен и не может использоваться шагом задания.  
  
`[ @description = ] 'description'` Описание прокси-сервера. Описание имеет тип **nvarchar (512)** и значение по умолчанию NULL. Описание позволяет документировать учетную запись-посредника, но оно не используется агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для других целей. Поэтому этот аргумент необязателен.  
  
`[ @credential_name = ] 'credential_name'` Имя учетных данных для прокси-сервера. Аргумент *credential_name* имеет тип **sysname** и значение по умолчанию NULL. Необходимо указать либо *credential_name* , либо *credential_id* .  
  
`[ @credential_id = ] credential_id` Идентификационный номер учетных данных для прокси-сервера. *Credential_id* имеет **тип int** и значение по умолчанию NULL. Необходимо указать либо *credential_name* , либо *credential_id* .  
  
`[ @proxy_id = ] id OUTPUT` Идентификационный номер, назначенный прокси-серверу при успешном создании.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Remarks  
 Эта хранимая процедура должна выполняться в базе данных **msdb** .  
  
 Учетная запись-посредник агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] управляет безопасностью шагов задания, в которых задействованы подсистемы, отличные от подсистемы языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Каждой учетной записи-посреднику соответствует учетная запись системы безопасности. Учетная запись-посредник может иметь доступ к любому числу подсистем.  
  
## <a name="permissions"></a>Разрешения  
 Эту процедуру могут выполнять только члены предопределенной роли безопасности **sysadmin** .  
  
 Члены предопределенной роли безопасности **sysadmin** могут создавать шаги задания, использующие любой прокси-сервер. Используйте хранимую процедуру [sp_grant_login_to_proxy &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md) , чтобы предоставить другим пользователям доступ к учетной записи-посреднику.  
  
## <a name="examples"></a>Примеры  
 В этом примере создается учетная запись-посредник для учетных данных `CatalogApplicationCredential`. Предполагается, что эти учетные данные уже существуют. Дополнительные сведения об учетных данных см. в статье [Создание учетных данных &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md).  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_proxy  
    @proxy_name = 'Catalog application proxy',  
    @enabled = 1,  
    @description = 'Maintenance tasks on catalog application.',  
    @credential_name = 'CatalogApplicationCredential' ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  

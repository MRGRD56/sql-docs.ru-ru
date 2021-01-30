---
description: sp_delete_targetserver (Transact-SQL)
title: sp_delete_targetserver (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_targetserver
- sp_delete_targetserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_targetserver
ms.assetid: cc438701-ad91-419d-9f23-ebc4c548c700
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2f9e77a548e32e32c41d7ed6b45621ac5f4a2594
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199143"
---
# <a name="sp_delete_targetserver-transact-sql"></a>sp_delete_targetserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет указанный сервер из списка доступных целевых серверов.  
   
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_delete_targetserver [ @server_name = ] 'server'   
     [ , [ @clear_downloadlist = ] clear_downloadlist ]  
     [ , [ @post_defection = ] post_defection ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @server_name = ] 'server'` Имя сервера, который необходимо удалить в качестве доступного целевого сервера. *Server* имеет тип **nvarchar (30)** и не имеет значения по умолчанию.  
  
`[ @clear_downloadlist = ] clear_downloadlist` Указывает, следует ли очистить список загрузки для целевого сервера. *clear_downloadlist* имеет тип **bit** и значение по умолчанию **1**. Если *clear_downloadlist* равен **1**, процедура очищает список загрузки для сервера перед удалением сервера. Если значение *clear_downloadlist* равно **0**, список загрузки не удаляется.  
  
`[ @post_defection = ] post_defection` Указывает, следует ли отправлять инструкцию по исключению на целевой сервер. *post_defection* имеет тип **bit** и значение по умолчанию 1. Если *post_defection* равен **1**, процедура отправляет инструкцию по исключению на целевой сервер перед удалением сервера. Если *post_defection* равен **0**, процедура не выполняет инструкцию по исключению на целевом сервере.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Remarks  
 Обычным способом удаления целевого сервера является вызов **sp_msx_defect** на целевом сервере. Используйте **sp_delete_targetserver** только в том случае, если требуется ручное исключение.  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения этой хранимой процедуры пользователям должна быть предоставлена предопределенная роль сервера **sysadmin** .  
  
## <a name="examples"></a>Примеры  
 В следующем примере сервер `LONDON1` удаляется из списка доступных серверов заданий.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_targetserver  
  @server_name = N'LONDON1' ;  
GO  
```  
  
## <a name="see-also"></a>См. также раздел  
 [sp_help_targetserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-targetserver-transact-sql.md)   
 [sp_msx_defect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-msx-defect-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

---
description: sp_subscription_cleanup (Transact-SQL)
title: sp_subscription_cleanup (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_subscription_cleanup_TSQL
- sp_subscription_cleanup
helpviewer_keywords:
- sp_subscription_cleanup
ms.assetid: bdc8aaa0-ff2d-40c2-84b2-4ba513ced279
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 79e44839da9a2df185a125007e7491b6d269d02e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186212"
---
# <a name="sp_subscription_cleanup-transact-sql"></a>sp_subscription_cleanup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Очищает метаданные после удаления подписки на подписчике. Для синхронизирующейся подписки транзакций, кроме того, выполняет триггеры немедленного обновления. Эта хранимая процедура выполняется на подписчике в базе данных подписки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_subscription_cleanup [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
    [ , [ @publication = ] 'publication']  
    [ , [ @reserved = ] 'reserved']  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @publisher_db = ] 'publisher_db'` Имя базы данных издателя. Аргумент *publisher_db* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и значение по умолчанию NULL. Если значение равно NULL, будут удалены все подписки, использующие публикацию общего агента в базе данных публикации.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_subscription_cleanup** используется в репликации транзакций и моментальных снимков.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_subscription_cleanup**.  
  
## <a name="see-also"></a>См. также:  
 [sp_expired_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-expired-subscription-cleanup-transact-sql.md)   
 [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

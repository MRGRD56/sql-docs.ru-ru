---
description: sp_addqreader_agent (Transact-SQL)
title: sp_addqreader_agent (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqreader_agent_TSQL
- sp_addqreader_agent
helpviewer_keywords:
- sp_addqreader_agent
ms.assetid: dc9f591a-e67e-4ba8-bf47-defd5eda0822
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f148297e264a0ca097671234655f9854b7e79aba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206646"
---
# <a name="sp_addqreader_agent-transact-sql"></a>sp_addqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Добавляет агент чтения очереди для указанного распространителя. Эта хранимая процедура выполняется на распространителе в базе данных распространителя или на издателе в базе данных публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addqreader_agent [ @job_login = ] 'job_login'  
        , [ @job_password = ] 'job_password'  
    [ , [ @job_name = ] 'job_name'  
    [ , [ @frompublisher = ] frompublisher   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @job_login = ] 'job_login'` Имя входа для [!INCLUDE[msCoName](../../includes/msconame-md.md)] учетной записи Windows, под которой запускается агент. *job_login* имеет тип **nvarchar (257)** и не имеет значения по умолчанию. Для соединения агента с распространителем всегда используется эта учетная запись Windows.  
  
`[ @job_password = ] 'job_password'` Пароль для учетной записи Windows, под которой запускается агент. Аргумент *job_password* имеет тип **sysname** и не имеет значения по умолчанию.  
  
> [!IMPORTANT]  
>  Не храните данные проверки подлинности в файлах скриптов. Для обеспечения лучшей защиты имена входа и пароли должны вводиться в ходе выполнения.  
  
`[ @job_name = ] 'job_name'` Имя существующего задания агента. Аргумент *job_name* имеет тип **sysname** и значение по умолчанию NULL. Этот аргумент указывается, только если агент создается с использованием существующего, а не вновь созданного задания (выбор по умолчанию).  
  
`[ @frompublisher = ] frompublisher` Указывает, выполняется ли процедура на издателе. *фромпублишер* имеет бит и значение по умолчанию **0**. Значение **1** означает, что процедура выполняется из издателя в базе данных публикации.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 **sp_addqreader_agent** используется в репликации транзакций.  
  
 **sp_addqreader_agent** должны выполняться по крайней мере один раз на распространителе, который поддерживает обновление посредством очередей после [sp_adddistributiondb](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) , но до [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
 Задание агент чтения очереди удаляется при выполнении [sp_dropdistributiondb](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_addqreader_agent**.  
  
## <a name="see-also"></a>См. также:  
 [Включение обновляемых подписок для публикаций транзакций](../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)   
 [Обновление скриптов репликации (программирование репликации на языке Transact-SQL)](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md)   
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_changeqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changeqreader-agent-transact-sql.md)   
 [sp_helpqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpqreader-agent-transact-sql.md)  
  
  

---
description: sp_dropmergepublication (Transact-SQL)
title: sp_dropmergepublication (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergepublication
- sp_dropmergepublication_TSQL
helpviewer_keywords:
- sp_dropmergepublication
ms.assetid: 9e1cb96e-5889-4f97-88cd-f60cf313ce68
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 35024724255eea714de42f8fb3a974188730fcef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208221"
---
# <a name="sp_dropmergepublication-transact-sql"></a>sp_dropmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет публикацию слиянием и ассоциированный с ней агент моментальных снимков. Прежде чем удалять публикацию слиянием, необходимо удалить все подписки. Статьи в публикации удаляются автоматически. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_dropmergepublication [ @publication= ] 'publication'   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
    [ , [ @ignore_merge_metadata = ] ignore_merge_metadata ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя публикации, которую нужно удалить. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию. Если **все** существующие публикации слиянием удалены, а также связанное с ними задание агент моментальных снимков. Если указать конкретное значение для параметра *publication*, то удаляются только эта публикация и связанное с ним задание агент моментальных снимков.  
  
`[ @ignore_distributor = ] ignore_distributor` Используется для удаления публикации без выполнения задач очистки на распространителе. *ignore_distributor* имеет **бит** и значение по умолчанию **0**. Данный аргумент также используется при переустановке распространителя.  
  
`[ @reserved = ] reserved` Зарезервировано для будущего использования. параметр *reserved* имеет значение **bit** и значение по умолчанию **0**.  
  
`[ @ignore_merge_metadata = ] ignore_merge_metadata` Только для внутреннего использования.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_dropmergepublication** используется в репликации слиянием.  
  
 **sp_dropmergepublication** рекурсивно удаляет все статьи, связанные с публикацией, а затем удаляет саму публикацию. Публикацию нельзя удалить, если у нее есть хотя бы одна подписка. Сведения об удалении подписок см. в разделе [Удаление принудительной подписки](../../relational-databases/replication/delete-a-push-subscription.md) и [Удаление подписки по запросу](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
 При запуске **sp_dropmergepublication** для удаления публикации не удаляются опубликованные объекты из базы данных публикации или соответствующие объекты из базы данных подписки. Используйте инструкцию DROP \<object> для удаления этих объектов вручную при необходимости.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_dropmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepublication-_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_dropmergepublication**.  
  
## <a name="see-also"></a>См. также:  
 [Удаление публикации](../../relational-databases/replication/publish/delete-a-publication.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

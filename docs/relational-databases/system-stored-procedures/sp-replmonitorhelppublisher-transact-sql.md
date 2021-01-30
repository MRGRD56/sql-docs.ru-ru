---
description: sp_replmonitorhelppublisher (Transact-SQL)
title: sp_replmonitorhelppublisher (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelppublisher_TSQL
- sp_replmonitorhelppublisher
helpviewer_keywords:
- sp_replmonitorhelppublisher
ms.assetid: 171501fe-4b74-4647-96c3-7691c777e01b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6252047b509af4311045185197994426c1d019ff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198288"
---
# <a name="sp_replmonitorhelppublisher-transact-sql"></a>sp_replmonitorhelppublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Возвращает сведения о текущем состоянии одного или нескольких издателей, связанных с распространителем. Эта хранимая процедура, используемая для наблюдения за репликацией, выполняется на распространителе в базе данных распространителя.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_replmonitorhelppublisher [ [ @publisher = ] 'publisher' ]  
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя, состояние которого отслеживается. параметр *Publisher* имеет тип **sysname** и значение по умолчанию NULL. Если задано значение NULL, то данные будут возвращены для всех издателей, которые используют этого распространителя.  
  
`[ @refreshpolicy = ] refreshpolicy` Только для внутреннего использования.  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Имя издателя.|  
|**distribution_db**|**sysname**|Имя базы данных распространителя, применяемой данным издателем.|  
|**status**|**int**|Максимальное состояние всех агентов репликации, связанных с публикациями этого издателя. Может принимать одно из приведенных ниже значений:<br /><br /> **1** = запущено<br /><br /> **2** = успех<br /><br /> **3** = выполняется<br /><br /> **4** = бездействие<br /><br /> **5** = повторная попытка<br /><br /> **6** = сбой|  
|**warning**|**int**|Максимальный уровень предупреждений, выдаваемых подпиской, принадлежащей публикации этого издателя. Значение может быть результатом операции логического OR над одним или несколькими из следующих значений.<br /><br /> **1** = срок действия истекает — подписка на публикацию транзакций не была синхронизирована в течение порогового срока хранения.<br /><br /> **2** = задержка — время, затраченное на репликацию данных от издателя транзакций подписчику, превышает пороговое значение в секундах.<br /><br /> **4** = mergeexpiration — подписка на публикацию слиянием не была синхронизирована в течение порогового значения срока хранения.<br /><br /> **8** = mergefastrunduration — время, затраченное на выполнение синхронизации подписки на публикацию слиянием, превышает пороговое значение (в секундах) при быстром сетевом подключении.<br /><br /> **16** = mergeslowrunduration — время, затраченное на выполнение синхронизации подписки на публикацию слиянием, превышает пороговое значение (в секундах) при низком или коммутируемом сетевом подключении.<br /><br /> **32** = mergefastrunspeed — скорость доставки строк во время синхронизации подписки на публикацию слиянием не смогла поддерживать пороговое значение скорости (в строках в секунду) при быстром сетевом подключении.<br /><br /> **64** = mergefastrunspeed — скорость доставки строк во время синхронизации подписки на публикацию слиянием не смогла поддерживать пороговое значение скорости (в строках в секунду) по отношению к низкому или коммутируемому сетевому соединению.|  
|**publicationcount**|**int**|Число публикаций, принадлежащих издателю.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_replmonitorhelppublisher** используется со всеми типами репликации.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** на распространителе или члены предопределенных ролей базы данных **db_owner** или **replmonitor** в базе данных распространителя могут выполнять **sp_replmonitorhelppublisher**.  
  
## <a name="see-also"></a>См. также:  
 [Наблюдение за репликацией программным образом](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  

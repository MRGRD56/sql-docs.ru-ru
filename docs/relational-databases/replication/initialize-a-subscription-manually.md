---
description: Инициализация подписки вручную
title: Инициализация подписки вручную | Документация Майкрософт
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- manual subscription initialization [SQL Server replication]
- subscriptions [SQL Server replication], initializing
- initializing subscriptions [SQL Server replication], without snapshots
ms.assetid: 27a1bc38-e498-4fff-8082-04b52aa4b22c
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: bfa7cc36898aa521fed2e0e20fd08a5296147fcf
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "91869224"
---
# <a name="initialize-a-subscription-manually"></a>Инициализация подписки вручную
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  В этом разделе описывается инициализация подписки вручную в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Обычно инициализация подписки производится через исходный моментальный снимок, однако подписку на публикацию можно инициализировать и другим способом. Для этого необходимо, чтобы на подписчике уже имелась схема и исходные данные.  
  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Если в базе данных, опубликованной с использованием репликации транзакций, между копированием данных и схемы на подписчик и ручной инициализацией подписки выполнялись какие-либо действия, то возникшие в результате этих действий изменения данных могут не реплицироваться на подписчик.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Ручная инициализация подписки на публикацию производится путем копирования схемы (а также обычно данных) в базу данных подписки. Схема и данные должны соответствовать базе данных публикации. Затем на странице **Инициализация подписок** мастера создания подписок нужно указать, что для подписки не нужны схема и данные. Дополнительные сведения о доступе к этому мастеру см. в разделах [Инициализация подписки на публикацию транзакций без моментального снимка](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md) и [Создание подписки по запросу](../../relational-databases/replication/create-a-pull-subscription.md).  
  
 При первой синхронизации подписки объекты и метаданные, необходимые для репликации, копируются в базу данных подписки.  
  
#### <a name="to-initialize-a-subscription-to-a-publication-manually"></a>Инициализация подписки на публикацию вручную  
  
1.  Убедитесь, что схема и данные скопированы в базу данных подписки.  
  
2.  Снимите флажок **Инициализировать** на странице **Инициализация подписок** мастера создания подписок. Это действие необходимо выполнить для каждой подписки, требующей копирования только объектов и метаданных репликации.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 Подписки можно инициализированы вручную с помощью хранимых процедур репликации.  
  
#### <a name="to-manually-initialize-a-pull-subscription-to-a-transactional-publication"></a>Ручная инициализация подписки по запросу на публикацию транзакций  
  
1.  Убедитесь, что схема и данные существуют в базе данных подписки. Дополнительные сведения см. в статье [Инициализация подписки на публикацию транзакций без моментального снимка](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
2.  На издателе в базе данных публикации выполните хранимую процедуру [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Задайте параметры **\@publication**, **\@subscriber**, укажите имя базы данных подписчика, содержащей публикуемые данные, в параметре **\@destination_db**, значение **pull** в параметре **\@subscription_type**, а также значение **replication support only** в параметре **\@sync_type**. Дополнительные сведения см. в статье [Создание подписки по запросу](../../relational-databases/replication/create-a-pull-subscription.md).  
  
3.  Выполните процедуру [sp_addpullsubscription](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)на подписчике. Сведения об обновлении подписок см. в разделе [Создание обновляемых подписок для публикаций транзакций](./publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
4.  Выполните процедуру [sp_addpullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)на подписчике. Дополнительные сведения см. в статье [Создание подписки по запросу](../../relational-databases/replication/create-a-pull-subscription.md).  
  
5.  Запустите агент распространителя, чтобы передать объекты репликации и загрузить последние изменения с издателя. Дополнительные сведения см. в статье [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-manually-initialize-a-push-subscription-to-a-transactional-publication"></a>Ручная инициализация принудительной подписки на публикацию транзакций  
  
1.  Убедитесь, что схема и данные существуют в базе данных подписки. Дополнительные сведения см. в статье [Инициализация подписки на публикацию транзакций без моментального снимка](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
2.  На издателе в базе данных публикации выполните хранимую процедуру [sp_addsubscription](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Укажите имя базы данных подписчика, содержащей публикуемые данные, в параметре **\@destination_db**, значение **push** в параметре **\@subscription_type** и значение **replication support only** в параметре **\@sync_type**. Сведения об обновлении подписок см. в разделе [Создание обновляемых подписок для публикаций транзакций](./publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
3.  На издателе в базе данных публикации выполните хранимую процедуру [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Дополнительные сведения см. в статье [Создание принудительной подписки](../../relational-databases/replication/create-a-push-subscription.md).  
  
4.  Запустите агент распространителя, чтобы передать объекты репликации и загрузить последние изменения с издателя. Дополнительные сведения см. в статье [Синхронизация принудительной подписки](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-manually-initialize-a-pull-subscription-to-a-merge-publication"></a>Ручная инициализация подписки по запросу на публикацию слиянием  
  
1.  Убедитесь, что схема и данные существуют в базе данных подписки. Это можно сделать путем восстановления резервной копии базы данных публикации на подписчике.  
  
2.  Выполните процедуру [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)на издателе. Укажите значения параметров **\@publication**, **\@subscriber**, **\@subscriber_db** и значение **pull** в параметре **\@subscription_type**. После этого подписка по запросу будет зарегистрирована.  
  
3.  На подписчике в базе данных, содержащей публикуемые данные, выполните хранимую процедуру [sp_addmergepullsubscription](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). В параметре **\@sync_type** укажите значение **none**.  
  
4.  Выполните процедуру [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)на подписчике. Дополнительные сведения см. в статье [Создание подписки по запросу](../../relational-databases/replication/create-a-pull-subscription.md).  
  
5.  Запустите агент слияния, чтобы передать объекты репликации и загрузить последние изменения с издателя. Дополнительные сведения см. в статье [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-manually-initialize-a-push-subscription-to-a-merge-publication"></a>Ручная инициализация принудительной подписки на публикацию слиянием  
  
1.  Убедитесь, что схема и данные существуют в базе данных подписки. Это можно сделать путем восстановления резервной копии базы данных публикации на подписчике.  
  
2.  В базе данных публикации на издателе выполните процедуру [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md). Укажите имя базы данных на подписчике, содержащей публикуемые данные, в параметре **\@subscriber_db**, значение **push** в параметре **\@subscription_type** и значение **none** в параметре **\@sync_type**.  
  
3.  На издателе в базе данных публикации выполните хранимую процедуру [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md). Дополнительные сведения см. в статье [Создание принудительной подписки](../../relational-databases/replication/create-a-push-subscription.md).  
  
4.  Запустите агент слияния, чтобы передать объекты репликации и загрузить последние изменения с издателя. Дополнительные сведения см. в статье [Синхронизация принудительной подписки](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
## <a name="see-also"></a>См. также  
 [Инициализация подписки на публикацию транзакций без моментального снимка](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)   
 [Создание резервной копии и восстановление из копий реплицируемых баз данных](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [Рекомендации по защите репликации](../../relational-databases/replication/security/replication-security-best-practices.md)  
  

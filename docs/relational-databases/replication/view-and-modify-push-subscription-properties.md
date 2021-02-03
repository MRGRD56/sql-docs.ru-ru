---
description: Просмотр и изменение свойств принудительной подписки
title: Просмотр и изменение свойств принудительной подписки | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- push subscriptions [SQL Server replication], properties
- subscriptions [SQL Server replication], push
- push subscriptions [SQL Server replication], modifying
- modifying replication properties, push subscriptions
- modifying subscriptions, SQL Server Management Studio
ms.assetid: 801d2995-7aa5-4626-906e-c8190758ec71
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 28abdcd83484be06b135252da8bc9af53b98448b
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250551"
---
# <a name="view-and-modify-push-subscription-properties"></a>Просмотр и изменение свойств принудительной подписки
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  В данном разделе описывается просмотр и изменение свойств принудительной подписки в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] при помощи среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]или объектов RMO.  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]

  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Просмотр и изменение свойств принудительной подписки со стороны издателя:  
  
-   В диалоговом окне **Свойства подписки — \<Publisher>: \<PublicationDatabase>** , которое можно открыть из [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   На вкладке **Все подписки** в мониторе репликации. Сведения о запуске монитора репликации см. в [этой статье](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-view-and-modify-push-subscription-properties-in-management-studio"></a>Просмотр и изменение свойств принудительной подписки в среде Management Studio  
  
1.  Подключитесь к издателю в среде [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], а затем раскройте узел сервера.  
  
2.  Раскройте папку **Репликация** , а затем папку **Локальные публикации** .  
  
3.  Раскройте соответствующую публикацию, щелкните правой кнопкой мыши подписку и выберите **Свойства**.  
  
4.  Измените свойства, если необходимо, и нажмите кнопку **ОК**.  
  
#### <a name="to-view-and-modify-push-subscription-properties-in-replication-monitor"></a>Просмотр и изменение свойств принудительной подписки в мониторе репликации  
  
1.  На левой панели монитора репликации раскройте группу издателей, раскройте нужный издатель, а затем выберите публикацию.  
  
2.  Перейдите на вкладку **Все подписки** .  
  
3.  Щелкните правой кнопкой мыши подписку и выберите **Свойства**.  
  
4.  Измените свойства, если необходимо, и нажмите кнопку **ОК**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 Принудительные подписки могут быть изменены программно, кроме того, с помощью хранимых процедур репликации можно программно получить доступ к их свойствам. Хранимые процедуры, используемые для этого, зависят от типа публикации, к которой принадлежит подписка.  
  
#### <a name="to-view-the-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Просмотр свойств принудительной подписки на публикацию моментальных снимков или транзакций  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Укажите параметры **\@publication**, **\@subscriber** и значение **all** в параметре **\@article**.  
  
2.  В издателе в базе данных публикации выполните хранимую процедуру [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), указав параметр **\@subscriber**.  
  
#### <a name="to-change-the-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Изменение свойств принудительной подписки на публикацию моментальных снимков или транзакций  
  
1.  В издателе в базе данных публикации выполните хранимую процедуру [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md), указав параметр **\@subscriber** и любые параметры для свойств подписчика, которые нужно изменить.  
  
2.  На издателе в базе данных публикации выполните хранимую процедуру [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md). Укажите параметры **\@publication**, **\@subscriber**, **\@destination_db**, значение **all** в параметре **\@article**, изменяемое свойство подписки в параметре **\@property** и новое значение в параметре **\@value**. При этом изменятся параметры безопасности для принудительной подписки.  
  
3.  Чтобы изменить свойства пакета служб DTS подписки, выполните хранимую процедуру [sp_changesubscriptiondtsinfo](../../relational-databases/system-stored-procedures/sp-changesubscriptiondtsinfo-transact-sql.md) на подписчике для базы данных подписки (необязательно). Укажите идентификатор задания агента распространения в параметре **\@jobid** и следующие свойства пакета служб DTS:  
  
    -   **\@dts_package_name**;  
  
    -   **\@dts_package_password**;  
  
    -   **\@dts_package_location**.  
  
     Свойства пакета служб подписки будут изменены.  
  
    > [!NOTE]  
    >  Идентификатор задания можно получить, выполнив процедуру [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md).  
  
#### <a name="to-view-the-properties-of-a-push-subscription-to-a-merge-publication"></a>Просмотр свойств принудительной подписки на публикацию слиянием  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_helpmergesubscription](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md). Укажите параметры **\@publication** и **\@subscriber**.  
  
2.  Выполните в издателе процедуру [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), указав параметр **\@subscriber**.  
  
#### <a name="to-change-the-properties-of-a-push-subscription-to-a-merge-publication"></a>Изменение свойств принудительной подписки на публикацию слиянием  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_changemergesubscription](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md). Укажите значения параметров **\@publication**, **\@subscriber**, **\@subscriber_db**, изменяемое свойство подписки в параметре **\@property** и новое значение в параметре **\@value**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> При помощи объектов RMO  
 Какие именно классы объектов RMO для этого применяются, зависит от типа публикации этой подписки.  
  
#### <a name="to-view-or-modify-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Просмотр и изменение свойств принудительной подписки на публикацию моментальных снимков или транзакций  
  
1.  Создайте соединение с издателем с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
3.  Установите свойства <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>и <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> .  
  
4.  Задайте соединение <xref:Microsoft.SqlServer.Management.Common.ServerConnection> с шага 1 для свойства <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Чтобы получить свойства объекта, вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если этот метод возвращает значение **false**, то либо на шаге 3 были неверно определены свойства подписки, либо подписка не существует.  
  
6.  Чтобы изменить свойства, установите новое значение для одного из свойств <xref:Microsoft.SqlServer.Replication.TransSubscription> , которое можно установить, и затем вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> (необязательно).  
  
7.  Чтобы просмотреть новые значения, вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> , который выполняет повторную загрузку свойств для подписки (необязательно).  
  
#### <a name="to-view-or-modify-properties-of-a-push-subscription-to-a-merge-publication"></a>Просмотр и изменение свойств принудительной подписки на публикацию слиянием  
  
1.  Создайте соединение с подписчиком с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
3.  Установите свойства <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>и <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> .  
  
4.  Задайте соединение <xref:Microsoft.SqlServer.Management.Common.ServerConnection> с шага 1 для свойства <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Чтобы получить свойства объекта, вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если этот метод возвращает значение **false**, то либо на шаге 3 были неверно определены свойства подписки, либо подписка не существует.  
  
6.  Чтобы изменить свойства, установите новое значение для одного из свойств <xref:Microsoft.SqlServer.Replication.MergeSubscription> , которое можно установить, и затем вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> (необязательно).  
  
7.  Чтобы просмотреть новые значения, вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> , который выполняет повторную загрузку свойств для подписки (необязательно).  
  
## <a name="see-also"></a>См. также:  
 [Просмотр сведений и выполнение задач с помощью монитора репликации](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Подписка на публикации](../../relational-databases/replication/subscribe-to-publications.md)  
  
  

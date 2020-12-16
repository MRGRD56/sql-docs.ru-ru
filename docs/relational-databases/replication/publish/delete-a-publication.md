---
title: Удаление публикации| Документация Майкрософт
description: Сведения о том, как удалить публикацию в SQL Server с помощью SQL Server Management Studio, Transact-SQL или объектов Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing publications
- publications [SQL Server replication], deleting
- articles [SQL Server replication], deleting
- deleting publications
ms.assetid: 408a1360-12ee-4896-ac94-482ae839593b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 8f149a523960aa0de3aa5b5285cd62c0f1a152f4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477765"
---
# <a name="delete-a-publication"></a>Удаление публикации
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  В данном разделе описывается удаление публикации в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]или объектов RMO.  
  
 **В этом разделе**  
  
-   **Для удаления публикации используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [объекты RMO;](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Удалите публикации из папки **Локальные публикации** в среде [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-delete-a-publication"></a>Удаление публикации  
  
1.  Подключитесь к издателю в среде [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], а затем раскройте узел сервера.  
  
2.  Раскройте папку **Репликация** , а затем папку **Локальные публикации** .  
  
3.  Щелкните правой кнопкой мыши публикацию, которую требуется удалить, и выберите **Удалить**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 Публикации могут быть удалены программно, с помощью хранимых процедур репликации. Какие именно хранимые процедуры для этого применяются, зависит от типа удаляемой публикации.  
  
> [!NOTE]  
>  При удалении публикации опубликованные объекты базы данных публикации и связанные с ними объекты базы данных подписки не удаляются. При необходимости их необходимо удалить вручную, при помощи инструкции `DROP <object>` .  
  
#### <a name="to-delete-a-snapshot-or-transactional-publication"></a>Удаление публикации моментальных снимков или транзакций  
  
1.  Выполните одно из следующих действий.  
  
    -   Для удаления отдельной публикации в базе данных публикации на издателе выполните инструкцию [sp_droppublication](../../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md) .  
  
    -   Чтобы удалить все публикации и удалить все объекты репликации из опубликованной базы данных, выполните процедуру [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) на издателе. Укажите значение **tran** в параметре **\@type**. Если распространитель недоступен или база данных находится в подозрительном состоянии или в режиме "вне сети", укажите значение **1** в параметре **\@force** (необязательно). Укажите имя базы данных в параметре **\@dbname**, если процедура [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) не выполнялась в базе данных публикации (необязательно).  
  
        > [!NOTE]  
        >  Если задать значение **1** в параметре **\@force**, в базе данных могут остаться объекты публикации, связанные с репликацией.  
  
2.  Если база данных содержит только одну публикацию, то для того, чтобы отключить публикацию текущей базы данных с помощью репликации моментальных снимков или транзакций, выполните хранимую процедуру [sp_replicationdboption (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) (необязательно).  
  
3.  (Необязательно) Чтобы удалить все метаданные репликации, оставшиеся в базе данных подписки, на подписчике в базе данных публикации выполните хранимую процедуру [sp_subscription_cleanup](../../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md) .  
  
#### <a name="to-delete-a-merge-publication"></a>Удаление публикации слиянием  
  
1.  Выполните одно из следующих действий.  
  
    -   Чтобы удалить отдельную публикацию, выполните инструкцию [sp_dropmergepublication (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md) в базе данных публикации на издателе.  
  
    -   Чтобы удалить все публикации и удалить все объекты репликации из опубликованной базы данных, выполните процедуру [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) на издателе. Укажите значение **merge** в параметре **\@type**. Если распространитель недоступен или база данных находится в подозрительном состоянии или в режиме "вне сети", укажите значение **1** в параметре **\@force** (необязательно). Укажите имя базы данных в параметре **\@dbname**, если процедура [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) не выполнялась в базе данных публикации (необязательно).  
  
        > [!NOTE]  
        >  Если задать значение **1** в параметре **\@force**, в базе данных могут остаться объекты публикации, связанные с репликацией.  
  
2.  Если база данных содержит только одну публикацию, то для того, чтобы отключить публикацию текущей базы данных с помощью репликации слияния, выполните хранимую процедуру [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) (необязательно).  
  
3.  Чтобы удалить все метаданные репликации, оставшиеся в базе данных подписки, на подписчике в базе данных публикации выполните хранимую процедуру [sp_mergesubscription_cleanup (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md) (необязательно).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 В следующем примере показано удаление публикации транзакций и отключение функции публикации транзакций для базы данных. В этом примере предполагается, что все подписки были удалены ранее. Дополнительные сведения см. в разделе [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) или [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_droppublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_1.sql)]  
  
 В следующем примере показано удаление публикации слиянием и отключение функции публикации слиянием для базы данных. В этом примере предполагается, что все подписки были удалены ранее. Дополнительные сведения см. в разделе [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) или [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_dropmergepublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> При помощи объектов RMO  
 Публикации можно удалять программно с помощью объектов RMO. Классы RMO, используемые, чтобы удалить публикацию, зависят от типа удаляемой публикации.  
  
#### <a name="to-remove-a-snapshot-or-transactional-publication"></a>Удаление публикации моментальных снимков или публикации транзакций  
  
1.  Создайте соединение с издателем с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.TransPublication>.  
  
3.  Задайте для публикации свойства <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> и <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> , а также установите созданное на шаге 1 соединение <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> в качестве значения для свойства.  
  
4.  Чтобы убедиться в существовании публикации, проверьте свойство <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> . Если значение этого свойства равно **false**, то либо на шаге 3 были неверно определены свойства публикации, либо публикация не существует.  
  
5.  Вызовите метод <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Необязательно) Если в базе данных не существует других публикаций транзакций, базу данных можно отключить от публикации транзакций следующим образом.  
  
    1.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. В качестве значения для свойства <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> укажите экземпляр соединения <xref:Microsoft.SqlServer.Management.Common.ServerConnection> , созданный на шаге 1.  
  
    2.  Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если этот метод возвращает значение **false**, убедитесь, что база данных существует.  
  
    3.  Задайте для публикации свойства <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> в значение **false**.  
  
    4.  Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Закройте соединения.  
  
#### <a name="to-remove-a-merge-publication"></a>Удаление публикации слиянием  
  
1.  Создайте соединение с издателем с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.MergePublication>.  
  
3.  Задайте для публикации свойства <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> и <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> , а также установите созданное на шаге 1 соединение <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> в качестве значения для свойства.  
  
4.  Чтобы убедиться в существовании публикации, проверьте свойство <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> . Если значение этого свойства равно **false**, то либо на шаге 3 были неверно определены свойства публикации, либо публикация не существует.  
  
5.  Вызовите метод <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Необязательно) Если в базе данных не существует других публикаций слиянием, базу данных можно отключить от публикации слиянием следующим образом.  
  
    1.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. Присвойте свойству <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> значение экземпляра <xref:Microsoft.SqlServer.Management.Common.ServerConnection> из шага 1.  
  
    2.  Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если этот метод возвращает значение **false**, проверьте, существует ли база данных.  
  
    3.  Задайте для публикации свойства <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> в значение **false**.  
  
    4.  Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Закройте соединения.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Примеры (объекты RMO)  
 В следующем примере удаляется публикация транзакций. Если в этой базе данных не существует других публикаций транзакций, публикация транзакций также отключается.  
  
 [!code-cs[HowTo#rmo_DropTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpub)]  
  
 В следующем примере удаляется публикация слиянием. Если в этой базе данных не существует других публикаций слиянием, публикация слиянием также отключается.  
  
 [!code-cs[HowTo#rmo_DropMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_DropMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropmergepub)]  
  
## <a name="see-also"></a>См. также:  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Публикация данных и объектов базы данных](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  

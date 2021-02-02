---
title: Создание принудительной подписки | Документация Майкрософт
description: Сведения о том, как создать принудительную подписку в SQL Server с помощью SQL Server Management Studio, Transact-SQL или объектов Replication Management Objects.
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- push subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: adfbbc61-58d1-4330-9ad6-b14ab1142e2b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 9cb0106cc23c6c7bbae05528b82ad5d83617cefe
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076176"
---
# <a name="create-a-push-subscription"></a>Создание принудительной подписки
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  В данном разделе описывается процесс создания принудительной подписки в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]или объектов RMO. Сведения о создании принудительной подписки для подписчиков, отличных от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Создание подписки для подписчика, отличного от подписчика SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
 
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
Для создания принудительной подписки в издателе или подписчике используется мастер создания подписки. Следуйте инструкциям на страницах мастера, чтобы выполнить следующие действия:  
  
- Указать издателя и публикацию.  
  
- Выбрать, где будут выполняться агенты репликации. Для принудительной подписки выберите **Выполнять все агенты на распространителе (принудительные подписки)** на странице **Расположение агента распространителя** или на странице **Расположение агента слияния** в зависимости от типа публикации.  
  
- Указать подписчиков и базы данных подписок.  
  
- Указать имена входов и пароли, используемые агентами репликации для подключений:  
  
  - Для подписок на публикации моментальных снимков и публикации транзакций укажите учетные данные на странице **Безопасность агента распространителя** .  
  
  - Для подписок на публикации слиянием укажите учетные данные на странице **Безопасность агента слияния** .  
  
    Дополнительные сведения о разрешениях, необходимых каждому агенту, см. в разделе [Модель безопасности агента репликации](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
- Указать расписание синхронизации и время, когда подписчик должен быть инициализирован.  
  
- Указать дополнительные параметры для публикаций слиянием: тип подписки и значения для параметризованного фильтра.  
  
- Указать дополнительные параметры для публикаций транзакций, позволяющие использовать обновляемые подписки. Один из параметров определяет, должны ли подписчики фиксировать изменения в издателе немедленно или записывать их в очередь. Второй параметр служит для указания учетных данных, используемых для подключения подписчика к издателю.  
  
- При необходимости создайте скрипт для подписки.  
  
#### <a name="to-create-a-push-subscription-from-the-publisher"></a>Создание принудительной подписки с издателя.  
  
1. Подключитесь к издателю в среде [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], а затем раскройте узел сервера.  
  
2. Раскройте папку **Репликация** , а затем папку **Локальные публикации** .  
  
3. Щелкните правой кнопкой мыши публикацию, для которой хотите создать одну или более подписок, а затем выберите пункт **Создать подписку**.  
  
4. Выполните инструкции на страницах мастера создания подписки.  
  
#### <a name="to-create-a-push-subscription-from-the-subscriber"></a>Создание принудительной подписки с подписчика  
  
1. Подключитесь к подписчику в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]и раскройте узел сервера.  
  
2. Раскройте папку **Репликация** .  
  
3. Щелкните правой кнопкой мыши папку **Локальные подписки** и выберите пункт **Создать подписку**.  
  
4. На странице **Публикация** мастера создания подписки выберите **\<Find SQL Server Publisher>** или **\<Find Oracle Publisher>** из раскрывающегося списка **Издатель**.  
  
5. Соединитесь с издателем в диалоговом окне **Соединение с сервером** .  
  
6. Выберите публикацию на странице **Публикация** .  
  
7. Выполните инструкции на страницах мастера создания подписки.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
Принудительные подписки можно создавать программно с помощью хранимых процедур репликации. Какие именно хранимые процедуры будут при этом применяться, зависит от типа публикации, к которой относится подписка.  
  
> [!IMPORTANT]
> По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Создание принудительной подписки на публикацию моментальных снимков или транзакций  
  
1. В издателе в базе данных публикации с помощью процедуры [sp_helppublication](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) проверьте, поддерживает ли публикация принудительные подписки.  
  
   - Если значение **allow_push** равно **1**, то принудительная подписка поддерживается.  
  
   - Если **allow_push** имеет значение **0**, выполните хранимую процедуру [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md). Укажите значение **allow_push** для параметра **\@property** и значение **true** для параметра **\@value**.  
  
2. В издателе в базе данных публикации выполните хранимую процедуру [sp_addsubscription](../system-stored-procedures/sp-addsubscription-transact-sql.md). Укажите значения параметров **\@publication**, **\@subscriber** и **\@destination_db**. Укажите значение **push** в качестве параметра **\@subscription_type**. Сведения об обновлении подписок см. в разделе [Создание обновляемых подписок для публикаций транзакций](publish/create-an-updatable-subscription-to-a-transactional-publication.md).  
  
3. В издателе в базе данных публикации выполните хранимую процедуру [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Укажите следующее.  
  
   - Параметры **\@subscriber**, **\@subscriber_db** и **\@publication**.  
  
   - Учетные данные [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, с которыми будет запускаться агент распространения на распространителе, в качестве значений параметров **\@job_login** и **\@job_password**.  
  
     > [!NOTE]
     > Для соединений, производимых с использованием встроенной проверки подлинности Windows, в параметрах **\@job_login** и **\@job_password** всегда указываются учетные данные Windows. Агент распространения всегда создает локальные соединения с распространителем посредством встроенной проверки подлинности Windows. По умолчанию агент подключается к подписчику с использованием встроенной проверки подлинности Windows.  
  
   - Значение **0** в параметре **\@subscriber_security_mode** и данные для входа [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в параметрах **\@subscriber_login** и **\@subscriber_password** (необязательно). Эти параметры указываются в том случае, если при соединении с подписчиком необходимо использовать проверку подлинности SQL Server.  
  
   - Расписание задания агента распространителя для этой подписки. Дополнительные сведения см. в статье [Указание расписаний синхронизации](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> При создании принудительной подписки в издателе с удаленным распространителем значения, указываемые для всех параметров, включая *job_login* и *job_password*, передаются распространителю в виде обычного текста. Прежде чем выполнять эту хранимую процедуру, необходимо зашифровать соединение между издателем и его удаленным распространителем. Дополнительные сведения см. в статье [Включение шифрования соединений в компоненте Database Engine &#40;диспетчер конфигураций SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>Создание принудительной подписки на публикацию слиянием  
  
1. В издателе в базе данных публикации с помощью процедуры [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) проверьте, поддерживает ли публикация принудительные подписки.  
  
   - Если значение **allow_push** равно **1**, то публикацией принудительные подписки поддерживаются.  
  
   - Если значение **allow_push** не равно **1**, то необходимо выполнить процедуру [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Укажите значение **allow_push** для параметра **\@property** и значение **true** для параметра **\@value**.  
  
2. В базе данных публикации в издателе выполните процедуру [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md). Укажите значения следующих параметров.  
  
   - **\@publication**. Имя публикации.  
  
   - **\@subscriber_type**. Для клиентской подписки укажите **local**. Для серверной — **global**.  
  
   - **\@subscription_priority**. Для серверной подписки укажите приоритет подписки (в диапазоне от **0.00** до **99.99**).  
  
   Дополнительные сведения см. в статье [Расширенное обнаружение и разрешение конфликтов репликации слиянием](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
3. В издателе в базе данных публикации выполните хранимую процедуру [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md). Укажите следующее.  
  
   - Параметры **\@subscriber**, **\@subscriber_db** и **\@publication**.  
  
   - Учетные данные Windows, с которыми будет запускаться агент слияния на распространителе, в качестве параметров **\@job_login** и **\@job_password**.  
  
     > [!NOTE]
     > Для соединений, производимых с использованием встроенной проверки подлинности Windows, в параметрах **\@job_login** и **\@job_password** всегда указываются учетные данные Windows. Агент слияния всегда создает локальные соединения с распространителем с использованием встроенной проверки подлинности Windows. По умолчанию агент подключается к подписчику с использованием встроенной проверки подлинности Windows.  
  
   - Значение **0** в параметре **\@subscriber_security_mode** и данные для входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в параметрах **\@subscriber_login** и **\@subscriber_password** (необязательно). Эти параметры указываются в том случае, если при соединении с подписчиком необходимо использовать проверку подлинности SQL Server.  
  
   - Значение **0** в параметре **\@publisher_security_mode** и данные для входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в параметрах **\@publisher_login** и **\@publisher_password** (необязательно). Эти параметры указываются в том случае, если при соединении с издателем необходимо использовать проверку подлинности SQL Server.  
  
   - Расписание агента слияния для данной подписки. Дополнительные сведения см. в статье [Указание расписаний синхронизации](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
> [!IMPORTANT]
> При создании принудительной подписки в издателе с удаленным распространителем значения, указываемые для всех параметров, включая *job_login* и *job_password*, передаются распространителю в виде обычного текста. Прежде чем выполнять эту хранимую процедуру, необходимо зашифровать соединение между издателем и его удаленным распространителем. Дополнительные сведения см. в статье [Включение шифрования соединений в компоненте Database Engine &#40;диспетчер конфигураций SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 В следующем примере создается принудительная подписка на публикацию транзакций. Значения имени входа и пароля задаются во время выполнения с помощью переменных скриптов **sqlcmd**.  
  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_1.sql)]  
  
 В следующем примере создается принудительная подписка на публикацию слиянием. Значения имени входа и пароля задаются во время выполнения с помощью переменных скриптов **sqlcmd**.  
  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects"></a><a name="RMOProcedure"></a> Использование объектов Replication Management Objects  
 Принудительные подписки можно создавать программно с помощью объектов RMO. Конкретные классы объектов RMO, используемые для этого, зависят от типа публикации, для которой создается подписка.  
  
> [!IMPORTANT]
> По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. Если необходимо хранить учетные данные, используйте [службы шифрования](/previous-versions/aa719848(v=vs.71)), предоставляемые платформой [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework.  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Создание принудительной подписки на публикацию моментальных снимков или транзакций  
  
1. Создайте соединение с издателем с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2. На основе соединения с издателем, созданного на шаге 1, создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.TransPublication> . Задайте свойства <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>и <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3. Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если метод возвращает значение **false**, то либо неверно определены свойства, указанные на шаге 2, либо публикация не существует на сервере.  
  
4. Выполните операцию «поразрядное логическое И» ( **&** в Visual C# и **And** в Visual Basic) над свойством <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> и флагом <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Если результат равен <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, присвойте свойству <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> результат операции «поразрядное логическое ИЛИ» ( **|** в Visual C# и **Or** в Visual Basic) над свойством <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> и <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Затем вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> , чтобы включить принудительные подписки.  
  
5. Если база данных подписки не существует, создайте ее с помощью класса <xref:Microsoft.SqlServer.Management.Smo.Database> . Дополнительные сведения см. в статье [Создание, изменение и удаление баз данных](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
7. Укажите следующие свойства подписки:  
  
   - соединение ( <xref:Microsoft.SqlServer.Management.Common.ServerConnection> ) с издателем, созданное на шаге 1, в свойстве <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>;  
  
   - имя базы данных подписки в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>;  
  
   - имя подписчика в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>; 
  
   - имя базы данных публикации в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>;  
  
   - имя публикации в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>;
  
   - Учетные данные <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> и значение <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> или <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> объекта [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Эта учетная запись будет использоваться для создания локальных соединений с распространителем и удаленных — с использованием проверки подлинности Windows.  
  
     > [!NOTE]
     > Указывать свойство <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> необязательно, если подписка создается членом предопределенной роли сервера **sysadmin**, однако мы рекомендуем это сделать. В этом случае агент будет выполнять олицетворение учетную запись агента SQL Server. Дополнительные сведения см. в статье [Модель безопасности агента репликации](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (необязательно) Значение **true** в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> (задано по умолчанию). Если будет присвоено значение **false**, подписку можно будет синхронизировать только программно.  
  
   - (необязательно) при соединении с подписчиком с проверкой подлинности SQL Server укажите значения свойств <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> и <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> или <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> объекта <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> .  
  
8. Вызовите метод <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> .  
  
> [!IMPORTANT]
> При создании принудительной подписки в издателе с удаленным распространителем значения, заданные для всех свойств, включая <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>, отправляются распространителю в виде обычного текста. Перед вызовом метода <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> следует зашифровать подключение между издателем и его удаленным распространителем. Дополнительные сведения см. в статье [Включение шифрования соединений в компоненте Database Engine &#40;диспетчер конфигураций SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>Создание принудительной подписки на публикацию слиянием  
  
1. Создайте соединение с издателем с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2. На основе соединения с издателем, созданного на шаге 1, создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.MergePublication> . Задайте свойства <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>и <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3. Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если метод возвращает значение **false**, то либо неверно определены свойства, указанные на шаге 2, либо публикация не существует на сервере.  
  
4. Выполните операцию «поразрядное логическое И» ( **&** в Visual C# и **And** в Visual Basic) над свойством <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> и флагом <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Если результат равен <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>, присвойте свойству <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> результат операции «поразрядное логическое ИЛИ» ( **|** в Visual C# и **Or** в Visual Basic) над свойством <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> и <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>. Затем вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> , чтобы включить принудительные подписки.  
  
5. Если база данных подписки не существует, создайте ее с помощью класса <xref:Microsoft.SqlServer.Management.Smo.Database> . Дополнительные сведения см. в статье [Создание, изменение и удаление баз данных](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md).  
  
6. Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
7. Укажите следующие свойства подписки:  
  
   - соединение ( <xref:Microsoft.SqlServer.Management.Common.ServerConnection> ) с издателем, созданное на шаге 1, в свойстве <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>;  
  
   - имя базы данных подписки в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>;  
  
   - имя подписчика в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>;    
   - имя базы данных публикации в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>;  
  
   - имя публикации в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>;    
   - Учетные данные <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> и значение <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> или <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> объекта [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Эта учетная запись будет использоваться для создания локальных соединений с распространителем и удаленных — с использованием проверки подлинности Windows.  
  
     > [!NOTE]
     > Указывать свойство <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> необязательно, если подписка создается членом предопределенной роли сервера **sysadmin**, однако мы рекомендуем это сделать. В этом случае агент будет выполнять олицетворение учетную запись агента SQL Server. Дополнительные сведения см. в статье [Модель безопасности агента репликации](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
   - (необязательно) Значение **true** в свойстве <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> (задано по умолчанию). Если будет присвоено значение **false**, подписку можно будет синхронизировать только программно.  
  
   - (необязательно) при соединении с подписчиком с проверкой подлинности SQL Server укажите значения свойств <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> и <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> или <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> объекта <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> .  
  
   - (необязательно) при соединении с издателем с проверкой подлинности SQL Server укажите значения свойств <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> и <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> или <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> объекта <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> .  
  
8. Вызовите метод <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> .  
  
> [!IMPORTANT]  
> При создании принудительной подписки в издателе с удаленным распространителем значения, заданные для всех свойств, включая <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>, отправляются распространителю в виде обычного текста. Перед вызовом метода <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> следует зашифровать подключение между издателем и его удаленным распространителем. Дополнительные сведения см. в статье [Включение шифрования соединений в компоненте Database Engine &#40;диспетчер конфигураций SQL Server&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Примеры (объекты RMO)  
 В следующем примере создается принудительная подписка на публикацию транзакций. Данные учетной записи Windows для задания агента распространения предоставляются во время выполнения.  
  
 [!code-cs[HowTo#rmo_CreateTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpushsub)]  
  
 В следующем примере создается принудительная подписка на публикацию слиянием. Данные учетной записи Windows для задания агента слияния предоставляются во время выполнения.  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>См. также раздел  
 [Просмотр и изменение свойств принудительной подписки](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Рекомендации по защите репликации](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Создание публикации](../../relational-databases/replication/publish/create-a-publication.md)   
 [Основные понятия объектов RMO](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Синхронизация принудительной подписки](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [Подписка на публикации](../../relational-databases/replication/subscribe-to-publications.md)   
 [Использование программы sqlcmd с переменными скрипта](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  

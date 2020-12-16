---
description: Работа с профилями агента репликации
title: Работа с профилями агента репликации | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], agents and profiles
- replication agent profiles [SQL Server]
- agents [SQL Server replication], profiles
- profiles [SQL Server], replication agents
ms.assetid: 9c290a88-4e9f-4a7e-aab5-4442137a9918
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b21d42777d4561685e714eeb72c07bb1e6fd405b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475875"
---
# <a name="work-with-replication-agent-profiles"></a>Работа с профилями агента репликации
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  В данном разделе описывается работа с профилями агента репликации в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]или объектов RMO. Поведение каждого агента репликации контролируется набором параметров, который может устанавливаться через профили агента. У каждого агента имеются профили по умолчанию, а некоторые агенты имеют дополнительные предопределенные профили. В каждый момент времени активен только один профиль.  
  
 **В этом разделе**  
  
-   **Для работы с профилями агента репликации используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
    -   Доступ к диалоговому окну «Профили агентов»  
  
    -   Указание профиля для агента  
  
    -   Создание профиля  
  
    -   Изменение профиля  
  
    -   Удаление профиля  
  
     [Transact-SQL](#TsqlProcedure)  
  
    -   Создание профиля  
  
    -   Изменение профиля  
  
    -   Удаление профиля  
  
    -   Использование профилей агента при синхронизации  
  
    -   Пример (Transact-SQL)  
  
     [Объекты RMO](#RMOProcedure)  
  
    -   Создание профиля  
  
    -   Изменение профиля  
  
    -   Удаление профиля  
  
-   **Дальнейшие действия.**  [После изменения параметров агента](#FollowUp)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
###  <a name="to-access-the-agent-profiles-dialog-box-from-sql-server-management-studio"></a><a name="Access_SSMS"></a> Доступ к диалоговому окну «Профили агентов» из среды SQL Server Management Studio  
  
1.  На странице **Общие** диалогового окна **Свойства распространителя — \<Distributor>** щелкните элемент **Параметры профиля по умолчанию**.  

#### <a name="to-access-the-agent-profiles-dialog-box-from-replication-monitor"></a>Доступ к диалоговому окну «Профили агентов» из монитора репликации  
  
-   Чтобы открыть диалоговое окно для всех агентов, щелкните издатель правой кнопкой мыши, затем щелкните **Профили агентов**.  
  
-   Чтобы открыть диалоговое окно для одного агента:  
  
    1.  На левой панели монитора репликации раскройте группу издателей, раскройте нужный издатель, а затем выберите публикацию.  
  
    2.  Для доступа к профилям агента распространителя и агента слияния щелкните правой кнопкой мыши подписку на вкладке **Все подписки** , затем щелкните **Профиль агента**. Для других агентов щелкните правой кнопкой мыши агент на вкладке **Агенты** и выберите **Профиль агента**.  
  
###  <a name="to-specify-a-profile-for-an-agent"></a><a name="Specify_SSMS"></a> Указание профиля для агента  
  
1.  Если в окне **Профили агентов** отображаются профили нескольких агентов, следует выбрать агент.  
  
2.  Выберите профиль в столбце **По умолчанию для новых** сетки **Профили агентов** . По умолчанию профиль применяется к агентам только для новых публикаций и подписок.  
  
3.  Чтобы указать, что всем агентам выбранного типа следует использовать для существующих публикаций или подписок данный профиль, щелкните **Изменить существующие агенты**.  
  
###  <a name="to-view-and-edit-the-parameters-associated-with-a-profile"></a><a name="Modify_SSMS"></a> Просмотр и редактирование параметров, связанных с профилем  
  
1.  Если в окне **Профили агентов** отображаются профили нескольких агентов, следует выбрать агент.  
  
2.  Нажмите кнопку свойств ( **…** ), следующую за профилем.  
  
3.  Просмотрите параметры и значения в диалоговом окне **Свойства профиля \<ProfileName>** .  
  
    -   Параметры в пользовательских профилях могут редактироваться, параметры же в предопределенных системных профилях недоступны для изменения.  
  
    -   Для просмотра всех параметров агента снимите флажок **Показывать только параметры, используемые в этом профиле** . К дополнительным сведениям о параметрах агента можно перейти по ссылкам в конце этого раздела.  
  
4.  Щелкните **Закрыть**.  
  
###  <a name="to-create-a-user-defined-profile"></a><a name="Create_SSMS"></a> Создание пользовательского профиля  
  
1.  Если в окне **Профили агентов** отображаются профили нескольких агентов, следует выбрать агент.  
  
2.  Нажмите кнопку **Создать**.  
  
3.  В диалоговом окне инициализации **Создать профиль агента** выберите существующий профиль, который будет служить основой нового профиля.  
  
4.  В диалоговом окне **Создать профиль агента** введите значения в текстовые поля **Имя** и **Описание** .  
  
5.  Чтобы настроить профиль, измените параметры. Для просмотра всех параметров агента снимите флажок **Показывать только параметры, используемые в этом профиле** . К дополнительным сведениям о параметрах агента можно перейти по ссылкам в конце этого раздела.  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="to-delete-a-user-defined-profile"></a><a name="Delete_SSMS"></a> Удаление пользовательского профиля  
  
1.  Если в окне **Профили агентов** отображаются профили нескольких агентов, следует выбрать агент.  
  
2.  Если профиль связан с одним или более агентом, измените профиль для этих агентов:  
  
    1.  Выберите другой профиль в сетке **Профили агентов** .  
  
    2.  Щелкните **Изменить существующие агенты**.  
  
        > [!NOTE]  
        >  В результате изменится профиль всех агентов выбранного типа для существующих публикаций или подписок, а не только тех из них, которые вы намереваетесь удалить.  
  
3.  Выберите профиль, подлежащий удалению, затем нажмите кнопку **Удалить**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_tsql"></a> Создание нового профиля агента  
  
1.  На распространителе выполните хранимую процедуру [sp_add_agent_profile (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md). Задайте параметр **\@name**, значение **1** для **\@profile_type** и одно из следующих значений для **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Если этот профиль станет профилем по умолчанию для агента репликации данного типа, укажите значение **1** для **\@default**. Идентификатор нового профиля можно получить с помощью выходного параметра **\@profile_id**. Таким образом создается новый профиль с набором параметров профиля на основе профиля по умолчанию для данного типа агента.  
  
2.  Когда новый профиль создан, для его настройки можно добавить, удалить или изменить параметры по умолчанию.  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_tsql"></a> Изменение существующего профиля агента  
  
1.  На распространителе выполните хранимую процедуру [sp_help_agent_profile (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). В параметре **\@agent_type** укажите одно из следующих значений:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Будут получены все профили для указанного типа агента. Запомните значение **profile_id** в результирующем наборе для профиля, который требуется изменить.  
  
2.  На распространителе выполните хранимую процедуру [sp_help_agent_parameter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md). В параметре **\@profile_id** укажите полученный в шаге 1 идентификатор. В результате будут возвращены все параметры для профиля. Запомните имена параметров профиля, которые требуется изменить или удалить.  
  
3.  Чтобы изменить значение параметра в профиле, выполните хранимую процедуру [sp_change_agent_parameter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md). Укажите идентификатор профиля из шага 1 в **\@profile_id**, имя изменяемого параметра в **\@parameter_name** и новое значение параметра в **\@parameter_value**.  
  
    > [!NOTE]  
    >  Существующий профиль агента невозможно сделать профилем по умолчанию для агента. Профиль по умолчанию должен быть создан заново, как показано в предыдущей процедуре.  
  
4.  Чтобы удалить параметр из профиля, выполните хранимую процедуру [sp_drop_agent_parameter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md). Укажите идентификатор профиля из шага 1 в **\@profile_id** и имя удаляемого параметра в **\@parameter_name**.  
  
5.  Чтобы добавить новый параметр в профиль, сделайте следующее.  
  
    -   Запросите таблицу [MSagentparameterlist (Transact-SQL)](../../../relational-databases/system-tables/msagentparameterlist-transact-sql.md) на распространителе, чтобы определить, какие параметры профиля можно задать для каждого типа агента.  
  
    -   На распространителе выполните хранимую процедуру [sp_add_agent_parameter (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md). Укажите идентификатор профиля из шага 1 в **\@profile_id**, имя допустимого добавляемого параметра в **\@parameter_name** и значение параметра в **\@parameter_value**.  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_tsql"></a> Удаление профиля агента  
  
1.  На распространителе выполните хранимую процедуру [sp_help_agent_profile (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). В параметре **\@agent_type** укажите одно из следующих значений:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Будут получены все профили для указанного типа агента. Запомните **profile_id** в результирующем наборе для удаляемого профиля.  
  
2.  На распространителе выполните хранимую процедуру [sp_drop_agent_profile (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md). В параметре **\@profile_id** укажите полученный в шаге 1 идентификатор.  
  
###  <a name="to-use-agent-profiles-during-synchronization"></a><a name="Synch_tsql"></a> Использование профилей агента при синхронизации  
  
1.  На распространителе выполните хранимую процедуру [sp_help_agent_profile (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). В параметре **\@agent_type** укажите одно из следующих значений:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Будут получены все профили для указанного типа агента. Запомните значение **profile_name** в результирующем наборе для профиля, который должен использоваться.  
  
2.  Если агент запускается из задания агента, измените шаг запуска в этом задании, указав значение **profile_name** из шага 1 после параметра командной строки **-ProfileName** . Дополнительные сведения см. в статье [View and Modify Replication Agent Command Prompt Parameters](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md) (Просмотр и изменение параметров командной строки агента репликации).  
  
3.  Если агент запускается из командной строки, укажите значение **profile_name** из шага 1 после параметра командной строки **-ProfileName** .  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 В этом примере создается пользовательский профиль для агента слияния с именем **custom_merge**, меняется значение параметра **-UploadReadChangesPerBatch** , добавляется новый параметр **-ExchangeType** и выводятся сведения о созданном профиле.  
  
 [!code-sql[HowTo#sp_addagentprofileparam](../../../relational-databases/replication/codesnippet/tsql/work-with-replication-ag_1.sql)]  
  
##  <a name="using-rmo"></a><a name="RMOProcedure"></a> При помощи объектов RMO  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_RMO"></a> Создание нового профиля агента  
  
1.  Создайте соединение с распространителем с помощью экземпляра класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.AgentProfile>.  
  
3.  Установите следующие свойства объекта.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> — имя профиля.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AgentType%2A> – значение <xref:Microsoft.SqlServer.Replication.AgentType> , указывающее на тип агента репликации, для которого создается профиль.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> – значение <xref:Microsoft.SqlServer.Management.Common.ServerConnection> , созданного на шаге 1.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.Description%2A> – описание профиля (необязательно).  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.Default%2A> — установите для свойства значение **true**, если во всех новых заданиях агентов для <xref:Microsoft.SqlServer.Replication.AgentType> этот профиль будет использоваться по умолчанию (необязательно).  
  
4.  Вызовите метод <xref:Microsoft.SqlServer.Replication.AgentProfile.Create%2A> для создания профиля на этом сервере.  
  
5.  После создания профиля на сервере можно производить его настройку, добавляя, удаляя или изменяя значения параметров агента репликации.  
  
6.  Назначение профиля для существующего задания агента репликации производится методом <xref:Microsoft.SqlServer.Replication.AgentProfile.AssignToAgent%2A> . В качестве параметра *distributionDBName* передайте имя базы данных распространителя, а в параметре *agentID*— идентификатор задания.  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_RMO"></a> Изменение существующего профиля агента  
  
1.  Создайте соединение с распространителем с помощью экземпляра класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Передайте объект <xref:Microsoft.SqlServer.Management.Common.ServerConnection> , созданный на шаге 1.  
  
3.  Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если этот метод возвратил значение **false**, проверьте, существует ли распространитель.  
  
4.  Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumAgentProfiles%2A> . Передайте значение <xref:Microsoft.SqlServer.Replication.AgentType> , чтобы возвращались только те профили, которые предназначены для конкретного типа агента репликации.  
  
5.  Извлеките объект <xref:Microsoft.SqlServer.Replication.AgentProfile> из возвращенного списка <xref:System.Collections.ArrayList>, где свойство <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> объекта соответствует имени профиля.  
  
6.  Чтобы изменить профиль, вызовите один из следующих методов объекта <xref:Microsoft.SqlServer.Replication.AgentProfile> .  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AddParameter%2A> – добавляет к профилю поддерживаемый параметр, где *name* – имя параметра агента репликации, а *value* – заданное значение. Перебор всех поддерживаемых параметров агента для данного типа агента производится методом <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> . Этот метод возвращает список <xref:System.Collections.ArrayList> , содержащий объекты <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> , которые представляют все поддерживаемые параметры.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.RemoveParameter%2A> – удаляет один из существующих параметров из профиля, где *name* – имя параметра агента репликации. Чтобы перечислить все определенные для профиля параметры текущего агента, вызовите метод <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> . Этот метод возвращает список <xref:System.Collections.ArrayList> объектов <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> , которые представляют существующие параметры для данного профиля.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.ChangeParameter%2A> – изменяет значение существующего параметра профиля, где *name* – имя параметра агента, а *newValue* – значение, которое параметр получает после изменения. Чтобы перечислить все определенные для профиля параметры текущего агента, вызовите метод <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> . Этот метод возвращает список <xref:System.Collections.ArrayList> объектов <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> , которые представляют существующие параметры для данного профиля. Перебор всех поддерживаемых параметров агента производится методом <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> . Этот метод возвращает список <xref:System.Collections.ArrayList> , содержащий объекты <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> , которые представляют поддерживаемые значения для всех параметров.  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_RMO"></a> Удаление профиля агента  
  
1.  Создайте соединение с распространителем с помощью экземпляра класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.AgentProfile>. Присвойте имя профиля свойству <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> и значение <xref:Microsoft.SqlServer.Management.Common.ServerConnection> , полученное на шаге 1, свойству <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если метод вернул значение **false**, то имя указано неверно или профиль не существует на сервере.  
  
4.  Удостоверьтесь в том, что свойству <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A> присвоено значение <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.User>, указывающее на клиентский профиль. Не следует удалять профиль, имеющий значение <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.System> в свойстве <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A>.  
  
5.  Вызовите метод <xref:Microsoft.SqlServer.Replication.AgentProfile.Remove%2A> для удаления с сервера пользовательского профиля, представляемого этим объектом.  
  
##  <a name="follow-up-after-changing-agent-parameters"></a><a name="FollowUp"></a> Дальнейшие действия. После изменения параметров агента  
Изменения параметров агента вступают в действие при следующем запуске агента. Если агент выполняется в непрерывном режиме, следует остановить и перезапустить агент. Начиная с SQL Server 2017 с накопительным пакетом обновлений 3 (CU3) некоторые изменения параметров агента вступают в силу без перезагрузки агентов. 
  
## <a name="see-also"></a>См. также:  
 [Профили агента репликации](../../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)   
 [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
  

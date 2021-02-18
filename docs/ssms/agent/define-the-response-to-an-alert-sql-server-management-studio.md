---
description: Определение ответа на предупреждение
title: Определение ответа на предупреждение
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], responding to
- responding to alerts
ms.assetid: c86ca6eb-c59f-46e9-bc32-d474e7c3b170
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 53787a3f08aab02d1a2fee4500570f4033b579fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342709"
---
# <a name="define-the-response-to-an-alert"></a>Определение ответа на предупреждение

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

В этой статье описано, как определить реакцию [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на предупреждения агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Перед началом  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Ограничения  
  
-   Режимы отправки уведомлений с помощью пейджера и команды **net send** будут удалены из агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в следующей версии [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Старайтесь не использовать эти функции в новых разработках и предусмотрите соответствующие изменения в приложениях, которые используют их в настоящее время.  
  
-   Обратите внимание, что для использования компонента Database Mail для отправки операторам уведомлений по электронной почте и на пейджер агент SQL Server необходимо настроить для использования компонента Database Mail. Дополнительные сведения см. в разделе [Назначить предупреждения для оператора](assign-alerts-to-an-operator.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] обеспечивает доступный графический способ управления заданиями и рекомендуется для создания и управления инфраструктурой заданий.  
  
### <a name="security"></a><a name="Security"></a>безопасность  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permissions  
Только члены предопределенной роли сервера **sysadmin** могут задавать отклик на предупреждение.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Использование среды SQL Server Management Studio  
  
#### <a name="to-define-the-response-to-an-alert"></a>Определение ответа на предупреждение  
  
1.  В **обозревателе объектов** щелкните значок «плюс», чтобы развернуть сервер, содержащий предупреждение, для которого необходимо задать отклик.  
  
2.  Щелкните знак "плюс", чтобы развернуть **Агент SQL Server**.  
  
3.  Чтобы развернуть папку **Предупреждения** , щелкните значок "плюс".  
  
4.  Щелкните правой кнопкой предупреждение, для которого необходимо определить отклик, и выберите **Свойства**.  
  
5.  В разделе **Выбор страницы** диалогового окна **Свойства предупреждения**_имя\_предупреждения_ выберите **Отклик**.  
  
6.  Выберите флажок **Выполнить задание** и из списка под пунктом **Выполнить задание** выберите задание, которое необходимо выполнить при возникновении предупреждения. Чтобы выбрать новое задание, нажмите кнопку **Создать задание**. Для получения дополнительных сведений о заданиях нажмите кнопку **Просмотр заданий**. Дополнительные сведения о параметрах, доступных в диалоговых окнах **Создание задания** и **Свойства задания**_имя\_задания_ см. в статьях [Создание задания](../../ssms/agent/create-a-job.md) и [Просмотр задания](../../ssms/agent/view-a-job.md).  
  
7.  Выберите флажок **Уведомлять операторов** , если необходимо уведомлять операторов в момент активации предупреждения. В списке **Список операторов** выберите один или несколько из следующих методов оповещения оператора или операторов: **Электронная почта**, **Пейджер** или **Net send**. Вы можете создать нового оператора, нажав кнопку **Создать оператора**. Вы можете просмотреть дополнительные сведения об операторе, нажав кнопку **Просмотр оператора**. Дополнительные сведения о доступных параметрах в диалоговых окнах **Создать оператора** и **Просмотр свойств оператора** см. в разделах [Create an Operator](../../ssms/agent/create-an-operator.md) и [View Information About an Operator](../../ssms/agent/view-information-about-an-operator.md).  
  
8.  После завершения нажмите кнопку **ОК**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Использование Transact-SQL  
  
#### <a name="to-define-the-response-to-an-alert"></a>Определение ответа на предупреждение  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    -- adds an e-mail notification for Test Alert.  
    -- assumes that Test Alert already exists and that
    -- François Ajenstat is a valid operator name   
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_notification  
     @alert_name = N'Test Alert',  
     @operator_name = N'François Ajenstat',  
     @notification_method = 1 ;  
    GO  
    ```  
  
Дополнительные сведения см. в разделе [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md).
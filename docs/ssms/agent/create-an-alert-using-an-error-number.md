---
description: Создание предупреждения по номеру сообщения
title: Создание предупреждения по номеру сообщения
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server], creating
- SQL Server Agent, alerts
- alerts [SQL Server], error numbers
ms.assetid: 03dd7fac-5073-4f86-babd-37e45a86023c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: aff078228b4b653879966501330a8781a9605585
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271691"
---
# <a name="create-an-alert-using-an-error-number"></a>Создание предупреждения по номеру сообщения
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

В этой статье описано, как создать предупреждения агента [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], возникающее в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] при появлении ошибки с определенным номером, с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Перед началом  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Ограничения  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] обеспечивает простой графический способ управления всей системой предупреждений и рекомендуется для настройки инфраструктуры предупреждений.  
  
-   События, сформированные посредством процедуры **xp_logevent** , появляются в базе данных master. Поэтому **xp_logevent** не вызывает срабатывание предупреждения, если только значение аргумента **\@database_name** для предупреждения не равно **'master'** или NULL.  
  
### <a name="security"></a><a name="Security"></a>безопасность  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permissions  
По умолчанию только члены предопределенной роли сервера **sysadmin** могут выполнять процедуру **sp_add_alert**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Использование среды SQL Server Management Studio  
  
#### <a name="to-create-an-alert-using-an-error-number"></a>Создание предупреждения по номеру сообщения  
  
1.  В **обозревателе объектов** щелкните знак «плюс», чтобы развернуть сервер, на котором необходимо создать предупреждение по номеру ошибки.  
  
2.  Щелкните знак "плюс", чтобы развернуть **Агент SQL Server**.  
  
3.  Щелкните правой кнопкой пункт **Предупреждения** и выберите **Создать предупреждение**.  
  
4.  В поле **Имя** диалогового окна **Создание предупреждения** введите имя этого предупреждения.  
  
5.  Установите флажок **Включено** , чтобы разрешить выдачу предупреждения. По умолчанию флажок **Включить** установлен.  
  
6.  В списке **Тип** выберите **Предупреждение о событии SQL Server**.  
  
7.  В разделе **Определение предупреждения о событии** в списке **Имя базы данных** выберите базу данных для установки ограничения на предупреждение относительно конкретной базы банных.  
  
8.  В разделе **Предупреждение будет выдано на основании** выберите **Номер ошибки**, а затем введите допустимый номер ошибки для предупреждения. Или нажмите кнопку **Серьезность** и выберите определенный уровень серьезности, который вызовет предупреждение.  
  
9. Чтобы ограничить сообщение определенной последовательностью символов, установите флажок в поле **Создавать предупреждение, если сообщение содержит** и введите ключевое слово или строку символов в поле **Текст сообщения**. Максимальное количество символов равно 100.  
  
10. Нажмите кнопку **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Использование Transact-SQL  
  
#### <a name="to-create-an-alert-using-an-error-number"></a>Создание предупреждения по номеру сообщения  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    -- adds an alert (Test Alert) that runs the Back up
    -- the AdventureWorks2012 Database job when fired   
    -- assumes that the message 55001 and the Back up
    -- the AdventureWorks2012 Database job already exist.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert  
        @name = N'Test Alert',  
        @message_id = 55001,   
       @severity = 0,   
       @notification_message = N'Error 55001 has occurred. The DB will be backed up...',   
       @job_name = N'Back up the AdventureWorks2012 Database' ;  
    GO  
    ```  
  
Дополнительные сведения см. в разделе [sp_add_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md).  

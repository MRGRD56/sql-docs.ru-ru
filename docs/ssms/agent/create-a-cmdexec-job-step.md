---
description: Create a CmdExec Job Step
title: Create a CmdExec Job Step
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- CmdExec jobs
ms.assetid: b48da5b4-6fe7-4eb7-bade-dc7d697c6d5c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: a28bc8725a6073f82a62f516968ab26a4f48d784
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355225"
---
# <a name="create-a-cmdexec-job-step"></a>Create a CmdExec Job Step
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

В этой статье описано, как создать и определить шаг задания агента [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], использующий исполняемую программу или команду операционной системы, с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] или управляющих объектов SQL Server.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Перед началом  
  
### <a name="security"></a><a name="Security"></a>безопасность  
По умолчанию только члены предопределенной роли сервера **sysadmin** могут создавать шаги задания CmdExec. Шаги задания службы агента SQL Server запускаются под учетной записью службы агента SQL Server, если пользователь **sysadmin** не создал учетную запись-посредника. Пользователь, не являющийся членом предопределенной роли сервера **sysadmin** может создавать шаги задания CmdExec, только если у него есть доступ к учетной записи-посреднику CmdExec.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permissions  
Дополнительные сведения см. в разделе [Обеспечение безопасности агента SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Использование среды SQL Server Management Studio  
  
#### <a name="to-create-a-cmdexec-job-step"></a>Создание шага задания CmdExec  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]и разверните его.  
  
2.  Разверните **Агент SQL Server**, создайте задание или щелкните правой кнопкой мыши существующее задание и выберите пункт **Свойства**.  
  
3.  В диалоговом окне **Свойства задания** выберите страницу **Шаги** и нажмите кнопку **Добавить**.  
  
4.  В диалоговом окне **Новый шаг задания** введите **имя шага** задания.  
  
5.  В списке **Тип** выберите **Операционная система (CmdExec)** .  
  
6.  В списке **Выполнять как** выберите учетную запись-посредник с учетными данными, используемыми в задании. По умолчанию шаги задания CmdExec выполняются под учетной записью службы агента SQL Server.  
  
7.  В поле **Код завершения процесса успешной команды** введите значение от 0 до 999999.  
  
8.  В поле **Команда** введите команду операционной системы или программу для выполнения. Пример см. в разделе «Использование Transact T-SQL».  
  
9. Выберите вкладку **Дополнительно** , чтобы задать следующие параметры шага задания: действие, которое необходимо выполнить при успешном или неуспешном выполнении шага задания, количество попыток агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнить шаг задания и файл, в который агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может записывать результат выполнения шага задания. Только члены предопределенной роли сервера **sysadmin** могут записывать выходные данные шага задания в файл операционной системы.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Использование Transact-SQL  
  
#### <a name="to-create-a-cmdexec-job-step"></a>Создание шага задания CmdExec  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    -- creates a job step that uses CmdExec  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'CMDEXEC',  
        @command = 'C:\clickme_scripts\SQL11\PostBOLReorg GetHsX.exe',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
Дополнительные сведения см. в разделе [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Использование управляющих объектов SQL Server  
**Создание шага задания CmdExec**  
  
Воспользуйтесь классом **JobStep** на любом языке программирования, таком как Visual Basic, Visual C# или PowerShell.  

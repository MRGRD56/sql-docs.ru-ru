---
description: Create an ActiveX Script Job Step
title: Create an ActiveX Script Job Step
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: e6c46c6b-2d61-4571-bc8e-a831cd6e6302
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1d602a87258cd126d217353e94c0c600870e5a61
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88418230"
---
# <a name="create-an-activex-script-job-step"></a>Create an ActiveX Script Job Step
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](https://docs.microsoft.com/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия между Управляемым экземпляром ManagSQL SQL Azure и SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

В этой статье описано, как создать и определить шаг агента [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], выполняющего скрипт ActiveX, с помощью [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] или управляющих объектов SQL Server.  

**Важно** [!INCLUDEssNoteDepFutureAvoid]
  
-   **Перед началом работы**  
  
    [Ограничения](#Restrictions)  
  
    [Безопасность](#Security)  
  
-   **Для создания шага задания Transact-SQL используется:**  
  
    [Среда SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [Управляющие объекты SQL Server](#SMO)  
  
## <a name="before-you-begin"></a>Перед началом  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Ограничения  
[!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
### <a name="security"></a><a name="Security"></a>безопасность  
Дополнительные сведения см. в разделе [Обеспечение безопасности агента SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Использование среды SQL Server Management Studio  
  
#### <a name="to-create-an-activex-script-job-step"></a>Создание шага задания скрипта ActiveX  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]и разверните его.  
  
2.  Разверните **Агент SQL Server**, создайте задание или щелкните правой кнопкой мыши существующее задание и выберите пункт **Свойства**. Дополнительные сведения о создании заданий см. в разделе [Создание заданий](../../ssms/agent/create-jobs.md).  
  
3.  В диалоговом окне **Свойства задания** выберите страницу **Шаги** и нажмите кнопку **Добавить**.  
  
4.  В диалоговом окне **Новый шаг задания** введите **имя шага**задания.  
  
5.  В списке **Тип** выберите **Скрипт ActiveX**.  
  
6.  В списке **Выполнять как** выберите учетную запись-посредник с учетными данными, используемыми в задании.  
  
7.  Выберите **Язык** , на котором написан скрипт. Или выберите **Другой** и введите имя языка скриптов [!INCLUDE[msCoName](../../includes/msconame_md.md)] ActiveX, на котором будет написан скрипт.  
  
8.  В поле **Команда** введите скрипт, который будет выполняться этим шагом задания. Или нажмите кнопку **Открыть** и выберите файл, содержащий скрипт.  
  
9. Выберите страницу **Дополнительно** , чтобы задать следующие параметры шага задания: какие действия предпринять в случае успешного или неуспешного выполнения шага задания, сколько раз агенту [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пытаться его выполнить и как часто повторять эти попытки.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Использование Transact-SQL  
  
#### <a name="to-create-an-activex-script-job-step"></a>Создание шага задания скрипта ActiveX  
  
1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    -- create an ActiveX Script job step written in VBScript that creates a restore point  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Create a restore point',  
        @subsystem = N'ACTIVESCRIPTING',  
        @command = N'Const RESTORE_POINT = 20  
  
    strComputer = "."  
    Set objWMIService = GetObject("winmgmts:" _  
        & "{impersonationLevel=impersonate}!\\" & strComputer & "\root\default")  
  
    Set objItem = objWMIService.Get("SystemRestore")  
    errResults = objItem.Restore(RESTORE_POINT)',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
Дополнительные сведения см. в разделе [sp_add_jobstep (Transact-SQL)](https://msdn.microsoft.com/97900032-523d-49d6-9865-2734fba1c755).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Использование управляющих объектов SQL Server  
**Создание шага задания скрипта ActiveX**  
  
Воспользуйтесь классом **JobStep** на любом языке программирования, таком как Visual Basic, Visual C# или PowerShell.  
  

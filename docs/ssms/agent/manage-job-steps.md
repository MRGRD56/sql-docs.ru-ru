---
description: Управление шагами задания
title: Управление шагами задания
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server replication]
- job steps [SQL Server Agent]
- jobs [SQL Server Agent], Integration Services package step
- executable programs as job steps
- operating systems [SQL Server], job steps
- Transact-SQL job step
- job steps [Transact-SQL]
- Integration Services packages, job steps
- replication job steps [SQL Server]
- logs [SQL Server], jobs
- SQL Server Agent jobs, job steps
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: 51352afc-a0a4-428b-8985-f9e58bb57c31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8892ff46f9c6c9fd382b39284cbc4bc81449a5d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355186"
---
# <a name="manage-job-steps"></a>Управление шагами задания
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Шаг задания — это действие, производимое заданием над базой данных или сервером. Каждое задание должно иметь, по крайней мере, один шаг. Шагами задания могут быть:  
  
-   Исполняемые программы и команды операционной системы.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции, включающие хранимые процедуры и расширенные хранимые процедуры.  
  
-   Скрипты PowerShell.  
  
-   [!INCLUDE[msCoName](../../includes/msconame_md.md)] скрипты ActiveX.  
  
-   Задачи репликации.  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] задачи.  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] пакеты.  
  
Каждый шаг задания выполняется в определенном контексте безопасности. Если шаг задания указывает учетную запись-посредник, он выполняется в контексте безопасности учетных данных для учетной записи-посредника. Если шаг задания не указывает учетную запись-посредник, этот шаг выполняется в контексте учетной записи службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Только члены предопределенной роли сервера "sysadmin" могут создавать задания, которые не указывают учетную запись-посредник явным образом.  
  
Так как шаги задания выполняются в контексте определенного пользователя [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows, этот пользователь должен иметь разрешения и конфигурацию, необходимые для выполнения шага. Например, если создается задание, в котором требуется буква диска или путь в формате UNC, шаги задания могут выполняться под учетной записью пользователя Windows во время проверки задач. Однако для шага задания пользователь Windows должен иметь необходимые разрешения, конфигурации буквы диска или доступ к требуемому диску. В противном случае шаг задания завершится ошибкой. Во избежание этой проблемы нужно, чтобы учетная запись-посредник для каждого шага задания имела необходимые разрешения для задачи, которую выполняет шаг задания. Дополнительные сведения см. в разделе [Защита и обеспечение безопасности (компонент Database Engine)](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).  
  
## <a name="job-step-logs"></a>Журналы шагов задания  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Агент может записывать выходные данные из определенных шагов задания в файл операционной системы или в таблицу "sysjobstepslogs" базы данных "msdb". Следующие шаги задания могут записывать выходные данные в оба адресата:  
  
-   Исполняемые программы и команды операционной системы.  
  
-   Инструкции[!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] задачи.  
  
Только шаги задания, которые выполняются пользователями, являющимися членами предопределенной роли сервера "sysadmin", могут записывать выходные данные шагов задания в файлы операционной системы. Если шаги задания выполняются пользователями, которые являются членами предопределенной роли базы данных SQLAgentUserRole, SQLAgentReaderRole или SQLAgentOperatorRole в базе данных "msdb", то выходные данные этих шагов задания могут быть записаны только в таблицу "sysjobstepslogs".  
  
Журналы шагов задания автоматически удаляются при удалении заданий или шагов заданий.  
  
> [!NOTE]  
> Задача репликации и ведение журнала шагов задания пакета служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] осуществляется соответствующей подсистемой. Чтобы настроить регистрацию шагов задания для этих типов шагов задания, нельзя использовать агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="executable-programs-and-operating-system-commands-as-job-steps"></a>Исполняемые программы и команды операционной системы в качестве шагов задания  
В качестве шагов задания можно использовать исполняемые программы и команды операционной системы. Эти файлы могут иметь расширения BAT, CMD, COM и EXE.  
  
При использовании исполняемой программы или команды операционной системы в качестве шага задания необходимо указать следующее.  
  
-   Код завершения процесса, возвращаемый при успешном выполнении команды.  
  
-   Команда для выполнения. Чтобы выполнить команду операционной системы, необходимо просто выполнить саму команду. Для внешней программы это ее имя и аргументы, например: **C:\Program Files\Microsoft SQL Server\100\Tools\Binn\sqlcmd.exe -e -q "sp_who"**  
  
    > [!NOTE]  
    > Необходимо предоставить полный путь к исполняемой программе, если только она не размещена в каталоге, указанном в системном пути или в пути пользователя, от имени которого выполняется шаг задания.  
  
## <a name="transact-sql-job-steps"></a>Шаги задания Transact-SQL  
При создании шага задания [!INCLUDE[tsql](../../includes/tsql-md.md)] необходимо выполнить следующие действия.  
  
-   Указать базу данных, в которой нужно выполнить задание.  
  
-   Ввести инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] для выполнения. Инструкция может вызывать хранимую процедуру или расширенную хранимую процедуру.  
  
При необходимости можно открыть существующий файл [!INCLUDE[tsql](../../includes/tsql-md.md)] в качестве команды для шага задания.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] в шагах задания не используются учетные записи-посредники агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Вместо этого шаг задания выполняется как владелец шага задания или как учетная запись службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , если владелец шага задания является членом предопределенной роли сервера "sysadmin". Члены предопределенной роли сервера "sysadmin" также могут указывать, что шаги задания [!INCLUDE[tsql](../../includes/tsql-md.md)] выполняются в контексте другого пользователя, с помощью параметра *database_user_name* хранимой процедуры "sp_add_jobstep stored". Дополнительные сведения см. в разделе [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
> [!NOTE]  
> Один шаг задания [!INCLUDE[tsql](../../includes/tsql-md.md)] может содержать несколько пакетов. [!INCLUDE[tsql](../../includes/tsql-md.md)] шаги задания могут содержать внедренные команды GO.  
  
## <a name="powershell-scripting-job-steps"></a>Шаги заданий со скриптами PowerShell  
При создании шага задания со скриптом PowerShell необходимо указать в качестве команды для шага одно из двух.  
  
-   Текст скрипта PowerShell.  
  
-   Существующий файл скрипта PowerShell.  
  
Подсистема агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell открывает сеанс PowerShell и загружает оснастки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Скрипт PowerShell, используемый в качестве команды для шага задания, может обращаться к поставщику [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell и командлетам. Дополнительные сведения о написании скриптов PowerShell с помощью оснасток [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell см. в разделе [SQL Server PowerShell](../../powershell/sql-server-powershell.md).  
  
## <a name="activex-scripting-job-steps"></a>Шаги задания скрипта ActiveX  
  
> [!IMPORTANT]
> Шаг задания скрипта ActiveX будет удален из агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в следующей версии [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется.  
  
При создании шага задания скрипта ActiveX необходимо:  
  
-   Указать язык скрипта, на котором будет записан шаг задания.  
  
-   Записать скрипт ActiveX.  
  
Также можно открыть существующий файл скрипта ActiveX как команду для шага задания. В противном случае команды скрипта ActiveX могут быть скомпилированы с помощью внешних средств (например, при помощи Microsoft Visual Basic), а затем выполнены как исполняемые программы.  
  
Если команда шага задания является скриптом ActiveX, можно использовать объект "SQLActiveScriptHost" для печати выходных данных в журнал шагов задания или для создания объектов COM. "SQLActiveScriptHost" — это глобальный объект, введенный системой размещения агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в пространство имен скрипта. Объект содержит два метода (Print и CreateObject). Представленный пример показывает, как скрипт ActiveX работает в Visual Basic Scripting Edition (VBScript).  
  
```  
' VBScript example for ActiveX Scripting job step  
' Create a Dmo.Server object. The object connects to the  
' server on which the script is running.  
  
Set oServer = CreateObject("SQLDmo.SqlServer")  
oServer.LoginSecure = True  
oServer.Connect "(local)"  
'Disconnect and destroy the server object  
oServer.DisConnect  
Set oServer = nothing  
```  
  
## <a name="replication-job-steps"></a>Шаги задания репликации  
При создании публикаций и подписок с помощью репликации задания репликации создаются по умолчанию. Тип создаваемого задания определяется типом репликации (моментальный снимок, транзакционная репликация или репликация слиянием) и используемыми параметрами.  
  
Шаги задания репликации активируют один из следующих агентов репликации:  
  
-   Агент моментальных снимков (задание Snapshot)  
  
-   Агент чтения журнала (задание LogReader)  
  
-   Агент распространителя (задание Distribution)  
  
-   Агент слияния (задание Merge)  
  
-   Агент чтения очереди (задание QueueReader)  
  
Настроив репликацию, можно указать выполнение агентов репликации одним из следующих способов: постоянно после запуска агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , по запросу или по расписанию. Дополнительные сведения об агентах репликации см. в разделе [Обзор агентов репликации](../../relational-databases/replication/agents/replication-agents-overview.md).  
  
## <a name="analysis-services-job-steps"></a>Шаги задания служб Analysis Services  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Агент поддерживает два определенных типа шагов заданий служб Analysis Services: шаги задания команды и шаги задания запроса.  
  
### <a name="analysis-services-command-job-steps"></a>Шаги задания команды служб Analysis Services  
При создании шага задания служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] необходимо выполнить следующее.  
  
-   Определить сервер базы данных OLAP, на котором необходимо выполнить шаг задания.  
  
-   Ввести инструкцию, которую необходимо выполнить. Для метода  **Execute** служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] инструкция должна быть в формате XML. Инструкция может не содержать полный конверт SOAP или XML-файл для метода  **Discover** служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)]. Обратите внимание, что в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] поддерживаются полные конверты SOAP и метод **Discover** , но поддержки шагов заданий агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] нет.  
  
### <a name="analysis-services-query-job-steps"></a>Шаги задания запроса служб Analysis Services  
При создании шага задания с запросом служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)] необходимо:  
  
-   Определить сервер базы данных OLAP, на котором необходимо выполнить шаг задания.  
  
-   Ввести инструкцию, которую необходимо выполнить. Эта инструкция должна быть запросом многомерных выражений (MDX).  
  
Дополнительные сведения о многомерных выражениях см. в разделе [Общие сведения об инструкциях многомерных выражений](/analysis-services/multidimensional-models/mdx/mdx-query-fundamentals-analysis-services?viewFallbackFrom=sql-server-ver15).  
  
## <a name="integration-services-packages"></a>Пакеты служб Integration Services  
При создании шага задания с пакетом служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] необходимо выполнить следующее.  
  
-   Указать источник пакета.  
  
-   Указать размещение пакета.  
  
-   Указать файлы конфигурации, если они необходимы для пакета.  
  
-   Указать файлы команд, если они необходимы для пакета.  
  
-   Указать необходимую для пакета проверку. Например, можно указать, что пакет должен быть подписанным или иметь определенный идентификатор пакета.  
  
-   Указать источники данных для пакета.  
  
-   Указать регистраторы для пакета.  
  
-   Указать переменные и значения, которые необходимо установить до запуска пакета.  
  
-   Указать параметры выполнения.  
  
-   Добавить или изменить параметры командной строки.  
  
Обратите внимание, что если пакет был развернут в каталоге служб SSIS и в качестве источника пакета был указан **Каталог служб SSIS** , значительная часть этих конфигурационных данных берется автоматически из пакета. На вкладке **Конфигурация** можно указать среду, значения параметров, значения диспетчера подключений, переопределения свойств и выполняется ли пакет в 32-разрядной среде.  
  
Дополнительные сведения о создании шагов заданий, которые выполняют пакеты служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , см. в разделе [Пакеты служб из заданий агента SQL Server](../../integration-services/packages/sql-server-agent-jobs-for-packages.md).  
  
## <a name="related-tasks"></a>Связанные задачи  
  
|Описание|Раздел|  
|-|-|  
|Описывает создание шага задания с помощью исполняемой программы.|[Create a CmdExec Job Step](../../ssms/agent/create-a-cmdexec-job-step.md)|  
|Описывает, как сбросить разрешения агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Настройка пользователя для создания заданий агента SQL Server и управления заданиями](../../ssms/agent/configure-a-user-to-create-and-manage-sql-server-agent-jobs.md)|  
|Описывает создание шага задания [!INCLUDE[tsql](../../includes/tsql-md.md)] .|[Создание шага задания Transact-SQL](../../ssms/agent/create-a-transact-sql-job-step.md)|  
|Описывает определение параметров для шагов заданий Transact-SQL агента Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Определение параметров для шагов заданий Transact-SQL](../../ssms/agent/define-transact-sql-job-step-options.md)|  
|Описывает создание шага задания скрипта ActiveX.|[Create an ActiveX Script Job Step](../../ssms/agent/create-an-activex-script-job-step.md)|  
|Описывает процесс создания и определения шагов заданий агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполняющих команды и запросы служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services.|[Create an Analysis Services Job Step](../../ssms/agent/create-an-analysis-services-job-step.md)|  
|Описывает, какое действие будет выполнять [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , если при выполнении задания происходит ошибка.|[Настройка потока действий системы при успешном или неуспешном выполнении шага задания](../../ssms/agent/set-job-step-success-or-failure-flow.md)|  
|Описывает, как просмотреть сведения о шаге задания в окне «Свойства шага задания».|[View Job Step Information](../../ssms/agent/view-job-step-information.md)|  
|Описывает, как удалить журнал шага задания агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Delete a Job Step Log](../../ssms/agent/delete-a-job-step-log.md)|  
  
## <a name="see-also"></a>См. также:  
[sysjobstepslogs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
[Создание заданий](../../ssms/agent/create-jobs.md)  
[sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  

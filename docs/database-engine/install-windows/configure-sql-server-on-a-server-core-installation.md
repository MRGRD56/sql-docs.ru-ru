---
title: Настройка установки Server Core
description: В этой статье приводятся сведения о настройке SQL Server в установке Server Core, а также информация о средствах устранения неполадок.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- IsHadrEnabled server property
- Server Core Installation [SQL Server]
ms.assetid: ed6e5e94-4b8d-422a-a17e-61b05a4df903
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 300e06b7eaab03af63685e495d8aa0de2ad83ed6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336391"
---
# <a name="configure-sql-server-on-a-server-core-installation"></a>Настройка SQL Server на установке Server Core

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Эта статья содержит сведения о настройке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в установке Server Core.  

##  <a name="configure-and-manage-server-core-on-windows-server"></a><a name="BKMK_ConfigureWindows"></a> Настройка и управление Server Core в Windows Server  
Раздел содержит ссылки на статьи, в которых приведены сведения о настройке установки Server Core и управлении ею.  
  
Не все компоненты [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] поддерживаются в режиме Server Core.  Некоторые из этих компонентов могут быть установлены на клиентском компьютере или другом сервере, на котором нет Server Core, и подключены к службам компонента ядра СУБД, установленным в Server Core.  
  
Дополнительные сведения о дистанционной настройке установкой Server Core и управлении еюсм. в следующих статьях:  
  
- [Установка Server Core](/windows-server/get-started/getting-started-with-server-core)  
  
- [Настройка установки Server Core системы Windows Server 2016 с помощью Sconfig.cmd](/windows-server/get-started/sconfig-on-ws2016)  
  
- [Установка ролей и компонентов в Windows Server 2012 R2 с Server Core](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574158(v=ws.11))
  
- [Управление установкой Server Core. Обзор](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441255(v=ws.10))  
  
- [Администрирование установки Server Core](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441258(v=ws.10))
  
##  <a name="install-ssnoversion-updates"></a><a name="BKMK_InstallSQLUpdates"></a> Установка обновлений [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
Этот раздел содержит сведения об установке обновлений для [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] на компьютере под управлением Windows Server Core. Пользователям рекомендуется своевременно оценивать и устанавливать последние обновления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , чтобы обеспечить наличие последних обновлений безопасности для систем. Дополнительные сведения об установке [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] на компьютере под управлением Windows Server Core см. в разделе [Установка SQL Server на Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md).  
  
Ниже приведены два сценария для установки обновлений продукта.  
  
- [Установка обновлений для SQL Server при установке нового экземпляра](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_NewInstall)  
  
- [Установка обновлений для SQL Server после установки экземпляра](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_alreadyInstall)  
  
###  <a name="installing-updates-for-ssnoversion-during-a-new-installation"></a><a name="bkmk_NewInstall"></a> Установка обновлений для [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] при установке нового экземпляра  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает только установку из командной строки в операционной системе Server Core. Дополнительные сведения см. в разделе [Установка SQL Server из командной строки](./install-sql-server-from-the-command-prompt.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] объединяет последние обновления продукта с установкой основного продукта, чтобы он и применимые обновления устанавливались одновременно.  
  
Когда программа установки обнаруживает последние версии соответствующих обновлений, эти обновления загружаются и интегрируются в текущую процедуру установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Функция обновления продукта может включить в установку пакет обновления, накопительное обновление или и то и другое.  
  
Укажите параметры UpdateEnabled и UpdateSource, чтобы ввести последние обновления продукта в установку основного продукта. В следующем примере показано, как выполнить обновления продукта в процессе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /UpdateEnabled=True /UpdateSource="<SourcePath>" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
###  <a name="installing-updates-for-ssnoversion-after-it-has-been-installed"></a><a name="bkmk_alreadyInstall"></a> Установка обновлений для [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] после установки экземпляра.  
В установленном экземпляре [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]рекомендуется установить последние обновления безопасности и критические обновления, в том числе выпуски для общего распространения (GDR) и пакеты обновления (SP). Отдельные накопительные обновления и обновления безопасности следует устанавливать в каждом отдельном случае по мере необходимости. Оцените необходимость обновления и установите его, если это требуется.  
  
Применение обновления из командной строки. Замените <имя_пакета> именем конкретного пакета обновления:  
  
- Обновление одного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и всех общих компонентов. Можно указать экземпляр с помощью параметра InstanceName или параметра InstanceID.  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceName=MyInstance  
    ```  
  
- Обновление только общих компонентов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch  
    ```  
  
- Обновление всех экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на компьютере и всех общих компонентов.  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /AllInstances  
    ```  
  
## <a name="startstop-ssnoversion-service"></a><a name="BKMK_StartStopServices"></a> Запуск и остановка службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
[Приложение sqlservr](../../tools/sqlservr-application.md) позволяет запускать, останавливать, приостанавливать и возобновлять работу экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из командной строки.  
  
Для запуска и остановки служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно также использовать службы Net.  
  
## <a name="enable-alwayson-availability-groups"></a><a name="BKMK_EnableAlwaysON"></a> Включение групп доступности AlwaysOn  
Включение групп доступности AlwaysOn является предварительным требованием для экземпляра сервера, чтобы использовать группы доступности в качестве решения высокого уровня доступности и аварийного восстановления. Дополнительные сведения об управлении группами доступности AlwaysOn см. в разделе [Включение и отключение групп доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).  
  
### <a name="using-ssnoversion-configuration-manager-remotely"></a>Удаленное использование диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
Эти действия предназначены для выполнения на компьютере клиента, работающего под управлением Windows или Windows Server, где установлена графическая оболочка сервера.  
  
1. Откройте **Управление компьютером**. Чтобы открыть **Управление компьютером**, нажмите кнопку **Пуск**, введите `compmgmt.msc` и нажмите кнопку **ОК**.    
  
2. В дереве консоли щелкните правой кнопкой мыши **Управление компьютером**, а затем выберите **Подключиться к другому компьютеру...**  
  
3. В диалоговом окне **Выбор компьютера** введите имя компьютера Server Core, которым необходимо управлять, или нажмите кнопку **Обзор**, чтобы найти его, а затем нажмите кнопку **ОК**.  
  
4. В дереве консоли в разделе **Управление компьютером** компьютера Server Core выберите **Службы и приложения**.  
  
5. Дважды щелкните диспетчер конфигурации **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** .  
  
6. В **диспетчере конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** выберите элемент **Службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** , правой кнопкой мыши щелкните **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** (\<instance name>), где \<instance name> — это имя локального экземпляра сервера, для которого нужно включить группы доступности AlwaysOn, а затем выберите пункт "Свойства".  
  
7. Перейдите на вкладку **Высокий уровень доступности AlwaysOn** .  
  
8. Убедитесь, что поле Имя отказоустойчивого кластера Windows содержит имя локального узла отказоустойчивого кластера. Если это поле не заполнено, значит в настоящее время этот экземпляр сервера не поддерживает группы доступности AlwaysOn. Локальный компьютер не является узлом кластера, кластер WSFC завершил работу либо этот выпуск [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] не поддерживает группы доступности Always On.  
  
9. Установите флажок «Включить группы доступности AlwaysOn» и нажмите кнопку «ОК».  
  
10. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сохранит внесенные изменения. После этого необходимо вручную перезапустить службу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Это позволит выбрать время перезапуска, которое лучше всего подходит под требования вашего предприятия. После перезапуска службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] функция AlwaysOn будет включена, а свойство IsHadrEnabled будет установлено в значение 1.  
  
> [!NOTE]
>  -   Чтобы подключиться к этому компьютеру, необходимо иметь соответствующие разрешения пользователя или получить полномочия на целевом компьютере от соответствующего источника.  
> -   Имя управляемого компьютера отображается в скобках рядом с элементом «Управление компьютером» в дереве консоли.  
  
### <a name="using-powershell-cmdlets-to-enable-alwayson-availability-groups"></a>Использование командлетов PowerShell для активации групп доступности AlwaysOn  
Командлет PowerShell, Enable-SqlAlwaysOn, используется для активации групп доступности AlwaysOn на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если группы доступности AlwaysOn включаются во время работы службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , то для вступления изменения в силу необходимо перезапустить службу компонента ядра СУБД. Если не указан параметр -Force, командлет запрашивает, следует ли перезапустить службу. В случае отказа никаких действий не предпринимается.  
  
Для выполнения этого командлета необходимо иметь разрешения администратора.  
  
Для активации групп доступности AlwaysOn в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]можно использовать один из следующих вариантов синтаксиса.  
  
```  
Enable-SqlAlwaysOn [-Path <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn -InputObject <Server> [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn [-ServerInstance <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
Следующая команда PowerShell активирует группы доступности AlwaysOn на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (компьютер или экземпляр).  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Machine\Instance  
```  
  
##  <a name="configuring-remote-access-of-ssnoversion-running-on-server-core"></a><a name="BKMK_ConfigureRemoteAccess"></a> Настройка удаленного доступа для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при работе на Server Core  
 Чтобы настроить удаленный доступ к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], который запускается в Windows Server Core, выполните действия, описанные ниже.  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>Включение удаленных подключений на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Чтобы разрешить удаленные соединения, выполните следующие инструкции для экземпляра Server Core в локальной программе SQLCMD.exe.  
  
-   `EXEC sys.sp_configure N'remote access', N'1'`  
  
     `GO`  
  
-   `RECONFIGURE WITH OVERRIDE`  
  
     `GO`  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>Включите и запустите службу браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 По умолчанию эта служба отключена.  Если она отключена на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , работающем на Server Core, то для ее включения выполните следующую команду из командной строки:  
  
 `sc config SQLBROWSER start= auto`  
  
 После включения службы выполните следующую команду из командной строки, чтобы запустить службу:  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>Создание исключений в брандмауэре Windows  
 Чтобы создать исключения в брандмауэре Windows для доступа к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполните действия, описанные в разделе [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>Включите поддержку TCP/IP в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Протокол TCP/IP для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Server Core можно включить через Windows PowerShell. Выполните следующие действия.  
  
1.  На компьютере, где запущена ОС Windows Server Core, запустите **Диспетчер задач**.  
  
2.  На вкладке **Приложения** нажмите **Создать задачу**.  
  
3.  В диалоговом окне **Создание новой задачи** введите **sqlps.exe** в поле **Открыть** и нажмите кнопку **ОК**. Это приведет к открытию окна **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell**.  
  
4.  В окне **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell** выполните следующий скрипт, чтобы включить протокол TCP/IP:  
  
```powershell
$smo = 'Microsoft.SqlServer.Management.Smo.'  
$wmi = new-object ($smo + 'Wmi.ManagedComputer')  
# Enable the TCP protocol on the default instance.  If the instance is named, replace MSSQLSERVER with the instance name in the following line.  
$uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
$Tcp = $wmi.GetSmoObject($uri)  
$Tcp.IsEnabled = $true  
$Tcp.Alter()  
$Tcp  
```  
  
##  <a name="ssnoversion-profiler"></a><a name="BKMK_Profiler"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Профилировщик  
 На удаленном компьютере запустите приложение [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] и выберите в меню «Файл» пункт «Создать трассировку», приложение откроет диалоговое окно «Соединение с сервером», где можно указать экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , размещенный на компьютере Server Core, к которому необходимо подключиться. Дополнительные сведения см. в разделе [Start SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md).  
  
 Сведения о разрешениях, необходимых для выполнения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], см. в разделе [Разрешения, необходимые для запуска приложения SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
 Дополнительные сведения о приложении [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]см. в разделе [Приложение SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md).  
  
##  <a name="ssnoversion-auditing"></a><a name="BKMK_Auditing"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Аудит  
 Для определения аудита можно дистанционно использовать среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)] . После создания и включения аудита он начнет вести записи в целевое назначение. Дополнительные сведения о создании аудитов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и управлении ими см. в разделе [Подсистема аудита SQL Server (компонент Database Engine)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
##  <a name="command-prompt-utilities"></a><a name="BKMK_CMD"></a> Программы командной строки  
 Можно использовать следующие средства командной строки, которые позволяют объединить в скрипт операции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на компьютере Server Core. В следующей таблице содержится список программ командной строки, поставляемых вместе с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для Server Core.  
  
|**Служебная программа**|**Описание**|**Установлена в**|  
|-----------------|---------------------|----------------------|  
|[Программа bcp](../../tools/bcp-utility.md)|Используется для копирования данных между экземпляром [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и файлом данных в указанном пользователем формате.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Программа dtexec](../../integration-services/packages/dtexec-utility.md)|Используется для настройки и выполнения пакета служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Программа dtutil](../../integration-services/dtutil-utility.md)|Используется для управления пакетами служб SSIS.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[Программа osql](../../tools/osql-utility.md)|Позволяет вводить инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] , системные процедуры и файлы скрипта в командной строке.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Приложение sqlagent90](../../tools/sqlagent90-application.md)|Используется для запуска агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из командной строки.|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<*имя_экземпляра*>\MSSQL\Binn|  
|[Программа sqlcmd](../../tools/sqlcmd-utility.md)|Позволяет вводить инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] , системные процедуры и файлы скрипта в командной строке.|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Программа SQLdiag](../../tools/sqldiag-utility.md)|Используется для сбора диагностических сведений для службы поддержки пользователей [!INCLUDE[msCoName](../../includes/msconame-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Программа sqlmaint](../../tools/sqlmaint-utility.md)|Служит для выполнения планов обслуживания баз данных, созданных в предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
|[Программа sqlps](../../tools/sqlps-utility.md)|Используется для выполнения команд и скриптов PowerShell. Загружает и регистрирует командлеты и поставщика PowerShell [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[Приложение sqlservr](../../tools/sqlservr-application.md)|Служит для запуска и остановки экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] из командной строки при устранении неполадок.|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
  
##  <a name="use-troubleshooting-tools"></a><a name="BKMK_troubleshoot"></a> Использование средств устранения неполадок  
 Программа [SQLdiag](../../tools/sqldiag-utility.md) позволяет выполнять сбор журналов и файлов данных с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и других типов серверов, а также мониторинг серверов и устранение определенных неполадок на серверах. SQLdiag предназначена для исследования и упрощения сбора диагностической информации для Microsoft Customer Support Services.  
  
 Служебную программу можно запустить в командной строке администратора в Server Core, используя синтаксис, описанный в статье [Программа SQLdiag](../../tools/sqldiag-utility.md).  
  
## <a name="see-also"></a>См. также:  
 [Установка SQL Server в Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)   
 [Инструкции по установке](/previous-versions/sql/)  
  

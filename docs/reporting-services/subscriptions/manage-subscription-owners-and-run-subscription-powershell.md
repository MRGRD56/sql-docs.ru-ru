---
title: Управление владельцами подписки и запуск подписки — PowerShell | Документы Майкрософт
description: Сведения о том, как программно передать владение подпиской Reporting Services от одного пользователя другому.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: subscriptions
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 01/16/2020
ms.openlocfilehash: b0174f0b7705c9a7c7c678782a4b17fb4a1a74af
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91985920"
---
# <a name="manage-subscription-owners-and-run-subscription---powershell"></a>Управление владельцами подписки и запуск подписки — PowerShell

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

Приступая к работе с [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , вы можете программно передать владение подпиской [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] от одного пользователя другому. В этом разделе содержится несколько скриптов Windows PowerShell, которые можно использовать для смены владельца подписки или простого перечисления владельцев. В каждом примере содержится образец синтаксиса для собственного режима и режима SharePoint. После смены владельца подписки подписка будет выполняться в контексте безопасности нового владельца, а в отчете в поле «User!UserID» будет отображаться значение нового владельца. Дополнительные сведения об объектной модели вызовов образцов см. в разделе <xref:ReportService2010.ReportingService2010.ChangeSubscriptionOwner%2A>  

![Содержимое, связанное с PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Содержимое, связанное с PowerShell")

##  <a name="in-this-topic"></a><a name="bkmk_top"></a> В этом разделе:
  
- [Использование скриптов](#bkmk_how_to)  
  
- [Скрипт. Вывод списка владельцев всех подписок](#bkmk_list_ownership_all)  
  
- [Скрипт. Вывод списка подписок, принадлежащих конкретному пользователю](#bkmk_list_all_one_user)  
  
- [Скрипт. Смена владельца всех подписок, принадлежащих конкретному пользователю](#bkmk_change_all)  
  
- [Скрипт. Вывод списка всех подписок, связанных с конкретным отчетом](#bkmk_list_for_1_report)  
  
- [Скрипт. Смена владельца конкретной подписки](#bkmk_change_all_1_subscription)  
  
- [Скрипт. Запуск (инициация) одной подписки](#bkmk_run_1_subscription)  
  
## <a name="how-to-use-the-scripts"></a><a name="bkmk_how_to"></a> Использование скриптов
  
### <a name="permissions"></a>Разрешения

В этом разделе приводится сводка по уровням разрешений, необходимым для использования каждого метода в собственном режиме и режиме SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. В скриптах, рассматриваемых в этом разделе, используются следующие методы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
- [Метод ReportingService2010.ListSubscriptions](/dotnet/api/reportservice2010.reportingservice2010.listsubscriptions)  
  
- [Метод ReportingService2010.ChangeSubscriptionOwner](/dotnet/api/reportservice2010.reportingservice2010.changesubscriptionowner)  
  
- [ReportingService2010.ListChildren](/dotnet/api/reportservice2010.reportingservice2010.listchildren)  
  
- Метод [ReportingService2010.FireEvent](/dotnet/api/reportservice2010.reportingservice2010.fireevent) используется только в последнем скрипте для запуска конкретной подписки. Если использовать этот скрипт не планируется, можно проигнорировать требования разрешения для метода FireEvent.  
  
**Собственный режим.**
  
- Вывод списка подписок: [перечисление ReadSubscription](/dotnet/api/microsoft.reportingservices.interfaces.reportoperation) в отчете И пользователь является владельцем подписки ИЛИ ReadAnySubscription.  
  
- Изменение подписок: Пользователь должен входить в группу BUILTIN\Administrators  
  
- Вывод списка дочерних элементов: Свойства ReadProperties для элемента  
  
- Вызов события: GenerateEvents (System)  
  
 **Режим интеграции с SharePoint:**
  
- Вывод списка подписок: ManageAlerts ИЛИ [CreateAlerts](/previous-versions/office/sharepoint-server/ms412690(v=office.15)) для отчета И пользователь является владельцем подписки и подписка является подпиской по времени).  
  
- Изменение подписок: ManageWeb  
  
- Вывод списка дочерних элементов: ViewListItems  
  
- Вызов события: ManageWeb  
  
 Дополнительные сведения см. в разделе [Сравнение ролей и задач служб Reporting Services с группами и разрешениями SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md).  
  
### <a name="script-usage"></a>Использование скрипта

**Создание файлов скрипта (PS1)**
  
1. Создайте папку с именем **c:\scripts**. Если выбирается другая папка, измените имя папки, используемое в конструкциях синтаксиса командной строки в примере.  
  
2. Для каждого скрипта создайте текстовый файл и сохраните файлы в папку c:\scripts. При создании файлов PS1 используйте имена из каждого синтаксиса командной строки в примере.  
  
3. Откройте окно командной строки с правами администратора.  
  
4. С помощью приведенного в каждом примере синтаксиса командной строки запустите каждый файл сценария.  
  
**Тестовые среды**
  
 Скрипты, приведенные в данном разделе, были протестированы в PowerShell версии 3 и со следующими версиями [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
- [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
## <a name="script-list-the-ownership-of-all-subscriptions"></a><a name="bkmk_list_ownership_all"></a>Скрипт. Вывод списка владельцев всех подписок

Этот скрипт выводит список всех подписок на одной сайте. С помощью этого скрипта можно проверить подключение или проверить путь к отчету и ИД подписки для использования в других скриптах. Этот скрипт упрощает аудит существующих подписок и их владельцев.  
  
 **Синтаксис собственного режима:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions.ps1 "[server]/reportserver" "/"  
```  
  
 **Синтаксис режима SharePoint:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions.ps1 "[server]/_vti_bin/reportserver" "https://[server]"  
```  
  
 **Скрипт:**
  
```
# Parameters  
#    server   - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
  
Param(  
    [string]$server,  
    [string]$site  
   )  
  
$rs2010 += New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site); # use "/" for default native mode site  
  
Write-Host " "  
Write-Host "----- $server's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted, Status  
```  
  
> [!TIP]  
> Чтобы проверить URL-адреса сайтов в режиме SharePoint, воспользуйтесь командлетом SharePoint **Get-SPSite**. Дополнительные сведения см. в статье [Get-SPSite](https://msdn.microsoft.com/library/ff607950\(v=office.15\).aspx).  
  
##  <a name="script-list-all-subscriptions-owned-by-a-specific-user"></a><a name="bkmk_list_all_one_user"></a> Скрипт. Вывод списка подписок, принадлежащих конкретному пользователю

Этот скрипт перечисляет все подписки, принадлежащие конкретному пользователю. С помощью этого скрипта можно проверить подключение или проверить путь к отчету и ИД подписки для использования в других скриптах. Этот скрипт полезен в случае, если какой-либо сотрудник увольняется из организации и необходимо проверить принадлежащие ему подписки, чтобы в дальнейшем сменить владельца или удалить подписку.  
  
 **Синтаксис собственного режима:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions4User.ps1 "[Domain]\[user]" "[server]/reportserver" "/"  
```  
  
**Синтаксис режима SharePoint:**
  
```powershell
powershell c:\scripts\ListAll_SSRS_Subscriptions4User.ps1 "[Domain]\[user]"  "[server]/_vti_bin/reportserver" "https://[server]"  
```  
  
**Скрипт:**
  
```
# Parameters:  
#    currentOwner - DOMAIN\USER that owns the subscriptions you wish to change  
#    server        - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site        - use "/" for default native mode site  
Param(  
    [string]$currentOwner,  
    [string]$server,  
    [string]$site  
)  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site);  
  
Write-Host " "  
Write-Host " "  
Write-Host "----- $currentOwner's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status | where {$_.owner -eq $currentOwner}  
```  
  
## <a name="script-change-ownership-for-all-subscriptions-owned-by-a-specific-user"></a><a name="bkmk_change_all"></a> Скрипт. Смена владельца всех подписок, принадлежащих конкретному пользователю

Этот скрипт меняет владельца подписок, принадлежащих конкретному пользователю параметр нового владельца.  
  
**Синтаксис собственного режима:**  
  
```powershell
powershell c:\scripts\ChangeALL_SSRS_SubscriptionOwner.ps1 "[Domain]\current owner]" "[Domain]\[new owner]" "[server]/reportserver"  
```  
  
**Синтаксис режима SharePoint:**  
  
```powershell
powershell c:\scripts\ChangeALL_SSRS_SubscriptionOwner.ps1 "[Domain]\{current owner]" "[Domain]\[new owner]" "[server]/_vti_bin/reportserver"  
```  
  
**Скрипт:**  
  
```
# Parameters:  
#    currentOwner - DOMAIN\USER that owns the subscriptions you wish to change  
#    newOwner      - DOMAIN\USER that will own the subscriptions you wish to change  
#    server        - server and instance name (e.g. myserver/reportserver, myserver/reportserver_db2, myserver/_vti_bin/reportserver)
  
Param(  
    [string]$currentOwner,  
    [string]$newOwner,  
    [string]$server  
)  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$items = $rs2010.ListChildren("/", $true);  
  
$subscriptions = @();  
  
ForEach ($item in $items)  
{  
    if ($item.TypeName -eq "Report")  
    {  
        $curRepSubs = $rs2010.ListSubscriptions($item.Path);  
        ForEach ($curRepSub in $curRepSubs)  
        {  
            if ($curRepSub.Owner -eq $currentOwner)  
            {  
                $subscriptions += $curRepSub;  
            }  
        }  
    }  
}  
  
Write-Host " "  
Write-Host " "  
Write-Host -foregroundcolor "green" "-----  $currentOwner's Subscriptions changing ownership to $newOwner : "  
$subscriptions | select SubscriptionID, Owner, Path, Description,  Status  | format-table -AutoSize  
  
ForEach ($sub in $subscriptions)  
{  
    $rs2010.ChangeSubscriptionOwner($sub.SubscriptionID, $newOwner);  
}  
  
$subs2 = @();  
  
ForEach ($item in $items)  
{  
    if ($item.TypeName -eq "Report")  
    {  
        $subs2 += $rs2010.ListSubscriptions($item.Path);  
    }  
}  
```  
  
## <a name="script-list-all-subscriptions-associated-with-a-specific-report"></a><a name="bkmk_list_for_1_report"></a> Скрипт. Вывод списка всех подписок, связанных с конкретным отчетом  

Этот скрипт перечисляет все подписки, связанные с конкретным отчетом. В синтаксисе пути к отчету в режиме SharePoint требуется использовать полный URL-адрес. В примерах синтаксиса в имени отчета указывается только название, содержащее пробел. Поэтому имя отчета следует заключить в одинарные кавычки.  
  
**Синтаксис собственного режима:**  
  
```powershell
powershell c:\scripts\List_SSRS_One_Reports_Subscriptions.ps1 "[server]/reportserver" "'/reports/title only'" "/"  
```  
  
**Синтаксис режима SharePoint:**  
  
```powershell
powershell c:\scripts\List_SSRS_One_Reports_Subscriptions.ps1 "[server]/_vti_bin/reportserver"  "'https://[server]/shared documents/title only.rdl'" "https://[server]"  
```  
  
**Скрипт:**  
  
```
# Parameters:  
#    server      - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    reportpath  - path to report in the report server, including report name e.g. /reports/test report >> pass in  "'/reports/title only'"  
#    site        - use "/" for default native mode site  
Param  
(  
      [string]$server,  
      [string]$reportpath,  
      [string]$site  
)  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
$subscriptions += $rs2010.ListSubscriptions($site);  
  
Write-Host " "  
Write-Host " "  
Write-Host "----- $reportpath 's Subscriptions: "  
$subscriptions | select Path, report, Description, Owner, SubscriptionID, lastexecuted,Status | where {$_.path -eq $reportpath}  
```  
  
## <a name="script-change-ownership-of-a-specific-subscription"></a><a name="bkmk_change_all_1_subscription"></a> Скрипт. Смена владельца конкретной подписки  
 Этот сценарий меняет владельца конкретной подписки. Подписка идентифицируется по SubscriptionID, указанному в скрипте. Чтобы определить правильный SubscriptionID, можно воспользоваться одним из скриптов вывода списка подписок.  
  
 **Синтаксис собственного режима:**  
  
```powershell
powershell c:\scripts\Change_SSRS_Owner_One_Subscription.ps1 "[Domain]\[new owner]" "[server]/reportserver" "/" "ac5637a1-9982-4d89-9d69-a72a9c3b3150"  
```  
  
 **Синтаксис режима SharePoint:**  
  
```powershell
powershell c:\scripts\Change_SSRS_Owner_One_Subscription.ps1 "[Domain]\[new owner]" "[server]/_vti_bin/reportserver" "https://[server]" "9660674b-f020-453f-b1e3-d9ba37624519"  
```  
  
 **Скрипт:**  
  
```
# Parameters:  
#    newOwner       - DOMAIN\USER that will own the subscriptions you wish to change  
#    server         - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site        - use "/" for default native mode site  
#    subscriptionID - guid for the single subscription to change  
  
Param(  
    [string]$newOwner,  
    [string]$server,  
    [string]$site,  
    [string]$subscriptionid  
   )  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
  
$subscription += $rs2010.ListSubscriptions($site) | where {$_.SubscriptionID -eq $subscriptionid};  
  
Write-Host " "  
Write-Host "----- $subscriptionid's Subscription properties: "  
$subscription | select Path, report, Description, SubscriptionID, Owner, Status  
  
$rs2010.ChangeSubscriptionOwner($subscription.SubscriptionID, $newOwner)  
  
#refresh the list  
$subscription = $rs2010.ListSubscriptions($site) | where {$_.SubscriptionID -eq $subscriptionid}; # use "/" for default native mode site  
Write-Host "----- $subscriptionid's Subscription properties: "  
$subscription | select Path, report, Description, SubscriptionID, Owner, Status  
```  
  
## <a name="script-run-fire-a-single-subscription"></a><a name="bkmk_run_1_subscription"></a> Скрипт. Запуск (инициация) одной подписки  

Этот скрипт запускает определенную подписку с помощью метода FireEvent. Независимо от настроенного для подписки расписания скрипт запустит подписку немедленно. EventType сопоставляется с известным набором событий, которые определены в файле конфигурации сервера отчетов **rsreportserver.config** . Скрипт использует следующий тип событий для стандартных подписок:  
  
 `<Event>`  
  
 `<Type>TimedSubscription</Type>`  
  
 `</Event>`  
  
 Дополнительные сведения о файле конфигурации см. в разделе [RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
 Скрипт содержит логику задержки "`Start-Sleep -s 6`", поэтому после запуска события есть небольшой период времени для ввода обновленного состояния в метод ListSubscription.  
  
 **Синтаксис собственного режима:**  
  
```powershell
powershell c:\scripts\FireSubscription.ps1 "[server]/reportserver" $null "70366e82-2d3c-4edd-a216-b97e51e26de9"  
```  
  
 **Синтаксис режима SharePoint:**  
  
```powershell
powershell c:\scripts\FireSubscription.ps1 "[server]/_vti_bin/reportserver" "https://[server]" "c3425c72-580d-423e-805a-41cf9799fd25"  
```  
  
 **Скрипт:**  
  
```
  
# Parameters  
#    server         - server and instance name (e.g. myserver/reportserver or myserver/reportserver_db2)  
#    site           - use $null for a native mode server  
#    subscriptionid - subscription guid  
  
Param(  
  [string]$server,  
  [string]$site,  
  [string]$subscriptionid  
  )  
  
$rs2010 = New-WebServiceProxy -Uri "https://$server/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential ;  
#event type is case sensative to what is in the rsreportserver.config  
$rs2010.FireEvent("TimedSubscription",$subscriptionid,$site)  
  
Write-Host " "  
Write-Host "----- Subscription ($subscriptionid) status: "  
#get list of subscriptions and filter to the specific ID to see the Status and LastExecuted  
Start-Sleep -s 6 # slight delay in processing so ListSubscription returns the updated Status and LastExecuted  
$subscriptions = $rs2010.ListSubscriptions($site);   
$subscriptions | select Status, Path, report, Description, Owner, SubscriptionID, EventType, lastexecuted | where {$_.SubscriptionID -eq $subscriptionid}  
  
```  

## <a name="see-also"></a>См. также раздел  

- [Метод ReportingService2010.ListSubscriptions](/dotnet/api/reportservice2010.reportingservice2010.listsubscriptions)  

- [Метод ReportingService2010.ChangeSubscriptionOwner](/dotnet/api/reportservice2010.reportingservice2010.changesubscriptionowner)   

- [ReportingService2010.ListChildren](/dotnet/api/reportservice2010.reportingservice2010.listchildren)  

- [ReportingService2010.FireEvent](/dotnet/api/reportservice2010.reportingservice2010.fireevent)
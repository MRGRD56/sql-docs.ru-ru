---
title: Установка или удаление надстройки служб Reporting Services для SharePoint | Документы Майкрософт
description: Сведения о том, как запустить пакет установки надстройки Microsoft SQL Server Reporting Services для продуктов SharePoint (rsSharePoint.msi) на серверах SharePoint, чтобы предоставить функции Reporting Services для развертывания SharePoint.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
ms.assetid: c2804a9a-08ea-4f4a-805d-a2c19c68733d
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: b4c8232909187894df11b7b1198fdb3416fbd1e4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97425489"
---
# <a name="install-or-uninstall-the-reporting-services-add-in-for-sharepoint-ssrs"></a>Установка или удаление надстройки служб Reporting Services для SharePoint (SSRS)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  Запустите пакет установки надстройки [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] для продуктов SharePoint (rsSharePoint.msi) на серверах SharePoint, чтобы использовать функции [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в развертывании SharePoint. К этим компонентам относится Power View, веб-часть средства просмотра отчетов, конечная точка прокси для URL-адреса, типы содержимого [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и страницы приложений, которые позволяют создавать, просматривать и администрировать отчеты, источники данных и другое содержимое сервера отчетов на сайте SharePoint. Для сервера отчетов, работающего в режиме интеграции с SharePoint, надстройка служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] для продуктов SharePoint является обязательным компонентом. Надстройку можно установить с помощью мастера установки SQL Server 2016 или скачать файл rsSharePoint.msi из пакета дополнительных компонентов SQL Server 2016. Список версий надстройки и страницы загрузки приведены в разделе [Где найти надстройку службы Reporting Services для продуктов SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Предварительные требования  
 Установка надстройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] является одним из обязательных шагов интеграции сервера отчетов с экземпляром продукта SharePoint. Дополнительные сведения об установке и настройке [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] см. в руководстве по [установке сервера отчетов в режиме интеграции с SharePoint](install-the-first-report-server-in-sharepoint-mode.md).  
  
-   При интеграции служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] с фермой SharePoint, содержащей несколько приложений клиентского веб-интерфейса, установите надстройку на каждый компьютер фермы с сервером клиентского веб-интерфейса. Это необходимо сделать только для веб-серверов, обслуживающих клиентские запросы, которые будут производить доступ к содержимому сервера отчетов.  
  
-   Чтобы установить надстройку служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , необходимо быть администратором компьютера. Например, чтобы запустить файл rsSharePoint.msi из командной строки, следует открыть командную строку с правами администратора с помощью команды **Запуск от имени администратора** .  
  
-   Чтобы установить надстройку [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , требуется членство в группе администраторов фермы SharePoint.  
  
-   Чтобы активировать функции интеграции служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , необходимо быть администратором семейства веб-сайтов.   
  
##  <a name="what-does-the-add-in-install"></a><a name="bkmk_whatinstalled"></a> Что устанавливает эта надстройка?  
 Процесс установки надстройки состоит из двух фаз, при этом обе они выполняются автоматически при стандартной установке.  
  
-   Сначала файлы устанавливаются в нужные папки. Это стандартные папки для развертываний SharePoint. Один из устанавливаемых файлов называется rsCustomAction.exe.  
  
-   На втором этапе установки выполняется набор пользовательских действий для регистрации файлов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в SharePoint. Пользовательские действия выполняются с помощью файла rsCustomAction.exe. По окончании двух этапов установки этот EXE-файл удаляется. Можно выполнить установку **только файлов** , при которой файл rsCustomAction.exe по окончании установки не запускается и остается на диске.  
  
## <a name="the-reporting-services-installation-order"></a>Порядок установки служб Reporting Services  
 Надстройку можно установить перед установкой SharePoint либо после установки SharePoint. Надстройка соответствует стандартам развертывания до установки SharePoint и устанавливает файлы в расположения, которые используются при установке SharePoint.  
  
> [!NOTE]  
>  Преимущество установки надстройки до развертывания продукта SharePoint заключается в том, что по мере добавления новых серверов в ферму надстройка служб Reporting Services будет настраиваться и активироваться фермой SharePoint.  
  
##  <a name="overview-of-the-installation-methods"></a><a name="bkmk_3ways_to_install"></a> Общие сведения о методах установки  
 Надстройку служб SQL Server 2016 Reporting Services для продуктов SharePoint можно установить одним из двух способов.  
  
-   **Мастер установки**. (![Примечание](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "Примечание")) Эту надстройку в SQL Server 2016 можно установить с помощью мастера установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Выберите **надстройку служб Reporting Services для продуктов SharePoint** на странице мастера **Выбор компонентов** .  
  
-   **rsSharepoint.msi**. Эту надстройку можно установить напрямую с установочного носителя или из скачанного отдельно файла. Файл rsSharepoint.msi поддерживает как графический пользовательский интерфейс, так и установку из командной строки. Файл MSI необходимо запустить с правами администратора, сначала открыв окно командной строки с повышенными разрешениями, затем запустить файл rsSharepoint.msi из командной строки. Дополнительные сведения о скачивании надстройки см. в разделе [Где найти надстройку службы Reporting Services для продуктов SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
    > [!NOTE]  
    >  При использовании параметра **/q** для автоматической установки из командной строки условия лицензионного соглашения не отображаются. Независимо от метода установки, использование программного обеспечения регулируется лицензионным соглашением и пользователь несет ответственность за его соблюдение.  
  
##  <a name="install-the-add-in-using-the-installation-file-rssharepointmsi"></a><a name="bkmk_install_rssharepoint"></a> Установка надстройки с помощью файла rsSharePoint.msi  
 В этом разделе рассматривается установка напрямую с помощью файла rssharepoint.msi, либо с помощью мастера установки, либо из командной строки. Если надстройка была установлена с помощью мастера установки SQL Server, выполнять эти действия не нужно.  
  
 С помощью следующей команды можно вывести полный список параметров командной строки:  
  
```  
Rssharepoint.msi /?  
```  
  
1.  Скачайте программу установки (**rsSharepoint.msi**) для надстройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Дополнительные сведения о скачивании надстройки см. в разделе [Где найти надстройку службы Reporting Services для продуктов SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md).  
  
2.  От имени администратора запустите файл **rsSharepoint.msi** , чтобы открыть мастер установки. Мастер отобразит страницу приветствия, условия лицензионного соглашения на использование программного обеспечения и регистрационные сведения. Программа установки создаст папки по следующему пути и скопирует туда файлы:  
  
     `%program files%\common files\Microsoft Shared\Web Server Extensions\15\` (SharePoint 2013)
  
     или диспетчер конфигурации служб  
  
     `%program files%\common files\Microsoft Shared\Web Server Extensions\16\` (SharePoint 2016)  
  
3.  Настройте параметры сервера отчетов и включите нужные компоненты в центре администрирования SharePoint. . Дополнительные сведения об установке и настройке служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], работающих в режиме интеграции с SharePoint, см. в разделе [Установка первого сервера отчетов в режиме интеграции с SharePoint](install-the-first-report-server-in-sharepoint-mode.md).  
  
###  <a name="files-only-installation"></a><a name="bkmk_files_only_installation"></a> Установка только файлов  
 Чтобы установить файлы и пропустить этап пользовательских действий, запустите файл rssharepoint.msi из командной строки с параметром SKIPCA.  
  
1.  Откройте командную строку **с разрешениями администратора**.  
  
2.  Выполните следующую команду:  
  
    ```  
    Msiexec.exe /i rsSharePoint.msi SKIPCA=1  
    ```  
  
 Откроется пользовательский интерфейс, и установка пройдет в обычном режиме, файл **rsCustomAction.exe** будет установлен. Но EXE-файл не будет запущен в конце установки и **rsCustomAction.exe** останется на компьютере по завершении установки.  
  
### <a name="use-a-two-step-installation-to-troubleshoot-installation-issues"></a>Используйте установку в два этапа для устранения неполадок при установке  
 Если во время установки возникли ошибки, программу установки можно запустить из командной строки в два этапа.  
  
1.  Откройте командную строку **с разрешениями администратора** и запустите установку в режиме «только файлы», как описано в предыдущем разделе.  
  
2.  Запустите исполняемый файл пользовательских действий.  
  
    1.  Перейдите к папке с файлом **rsCustomAction.exe**. Этот файл копируется на компьютер программой установки надстройки, которая устанавливает только файлы. **rsCustomAction.exe** находится в каталоге **%Temp%** . Для перехода к файлу введите в командной строке следующую команду:  
  
         **CD %temp%** .  
  
         Файл должен быть расположен в папке: **\Users\\<ваше имя\>\AppData\Local\Temp**.  
  
    2.  Введите следующую команду. Выполнение этого шага настройки требует нескольких минут. В течение этого времени будет перезапущена служба W3SVC. Пока программа копирует файлы, регистрирует компоненты и запускает мастер настройки продуктов SharePoint, на экране будут появляться различные сообщения о состоянии.  
  
        ```  
        rsCustomAction.exe /i  
        ```  
  
    3.  Время, необходимое для применения изменений, варьируется и зависит от используемой среды сервера. Можно также запустить **iisreset** , чтобы принудительно выполнить более быстрое обновление.  
  
### <a name="quiet-installation-for-scripting"></a>Автоматическая установка  
 Вы можете использовать параметры **/q** или **/quiet** для автоматической установки, в ходе которой не будут выводиться никакие диалоговые окна или сообщения. Автоматическая установка удобна, если установку надстройки нужно выполнить из скрипта.  
  
> [!NOTE]  
>  При использовании параметра **/q** для автоматической установки из командной строки условия лицензионного соглашения не отображаются. Независимо от метода установки, использование программного обеспечения регулируется лицензионным соглашением и пользователь несет ответственность за его соблюдение.  
  
 Выполнение автоматической установки  
  
1.  Откройте командную строку **с разрешениями администратора**.  
  
2.  Выполните следующую команду:  
  
    ```  
    Msiexec.exe /i rsSharePoint.msi /q  
    ```  
  
##  <a name="how-to-remove-the-reporting-services-add-in"></a><a name="bkmk_remove_addin"></a> Удаление надстройки служб Reporting Services  
 Удалить надстройку служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] для продуктов SharePoint можно с помощью панели управления [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows или из командной строки.  
  
1.  При использовании панели управления выполняется полное удаление файлов с данного компьютера **И** удаление объекта и компонентов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] из фермы SharePoint. Когда объект и компоненты служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] будут удалены, вы больше не сможете просматривать и обновлять отчеты.  
  
2.  Метод удаления надстройки с помощью командной строки позволяет с помощью параметра LocalOnly удалять только файлы надстройки с локального компьютера, при этом объект и компоненты [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в ферме остаются без изменений.  
  
 При удалении надстройки будут удалены функции интеграции сервера, используемые для обработки отчетов на сервере отчетов. Также будут удалены страницы служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] из центра администрирования SharePoint и другие пользовательские страницы служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . При необходимости также удалите все отчеты и другие элементы сервера отчетов, которые больше не используются на затронутых сайтах SharePoint. Их запуск будет невозможен после удаления надстройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Чтобы удалить надстройку служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , необходим работающий экземпляр SharePoint. Если экземпляр SharePoint был удален ранее, чтобы удалить надстройку служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , его придется установить повторно.  
  
 Удаление надстройки выполняется одинаково для изолированных серверов и для ферм серверов. С помощью программы установки удаляются программные файлы и настройки конфигурации, добавленные во время установки.  
  
 При удалении надстройки следующие элементы не удаляются:  
  
-   Имена входа, созданные для учетной записи службы Report Server для доступа к конфигурации и базам данных содержимого SharePoint. Необходимо удалить их для учетной записи сервера отчетов на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)], где расположены базы данных SharePoint.  
  
-   Разрешения или группы, созданные для пользователей отчетов. Если были созданы пользовательские уровни разрешений или группы SharePoint для предоставления доступа к функциям сервера отчетов, можно отменить любые разрешения, необходимость в которых отпала.  
  
-   Файлы данных, загруженные в библиотеку SharePoint, включая файл определения отчета (RDL), общего источника данных (RSDS) и элементы опубликованных отчетов (RSC). Они не удаляются, но их выполнение становится невозможным. Удаление файлов необходимо произвести вручную.  
  
-   Программой установки не будет удалена база данных сервера отчетов или изменен экземпляр сервера отчетов, используемый для операций в режиме интеграции.  
  
### <a name="to-uninstall-from-windows-control-panel"></a>Удаление с панели управления Windows  
 Запуск мастера с панели управления [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows и удаление надстройки  
  
1.  На панели управления, в разделе **Программы** выберите **Удалить программу**.  
  
2.  Выберите **Надстройка служб Microsoft SQL Server RS для SharePoint**. Также вы можете открыть мастер удаления, запустив файл **rssharepoint.msi** из командной строки без ключей.  
  
3.  Щелкните **Удалить**.  
  
### <a name="uninstall-from-the-command-line"></a>Удаление из командной строки  
 Удаление надстройки из командной строки  
  
1.  Откройте командную строку **с разрешениями администратора**.  
  
2.  Выполните следующую команду:  
  
    ```  
    msiexec.exe /uninstall rsSharePoint.msi  
    ```  
  
3.  Появится сообщение с запросом на подтверждение. Нажмите кнопку **Да**.  
  
### <a name="uninstall-the-add-in-from-the-local-server-only"></a>Удаление надстройки только с локального сервера  
 Рассмотренные выше методы удаления надстройки также удаляют объект и компоненты служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] из фермы. Если ферма состоит из нескольких серверов и требуется удалить надстройку только с локального компьютера, сохранив при этом ферму SharePoint а рабочем состоянии, выполните следующие шаги.  
  
1.  Откройте командную строку **с разрешениями администратора**.  
  
2.  Выполните следующую команду:  
  
    ```  
    Msiexec.exe /uninstall rsSharePoint.msi LocalOnly=1  
    ```  
  
 При этом будет отменена регистрация компонентов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в SharePoint и удалены файлы, но только на локальном компьютере.  
  
 Если необходимо отменить регистрацию компонентов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в SharePoint, но сохранить файлы на диске для последующего использования, выполните следующие шаги.  
  
1.  Откройте командную строку **с разрешениями администратора**.  
  
2.  Выполните следующую команду:  
  
    ```  
    rsCustomAction.exe /p  
    ```  
  
 При этом предполагается, что установка MSI-файла была проведена с параметром SkipCA=1 и доступен файл rscusstomaction.exe. Дополнительные сведения см. в разделе с описанием установки только файлов.  
  
##  <a name="how-to-repair-rssharepointmsi-from-the-command-line"></a><a name="bkmk_repair"></a> Восстановление rssharepoint.msi из командной строки  
 Чтобы восстановить или удалить надстройку служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] с использованием командной строки, выполните следующие действия.  
  
1.  Откройте командную строку **с разрешениями администратора**.  
  
2.  Выполните следующую команду:  
  
    ```  
    msiexec.exe /f rssharepoint.msi  
    ```  
  
##  <a name="setup-log-files"></a><a name="bkmk_logfiles"></a> Файлы журналов установки  
 При запуске программы установки она записывает сведения в файл журнала в папке **%temp%** пользователя, который устанавливает надстройку служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Например, **c:\Users\\<имя_пользователя\>\AppData\Local\Temp**. Имя файла имеет вид **RS_SP_\<number>.log**, например **RS_SP_0.log**. Каждая запись об ошибке в журнале начинается со строки «SSRSCustomActionError».  
  
> [!NOTE]  
>  AppData — это скрытая папка в операционной системе Windows. Для отображения скрытых файлов и папок, возможно, потребуется изменить параметры папки в проводнике Windows.  
  
#### <a name="view-a-log-file-with-windows-notepad"></a>Просмотр файла журнала в Блокноте Windows  
  
1.  Следующие команды меняют путь в командной строке, выводят список RS-файлов журналов и открывают один из файлов в Блокноте Windows:  
  
    ```  
    cd %temp%  
    ```  
  
    ```  
    Dir rs_sp*.log  
    ```  
  
    ```  
    notepad rs_sp_3.log  
    ```  
  
#### <a name="view-a-log-file-with-powershell"></a>Просмотр файла журнала с помощью PowerShell  
  
1.  Введите следующую команду в консоли управления SharePoint, чтобы получить из файла, содержащего подстроку "ssrscustomactionerror", отфильтрованный список строк.  
  
    ```  
    Get-content -path C:\Users\<UserName\AppData\Local\Temp\rs_sp_0.log | select-string "ssrscustomactionerror"  
    ```  
  
2.  Вывод имеет следующий вид:  
  
     `2011-05-23 12:40:12: SSRSCustomActionError: SharePoint is installed, but not configured`.  
  
##  <a name="upgrade"></a><a name="bkmk_upgrade"></a> Обновление  
 Если надстройка служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] уже установлена, ее можно обновить до текущей версии. Программа установки надстройки обнаружит существующую версию и предложит подтвердить обновление. Сообщение имеет следующий вид:  
  
 **В системе обнаружена более ранняя версия этого продукта. Обновить установленную версию?**  
  
 При получении подтверждения старая версия надстройки будет удалена, а новая установлена.  
  
 Обратите внимание, что надстройка служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] не привязана к экземпляру. Можно установить только один экземпляр надстройки на компьютер. Более ранние версии нельзя запускать параллельно с текущей версией.  
  
##  <a name="rscustomactionexe"></a><a name="bkmk_rscustomaction"></a> RsCustomAction.exe  
 В следующей таблице описываются параметры командной строки для программы rscustomaction.exe.  
  
|Параметр|Описание|  
|------------|-----------------|  
|i|Устанавливает пользовательские действия. Регистрирует компоненты служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в SharePoint. Перезапускает службу W3SVCservice.|  
|r|Восстановление|  
|u|Удаление. Отменяет регистрацию компонентов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] во всей ферме SharePoint, но оставляет файлы на диске. Перезапускает службу W3SVCservice.|  
|p|Локальное удаление Отменяет регистрацию компонентов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] только на локальном компьютере. Файлы останутся на диске. Перезапускает службу W3SVCservice.|  
|t|Только SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 2005. Переключение проверяет, имеет ли сервер отчетов рабочее соединение с базой данных сервера отчетов.|  
  
## <a name="configuring-reporting-services"></a>Настройка служб Reporting Services  
 После установки надстройки на все нужные компьютеры настройте сервер отчетов с помощью центра администрирования SharePoint. Необходимые действия зависят от порядка установки разных технологий. См. дополнительные сведения об [установке сервера отчетов в режиме интеграции с SharePoint](install-the-first-report-server-in-sharepoint-mode.md) и [сервере отчетов Reporting Services (режим SharePoint)](../../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md)  
  
## <a name="see-also"></a>См. также:

[Установка первого сервера отчетов в режиме интеграции с SharePoint](install-the-first-report-server-in-sharepoint-mode.md)   
[Сервер отчетов служб Reporting Services (режим SharePoint)](../../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
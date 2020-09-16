---
description: Подготовка подписок и предупреждений для приложений служб SSRS
title: Подготовка подписок и предупреждений для приложений служб SSRS | Документы Майкрософт
ms.date: 06/03/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services Shared Service
- SharePoint Mode [Reporting Services]
- SharePoint Mode
- Reporting Services Service Application
- SSRS service application
ms.assetid: d0de3f1f-4887-47fb-bacf-46aaad74c4be
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 28f69b4aa47b45708832162b2b4b8429a847bc65
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88454536"
---
# <a name="provision-subscriptions-and-alerts-for-ssrs-service-applications"></a>Подготовка подписок и предупреждений для приложений служб SSRS
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и предупреждения данных требуют наличия агента SQL Server, а также настройку разрешений для агента SQL Server. Если появляются сообщения об ошибках, указывающие, что необходим агент SQL Server, хотя агент SQL Server уже запущен, необходимо обновить и проверить разрешения. Этот раздел относится к [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint и здесь описаны три способа обновления разрешений агента SQL Server для подписок [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Вводимые учетные данные для шагов из данного раздела должны иметь достаточные разрешения для предоставления роли RSExecRole прав на выполнение объектов из приложения службы, баз данных msdb и master.  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** SharePoint 2016 &#124; SharePoint 2013|  
  
 ![Разрешение для агента SQL Server на доступ к базам данных приложений служб](../../reporting-services/install-windows/media/rs-provisionsqlagent.gif "Разрешение для агента SQL Server на доступ к базам данных приложений служб")  
  
||Описание|  
|------|-----------------|  
|**1**|Экземпляр компонента SQL Server Database Engine, на котором размещаются базы данных приложения службы Reporting Services.|  
|**2**|Экземпляр агента SQL Server для экземпляра компонента SQL Server Database Engine.|  
|**3**|Базы данных приложения службы Reporting Services. Имена создаются на основе сведений, которые использовались при создании приложения службы. Ниже приведены примеры имен баз данных.<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0_Alerting<br /><br /> ReportingService_2fbae157295d49df86d0b85760c704b0TempDB|  
|**4**|Базы данных master и MSDB экземпляра компонента SQL Server Database Engine.|  
  
 Используйте один из следующих трех методов, чтобы обновить разрешения:  
  
1.  На странице **Подготовка подписок и предупреждений** введите учетные данные и нажмите кнопку **ОК**.  
  
2.  На странице «Подготовка подписок и предупреждений» нажмите кнопку **Загрузить скрипт** , чтобы загрузить скрипт Transact-SQL, с помощью которого можно настроить разрешения.  
  
3.  Запустите командлет PowerShell, чтобы построить скрипт SQL, который можно использовать в настройке разрешений.  
  
### <a name="to-update-permissions-using-the-provision-page"></a>Обновление разрешений с помощью страницы подготовки  
  
1.  В центре администрирования SharePoint в разделе **Управление приложениями** выберите **Управление приложениями службы**.  
  
2.  Найдите нужное приложение службы в списке и щелкните имя приложения либо щелкните столбец **Тип** , чтобы выбрать приложение-службу, а затем нажмите кнопку **Управление** на ленте SharePoint.  
  
3.  На странице **Управление приложением служб Reporting Services** щелкните **Подготовка подписок и предупреждений**.  
  
4.  Если администратор SharePoint имеет достаточно прав для базы данных Master и баз данных приложений служб, введите эти учетные данные.  
  
5.  Нажмите кнопку **ОК** .  
  
##  <a name="to-download-the-transact-sql-script"></a><a name="bkmk_download"></a> Загрузка скрипта Transact-SQL  
  
1.  В центре администрирования SharePoint в разделе **Управление приложениями** выберите **Управление приложениями службы**.  
  
2.  Найдите нужное приложение службы в списке и щелкните имя приложения либо щелкните столбец **Тип** , чтобы выбрать приложение-службу, а затем нажмите кнопку **Управление** на ленте SharePoint.  
  
3.  На странице **Управление приложением служб Reporting Services** щелкните **Подготовка подписок и предупреждений**.  
  
4.  В области **Просмотр состояния** убедитесь, что агент SQL Server запущен.  
  
5.  Нажмите **Загрузить скрипт** , чтобы загрузить скрипт Transact SQL, путем запуска которого в среде SQL Server Management Studio можно предоставлять разрешения. Имя созданного файла скрипта будет содержать имя приложения служб Reporting Services, например **[имя приложения службы]-GrantRights.sql**.  
  
### <a name="to-generate-the-transact-sql-statement-with-powershell"></a>Создание инструкции Transact-SQL с помощью PowerShell  
  
1.  Создать скрипт Transact-SQL можно с помощью командлета Windows PowerShell в консоли управления SharePoint 2016 или консоль управления SharePoint 2013.  
  
2.  В меню **Пуск** выберите пункт **Все программы**.  
  
3.  Разверните **Продукты Microsoft SharePoint 2016** и выберите **Консоль управления SharePoint 2016**.
  
4.  Обновите следующий командлет PowerShell, заменив имя базы данных сервера отчетов, учетную запись пула приложений и путь к инструкции.  
  
     **Синтаксис командлета:** `Get-SPRSDatabaseRightsScript -DatabaseName <ReportingServices database name> -UserName <app pool account> -IsWindowsUser | Out-File <path of statement>`  
  
     **Образец командлета:** `Get-SPRSDatabaseRightsScript -DatabaseName ReportingService_46fd00359f894b828907b254e3f6257c -UserName "NT AUTHORITY\NETWORK SERVICE" -IsWindowsUser | Out-File c:\SQLServerAgentrights.sql`  
  
## <a name="using-the-transact-sql-script"></a>Использование скрипта Transact-SQL  
 Следующие процедуры можно использовать для скриптов, загруженных со страницы подготовки либо созданных с помощью PowerShell.  
  
#### <a name="to-load-the-transact-sql-script-in-sql-server-management-studio"></a>Загрузка скрипта Transact-SQL в среду SQL Server Management Studio  
  
1.  Чтобы открыть среду SQL Server Management Studio, выберите в меню **Пуск** пункт [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)] , а затем среду **SQL Server Management Studio**.  
  
2.  В диалоговом окне **Соединение с сервером** установите следующие параметры.  
  
    -   В раскрывающемся списке **Тип сервера** выберите **Компонент Database Engine**.  
  
    -   В поле **Имя сервера**введите имя экземпляра SQL Server, для которого настраивается агент SQL Server.  
  
    -   Выберите режим проверки подлинности.  
  
    -   При соединении с помощью проверки подлинности SQL Server введите имя входа и пароль.  
  
3.  Нажмите кнопку **Подключить**.  
  
#### <a name="to-run-the-transact-sql-statement"></a>Запуск инструкции Transact-SQL  
  
1.  На панели инструментов среды SQL Server Management Studio выберите **Создать запрос**.  
  
2.  В меню **Файл** выберите **Открыть**, затем **Файл**.  
  
3.  Перейдите в папку, где была сохранена инструкция Transact-SQL, созданная в консоли управления SharePoint 2016, или консоль управления SharePoint 2013.  
  
4.  Щелкните файл, затем нажмите кнопку **Открыть**.  
  
     Инструкция будет добавлена в окно запросов.  
  
5.  Нажмите кнопку **Выполнить**.  
  
  

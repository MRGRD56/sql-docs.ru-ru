---
description: Перенос установки служб Reporting Services (режим интеграции с SharePoint)
title: Перенос установки служб Reporting Services (режим интеграции с SharePoint) | Документы Майкрософт
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 61290949-690a-4e19-b078-57c99b6b30fa
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: 894dfe8b6b3c4832a687cd10b2ed21644d1f7227
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439349"
---
# <a name="migrate-a-reporting-services-installation-sharepoint-mode"></a>Перенос установки служб Reporting Services (режим интеграции с SharePoint)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE [ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  В этом разделе дается обзор шагов, необходимых для миграции развертывания служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint из одной среды SharePoint в другую. Конкретные шаги могут различаться в зависимости от версии, с которой выполняется перенос. Дополнительные сведения о сценариях обновления и переноса в режиме интеграции с SharePoint см. в разделе [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md). Сведения о том, как только скопировать элементы отчета с одного сервера на другой, см. в разделе [Образец скрипта программы rs.exe служб Reporting Services для копирования содержимого между серверами отчетов](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md).  
  
 Сведения о миграции развертывания служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , работающих в собственном режиме, см. в статье [Перенос установки служб Reporting Services (собственный режим)](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
 Обычная причина выполнения миграции — обновление развертывания SharePoint 2010 до SharePoint 2013/2016. SharePoint 2013/2016 не поддерживает обновление на месте с SharePoint 2010, поэтому необходимо выполнить процедуру **обновления с переподключением баз данных** или перенести только содержимое.  
  
 Дополнительные сведения об обновлении SharePoint 2013/2016 см. в следующих материалах:  

-   [Общие сведения о процессе обновления до SharePoint 2016](https://technet.microsoft.com/library/cc262483\(v=office.16\)).

-   [Общие сведения о процессе обновления до SharePoint 2013](https://go.microsoft.com/fwlink/p/?LinkId=256688).
  
-   [Подготовка с очисткой перед обновлением до SharePoint 2013](https://go.microsoft.com/fwlink/p/?LinkId=256689).  
  
-   [Обновление баз данных с SharePoint 2013 до SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\)).

-   [Обновление баз данных с SharePoint 2010 до SharePoint 2013](https://go.microsoft.com/fwlink/p/?LinkId=256690).
  
-   [Перемещение баз данных содержимого в SharePoint 2016](https://technet.microsoft.com/library/cc262792\(v=office.16\).aspx).

-   [Перемещение баз данных содержимого в SharePoint 2013](https://technet.microsoft.com/library/cc262792.aspx).
  
##  <a name="migrate-from-reporting-services-sharepoint-mode-versions-prior-to-sql-server-2012"></a><a name="bkmk_prior_versions"></a> Перенос из версий служб Reporting Services в режиме интеграции SharePoint, предшествующих SQL Server 2012  
 Архитектура служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint изменилась в [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], включая схему базы данных приложения службы. Если нужно перенести систему в [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] в режиме интеграции с SharePoint с версий, предшествующих [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], сначала создайте новую среду SharePoint, установив SharePoint и службы SQL Server 2016 Reporting Services в режиме интеграции с SharePoint. Дополнительные сведения см. в разделе [Установка служб Reporting Services в режиме SharePoint](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md).  
  
 Как только новая среда SharePoint будет запущена, можно выбрать между переносом только содержимого и полным переносом на уровне баз данных, включая базы данных содержимого.  
  
###  <a name="content-only-migration"></a><a name="bkmk_content_only_migration"></a> Перенос только содержимого  
 **Перенос только содержимого служб Reporting Services** . Если требуется скопировать содержимое [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в новую ферму, то необходимо использовать такие инструменты, как **rs.exe** , чтобы скопировать содержимое в новый установленный экземпляр SharePoint. Дополнительные сведения о переносах только содержимого см. в следующих материалах:  
  
-   **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Скрипты RSS.** Скрипты могут переносить содержимое и ресурсы между серверами отчетов в собственном режиме и в режиме интеграции с SharePoint. Дополнительные сведения см. в статьях [Образец скрипта программы rs.exe служб Reporting Services для копирования содержимого между серверами отчетов](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md) и [Скрипт программы RS.exe служб Reporting Services, который переносит содержимое с одного сервера отчетов на другой](https://azuresql.codeplex.com/releases/view/115207).  
  
-   **Средство миграции служб Reporting Services** . Средство миграции может скопировать элементы отчета с сервера в основном режиме на сервер в режиме интеграции с SharePoint. См. дополнительные сведения о [средстве миграции служб Reporting Services](https://www.microsoft.com/download/details.aspx?id=29560) (https://www.microsoft.com/download/details.aspx?id=29560).  
  
###  <a name="full-migration"></a><a name="bkmk_full_migration"></a> Полный перенос  
 **Полная миграция** . При переносе в новую ферму баз данных содержимого SharePoint вместе с базами данных каталогов [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] можно использовать серию параметров архивации и восстановления, представленных в данном разделе. В некоторых случаях на этапе восстановления необходимо использовать инструменты, отличные от инструментов, которые использовались при резервном копировании. Например, диспетчер конфигурации [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] можно использовать для резервного копирования ключей шифрования из предыдущей версии [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Но для восстановления ключей шифрования в установку SQL Server 2016 Reporting Services в режиме интеграции с SharePoint необходимо использовать центр администрирования SharePoint или PowerShell.  
  
####  <a name="databases-you-will-see-in-the-completed-migration"></a><a name="bkmk_databases"></a> Базы данных, доступные для просмотра после завершения миграции  
 В следующей таблице описаны базы данных SQL Server, которые связаны с [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и доступны после успешного переноса установки SharePoint [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
|База данных|Пример имени|Примечания|  
|--------------|------------------|-|  
|База данных каталогов|ReportingService_[идентификатор GUID приложения службы] **(&#42;)**|Пользователь выполняет перенос.|  
|Временная база данных|ReportingService_[идентификатор GUID приложения службы]TempDB **(&#42;)**|Пользователь выполняет перенос.|  
|База данных предупреждений|ReportingService_[идентификатор GUID приложения служб]_Alerting|Создается во время создания приложения служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|  
  
 **(&#42;)** Представленные в таблице примеры имен соответствуют соглашению об именовании SSRS, которое используется при создании нового приложения служб SSRS. При выполнении переноса с другого сервера используются имена для каталога и временных баз данных из исходной установки.  
  
####  <a name="backup-operations"></a><a name="bkmk_backup_operations"></a> Операции резервного копирования  
 В данном разделе описываются типы информации, которые необходимы для переноса, и инструменты или процессы, которые используются для резервного копирования.  
  
 ![Базовая диаграмма миграции служб SSRS SharePoint](../../reporting-services/install-windows/media/rs-sharepoint-migration.gif "Базовая диаграмма миграции служб SSRS SharePoint")  
  
|Элемент|Объекты|Метод|Примечания|  
|-|-------------|------------|-----------|  
|**1**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|**Rskeymgmt.exe** или диспетчер конфигурации служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . См. статью [Резервное копирование и восстановление ключей шифрования служб Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).|Указанные инструменты можно использовать для резервного копирования. Для восстановления используйте страницы управления приложением служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] или PowerShell.|  
|**2**|База данных содержимого SharePoint.||Создайте резервную копию базы данных и отсоедините базу данных.<br /><br /> Дополнительные сведения см. в разделе об обновлении присоединения баз данных в статье [Определение стратегии обновления до SharePoint 2010 (https://technet.microsoft.com/library/cc263447.aspx)](https://technet.microsoft.com/library/cc263447.aspx)).|  
|**3**|База данных SQL Server, которая используется в качестве базы данных каталогов [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Резервное копирование и восстановление баз данных SQL Server<br /><br /> или<br /><br /> Присоединение и отсоединение баз данных SQL Server||  
|**4**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Простая копия файла.|Если выполнена настройка файла, то необходимо скопировать только rsreportserver.config. Пример расположения файлов по умолчанию: C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting\\*:<br /><br /> <br /><br /> RSReportServer.config<br /><br /> Rssvrpolicy.config<br /><br /> Web.config для приложения ASP.NET сервера отчетов.<br /><br /> Machine.config для ASP.NET.|  
  
####  <a name="restore-operations"></a><a name="bkmk_restore_operations"></a> Операции восстановления  
 В данном разделе описываются типы информации, которые необходимы для переноса, и инструменты или процессы, которые используются для восстановления. Инструменты, которые используются для восстановления, могут отличаться от инструментов, используемых при резервном копировании.  
  
 Перед восстановлением необходимо установить и настроить новую ферму SharePoint и службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint. Дополнительные сведения об установке служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint см. в статье [Установка служб Reporting Services в режиме SharePoint](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md).  
  
|Элемент|Объекты|Метод|Примечания|  
|-|-------------|------------|-----------|  
|**1**|Восстановите базы данных содержимого SharePoint в новую ферму.|Метод "Обновление присоединения базы данных" SharePoint.|Основные шаги:<br /><br /> 1) Восстановите базу данных на новом сервере.<br /><br /> 2) Подключите базу данных содержимого к веб-приложению, указав URL-адрес.<br /><br /> 3) Используйте Get-SPWebapplication для вывода списка всех веб-приложений и URL-адресов.<br /><br /> <br /><br /> Дополнительные сведения см. в разделе об обновлении присоединения баз данных в статьях [Определение стратегии обновления до SharePoint 2010 (https://technet.microsoft.com/library/cc263447.aspx)](https://technet.microsoft.com/library/cc263447.aspx)) и [Присоединение баз данных и обновление до SharePoint Server 2010 (https://technet.microsoft.com/library/cc263299.aspx)](https://technet.microsoft.com/library/cc263299.aspx)).|  
|**2**|Восстановите базу данных SQL Server, которая является базой данных каталогов [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (ReportServer).|Резервное копирование и восстановление баз данных SQL.<br /><br /> **или диспетчер конфигурации служб**<br /><br /> Присоединение и отсоединение баз данных SQL Server.|При первом использовании базы данных службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] обновляют схему баз данных, которая требуется для работы со средой SQL Server 2016.|  
|**3**|Создайте новое приложение службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Создайте новое приложение службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|При создании нового приложения службы настройте его для использования скопированной базы данных сервера отчетов.<br /><br /> Дополнительные сведения об использовании центра администрирования SharePoint см. в разделе "Шаг 3. Создание приложения служб Reporting Services" статьи [Установка первого сервера отчетов в режиме интеграции с SharePoint](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md).<br /><br /> Примеры использования PowerShell см. в разделе "Создание приложения службы Reporting Services с помощью PowerShell" в статье [Служба и приложения службы Reporting Services в SharePoint](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md).|  
|**4**|Восстановите файлы конфигурации служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Простая копия файла.|Пример расположения файлов по умолчанию: C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting\.|  
|**5**|Восстановите ключи шифрования служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Восстановите файл с резервной копией ключей с помощью страницы SystemSettings приложения службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].<br /><br /> **или диспетчер конфигурации служб**<br /><br /> PowerShell.|См. раздел "Управление ключами" в статье [Управление приложением службы Reporting Services в SharePoint](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).|   
  
##  <a name="migrate-from-a-sql-server-2012-or-sql-server-2014-deployment"></a><a name="bkmk_migrate_from_ctp"></a> Миграция из развертывания SQL Server 2012 или SQL Server 2014  
 В ферме из нескольких серверов базы данных содержимого и каталога пользователей обычно размещаются на разных компьютерах. В этом случае необходимо добавить к ферме SharePoint новый сервер с установленными службами [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , а затем удалить старый сервер. Копировать базы данных не требуется.  
  
### <a name="backup-operations"></a>Операции резервного копирования  
  
1.  Создайте резервную копию ключей шифрования служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
2.  Создайте резервную копию приложения служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в центре администрирования SharePoint (или используйте PowerShell). При этом также создаются резервные копии баз данных приложения в SharePoint Дополнительные сведения см. в статье  [Резервное копирование и восстановление служебного приложения SharePoint службы Reporting Services](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md).  
  
3.  При наличии учетной записи автоматического выполнения и использовании проверки подлинности Windows запишите учетные данные, чтобы их можно было использовать в процессе восстановления.  
  
4.  Дополнительные сведения см. в разделе [Резервное копирование приложений службы в SharePoint 2013](https://technet.microsoft.com/library/ee428318.aspx).  
  
### <a name="restore-operations"></a>Операции восстановления  
  
1.  Восстановите приложение служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] с помощью центра администрирования SharePoint. Также можно использовать PowerShell.  
  
2.  Восстановите ключи шифрования служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
     См. раздел "Управление ключами" в статье [Управление приложением службы Reporting Services в SharePoint](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  
  
3.  Настройте учетную запись автоматического выполнения и учетные данные Windows для приложения служб.  
  
4.  Дополнительные сведения см. в разделе [Восстановление приложений службы в SharePoint 2013](https://technet.microsoft.com/library/ee428305.aspx).  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> Дополнительные ресурсы  
  
-   [Обновление до SharePoint 2013 (https://technet.microsoft.com/library/ee833948.aspx)](https://technet.microsoft.com/library/ee833948.aspx).  
  
-   [Общие сведения об обновлении до SharePoint 2013 (https://technet.microsoft.com/library/cc262483.aspx)](https://technet.microsoft.com/library/cc262483.aspx).  

## <a name="next-steps"></a>Дальнейшие шаги

[Обновление и перенос служб Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Перенос установки служб Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).

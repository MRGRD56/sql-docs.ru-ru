---
description: Verify a Reporting Services Installation
title: Проверка установки служб Reporting Services | Документы Майкрософт
ms.date: 06/03/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- checking report server installations
- verifying report server installations
- Report Designer [Reporting Services], verifying installations
- installing Reporting Services, verifying installations
- report servers [Reporting Services], verifying installations
- Setup [Reporting Services], verifying installations
ms.assetid: 82a51a99-66f0-4b0c-b05b-07d22387adb0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3359888ec94a9893dc018782f73827f876e95037
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472405"
---
# <a name="verify-a-reporting-services-installation"></a>Verify a Reporting Services Installation
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] можно установить в один из двух режимов — в собственном режиме или в режиме интеграции с SharePoint. Шаги, которые необходимо выполнить для проверки установки, зависят от выбранного режима сервера отчетов.  

> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.

::: moniker range="=sql-server-2016"
  
##  <a name="verify-sharepoint-mode-installation"></a><a name="bkmk_sharepointmode"></a> Проверка установки в режиме SharePoint  
  
### <a name="to-verify-the-reporting-services-service"></a>Проверка службы Reporting Services  
  
1.  В центре администрирования SharePoint выберите пункт **Управление службами на сервере** в группе **Параметры системы** .  
  
2.  Убедитесь в том, что **Служба SQL Server Reporting Services** установлена и находится в состоянии **Выполняется** .  
  
     Если вы не видите в списке службу [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , убедитесь, что служба установлена. См. дополнительные сведения об [установке сервера отчетов в режиме интеграции с SharePoint](install-the-first-report-server-in-sharepoint-mode.md).  
  
### <a name="to-verify-the-service-application"></a>Проверка приложения службы  
  
1.  Для проверки из центра администрирования наличия по крайней мере одного приложения службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] выберите пункт **Управление приложениями службы** в группе **Управление приложениями** .  
  
2.  Убедитесь в наличии приложения службы типа **Приложение службы SQL Server Reporting Services** и соответствующего прокси-сервера приложения.  
  
3.  Щелкните **рядом** с именем этого приложения службы и нажмите кнопку **Свойства** на панели инструментов SharePoint.  Если вы щелкнете имя приложения службы, будут открыты страницы управления приложением службы, а не страница свойств.  
  
4.  Убедитесь, что **Связь с веб-приложением** настроена на указание нужного веб-приложения.  
  
### <a name="to-verify-the-site-collection-feature"></a>Проверка компонента семейства веб-сайтов  
  
1.  Выберите в параметрах веб-сайта пункт **Компоненты семейства веб-сайтов** в группе **Администрирование семейства веб-сайтов** .  
  
2.  Убедитесь, что активен **Компонент интеграции сервера отчетов** .  
  
### <a name="to-verify-reporting-server-content-types"></a>Проверка типов содержимого сервера отчетов  
  
1.  Инструкции по добавлению типов содержимого сервера отчетов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] см. в статье [Добавление типов содержимого служб Reporting Services в библиотеку SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
### <a name="to-verify-you-can-launch-report-builder"></a>Проверка возможности запуска построителя отчетов  
  
1.  Находясь в библиотеке документов, выберите **Документы** на ленте SharePoint.  
  
2.  Нажмите кнопку **Создать документ** и выберите пункт **Отчет в построителе отчетов**. Если этот пункт не виден, изучите предыдущую процедуру по добавлению в библиотеку типов содержимого сервера отчетов.  
  
### <a name="create-a-basic-report"></a>Создание простого отчета  
  
1.  В библиотеке документов SharePoint создайте базовый отчет служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , который содержит только одно текстовое поле, например заголовок. Отчет не содержит никаких источников данных и наборов данных. Цель — проверить, что можно будет открыть построитель отчетов и просмотреть базовый отчет.  
  
2.  Сохраните отчет в библиотеке документов и запустите отчет из библиотеки. Дополнительные сведения о создании отчетов с помощью построителя отчетов см. в руководстве по [запуску построителя отчетов](../report-builder/start-report-builder.md).  
  
### <a name="reporting-services-samples"></a>Образцы служб Reporting Services  
  
1.  Выполните один из учебников по службам Reporting Services Дополнительные сведения см. в статье [Учебники по службам Reporting Services (SSRS)](../../reporting-services/reporting-services-tutorials-ssrs.md).  
  
2.  Загрузите образец базы данных Adventure Works и образцы отчетов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] с сайта GitHub. Дополнительные сведения см. в разделе [Образцы баз данных AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  

::: moniker-end
  
##  <a name="verify-a-native-mode-installation"></a><a name="bkmk_nativemode"></a> Проверка установки в собственном режиме  
 Если сервер отчетов устанавливается в собственном режиме в конфигурации по умолчанию, то программа установки проводит установку и развертывание сервера. Можно проверить, развернула ли программа установки сервер отчетов, выполнив несколько простых тестов. Для выполнения этих шагов необходимо быть локальным администратором. Чтобы тестирование могли выполнить другие пользователи, необходимо сконфигурировать доступ к серверу отчетов для этих пользователей.  
  
### <a name="to-verify-that-the-report-server-is-installed-and-running"></a>Проверка успешной установки и запуска сервера отчетов  
  
1.  Запустите программу настройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и подключитесь к только что установленному экземпляру сервера отчетов. На странице «URL-адрес веб-службы» имеется ссылка на веб-службу сервера отчетов. Щелкните ссылку и убедитесь, что можете обращаться к серверу. Если база данных сервера отчетов не настроена, выполните ее настройку до активизации ссылки.  
  
2.  Откройте приложения командной строки служб и убедитесь в том, что служба сервера отчетов запущена. Для просмотра состояния службы сервера отчетов нажмите кнопку **Пуск**, выберите **Панель управления**, дважды щелкните **Администрирование** и откройте оснастку **Службы**. Когда появится список служб, найдите службу **Сервер отчетов (MSSQLSERVER)**. Состояние службы должно быть **Работает**.  
  
3.  Откройте браузер и в строке адреса введите URL-адрес сервера отчетов. Адрес должен состоять из имени сервера и имени виртуального каталога, которое было определено во время установки сервера отчетов. По умолчанию имя виртуального каталога сервера отчетов — **ReportServer**. Можно использовать следующий URL-адрес для проверки установки сервера отчетов: https:// *\<computer name>* /ReportServer *\<_instance name>* . URL-адрес будет другим, если сервер отчетов установлен как именованный экземпляр. Дополнительные сведения о формате URL-адресов см. в статье [Настройка URL-адресов сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md). Если вы являетесь локальным администратором систем Windows Vista или Windows Server 2008, ознакомьтесь со статьей [Настройка сервера отчетов, работающего в собственном режиме, для локального администрирования (службы SSRS)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
4.  Запустите отчет, чтобы проверить работу сервера отчетов. Для выполнения этого шага можно создать образец отчета из учебника. Дополнительные сведения см. в статье [Создание простого табличного отчета (учебник по службам SSRS)](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md).  
  
### <a name="to-verify-that-the-ssrswebportal-is-installed-and-running"></a>Проверка успешной установки и запуска [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]  
  
1.  Откройте браузер и в строке адреса введите URL-адрес веб-портала. Адрес состоит из имени сервера и имени виртуального каталога, указанного для [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] в процессе установки или на странице "URL-адрес веб-портала" в программе настройки служб Reporting Services. По умолчанию виртуальный каталог [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] называется **Reports**. Можно использовать следующий URL-адрес для проверки установки [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] :  
  
     https:// *\<computer name>* /Reports *\<_instance name>* .  
  
2.  Используйте [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] для создания новой папки или передачи файла с целью проверки, возвращаются ли определения обратно в базу данных сервера отчетов. Если эти операции успешно завершаются, то соединение функционирует.  
  
     Дополнительные сведения см. в статье [Веб-портал (службы SSRS в собственном режиме)](../../reporting-services/web-portal-ssrs-native-mode.md).  
  
### <a name="to-verify-that-report-designer-is-installed-and-running"></a>Проверка успешной установки и запуска конструктора отчетов  
  
1.  Откройте среду [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]и создайте новый проект, основанный на типе проекта сервера отчетов. Дополнительные сведения об использовании мастера проектов сервера отчетов см. в статье [Службы Reporting Services в SQL Server Data Tools &#40;SSDT&#41;](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).  
  
2.  Если установлены образцы отчетов, откройте файлы образцов проектов отчетов и опубликуйте отчеты на сервере отчетов.  
  
## <a name="see-also"></a>См. также:  
 [Устранение неполадок при установке служб Reporting Services](../../reporting-services/install-windows/troubleshoot-a-reporting-services-installation.md)   
 [Причины ошибок служб Reporting Services и способы их устранения](../../reporting-services/troubleshooting/cause-and-resolution-of-reporting-services-errors.md)  
  
  

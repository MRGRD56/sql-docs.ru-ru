---
title: Запуск построителя отчетов | Документы Майкрософт
description: Построитель отчетов является изолированной средой создания отчетов. При первом запуске в центре загрузки Майкрософт будет предложено скачать его.
ms.date: 01/03/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
helpviewer_keywords:
- Report Builder, launching
- launching Report Builder
- SharePoint integration [Reporting Services], starting Report Builder
- starting Report Builder
ms.assetid: 8c8c7d2e-b315-418d-bf65-90e7685e4259
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 00a954a23cf9b17a58c3272a03222019400ae891
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907042"
---
# <a name="start-report-builder"></a>Запуск построителя отчетов

[!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] является изолированной средой создания отчетов. С ее помощью можно создавать отчеты и публиковать их в среде [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , работающей в основном режиме или режиме интеграции с SharePoint.  

> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.
  
 При первом запуске [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] с веб-портала [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] или [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint вам будет предложено скачать средство из Центра загрузки Майкрософт. 
 
![Снимок экрана: сообщение "Идет открытие построителя отчетов".](../../reporting-services/report-builder/media/report-builder-get-report-builder.png) 
 
 Пользователь или администратор может также [установить построитель отчетов на компьютер из Центра загрузки Майкрософт](https://go.microsoft.com/fwlink/?LinkID=219138). См. подраздел "Установка построителя отчетов с помощью Systems Manager Server" в разделе [Установка построителя отчетов](../../reporting-services/install-windows/install-report-builder.md) .
 
 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] не устанавливается вместе со службами SQL Server Reporting Services. Его необходимо скачать и установить отдельно.  
  
 Если при запуске [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] на веб-портале или на сайте SharePoint отобразится более ранняя версия [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] , обратитесь к администратору, который может обновить версию на веб-портале или сайте SharePoint.  
  
## <a name="to-start-ssrbnoversion-from-the-ssrsnoversion-web-portal"></a>Запуск [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] на веб-портале [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  
  
1.  В веб-браузере введите в адресной строке URL-адрес для сервера отчетов. URL-адрес по умолчанию — https://\<*servername*>/reports.  
  
2.  На верхней панели веб-портала выберите **Создать** > **Отчет с разбиением на страницы**.  
  
     ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")  
  
     В первый раз будет предложено [установить построитель отчетов](../../reporting-services/install-windows/install-report-builder.md). 
  
     После этого откроется [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] , и можно будет приступить к созданию отчета или открыть отчет с сервера отчетов.  
  
## <a name="to-start-ssrbnoversion-in-sharepoint-integrated-mode"></a>Запуск [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] в режиме интеграции с SharePoint  
  
1.  Перейдите на сайт SharePoint, содержащий нужную библиотеку.  
  
2.  Откройте библиотеку.  
  
3.  Нажмите **Документы**.  
  
4.  В меню **Создать документ** выберите пункт **Отчет построителя отчетов**.  
  
     В первый раз будет запущен мастер [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] для SQL Server. Дополнительные сведения см. в разделе [Install Report Builder](../../reporting-services/install-windows/install-report-builder.md) .  
  
     [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] , и можно будет приступить к созданию отчета или открыть отчет на сервере отчетов.  
  
     **Примечание** . Если меню **Создать документ** не содержит параметры **Отчет построителя отчетов** , **Модель построителя отчетов** или **Источник данных отчета** , необходимо добавить типы их содержимого в библиотеку SharePoint. Дополнительные сведения см. в разделе [Добавление типов содержимого служб Reporting Services в библиотеку SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  

## <a name="next-steps"></a>Дальнейшие действия

[Построитель отчетов в SQL Server](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)   
[Определение параметров по умолчанию для построителя отчетов](../../reporting-services/report-builder/set-default-options-for-report-builder.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).

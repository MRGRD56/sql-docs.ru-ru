---
title: Параметры сайта SharePoint для веб-части "Средство просмотра отчетов" (SSRS) | Документы Майкрософт
description: Узнайте, как настроить параметры сайта SharePoint в веб-части "Средство просмотра отчетов" на сервере отчетов SQL Server.
ms.date: 11/15/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: jt000
ms.author: jasontre
ms.openlocfilehash: dfb8a3a33d56660bd8a7bdeeffb88824a3960dce
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596926"
---
# <a name="sharepoint-site-settings-for-the-report-viewer-web-part---reporting-services"></a>Параметры сайта SharePoint для веб-части "Средство просмотра отчетов" (службы Reporting Services)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

Веб-часть "Средство просмотра отчетов" имеет несколько параметров, которые можно настроить. Эти параметры может включать и отключать администратор сайта на странице параметров сайта SharePoint. Каждый сайт имеет собственные параметры. Кроме того, эти параметры не будут сброшены после переустановки веб-части "Средство просмотра отчетов".

## <a name="accessing-the-site-settings-page"></a>Открытие страницы "Параметры сайта"

Доступ к параметрам сайта можно получить одним из указанных ниже способов.

1. На сайте SharePoint щелкните значок **шестеренки** в левом верхнем углу и выберите пункт **Параметры сайта**.

    ![Параметры сайта в меню со значком шестеренки.](media/sharepoint-site-settings.png)

2. Щелкните **Параметры веб-части "Средство просмотра отчетов"** в группе параметров сайта **Службы Reporting Services**.

    > [!NOTE]
    > Доступ к параметрам сайта можно также получить, перейдя напрямую к файлу `<site>/_layouts/15/ReportViewerWebPart/ReportViewerWebPartSettings.aspx`.

## <a name="report-viewer-web-part-settings"></a>Параметры веб-части "Средство просмотра отчетов"

|Параметр|Комментарии|  
|-------------|--------------|  
|Сбор данных об использовании|Позволяет отправлять сведения об ошибках и использовании в корпорацию Майкрософт для улучшения продуктов. Правила Майкрософт в отношении сбора данных и сообщений об ошибках см. в [заявлении о конфиденциальности Microsoft SQL Server](../../sql-server/sql-server-privacy.md).|  
|Включение для отчетов метаданных о специальных возможностях|Задает [сведения об устройстве `AccessibleTablix`](../html-device-information-settings.md) для отчетов, подготавливаемых к просмотру.|
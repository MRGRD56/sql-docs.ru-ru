---
title: Поддержка браузера для служб Reporting Services и Power View | Документы Майкрософт
description: Узнайте, какие версии браузера поддерживаются для просмотра служб SQL Server Reporting Services, элементов управления ReportViewer и Power View и управления ими.
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- displaying reports
- scripts [Reporting Services], requirements
- viewing reports
- browsers [Reporting Services]
- Web browsers [Reporting Services], about browser support
- browsing reports [Reporting Services]
- components [Reporting Services], browsers
- Web browsers [Reporting Services]
ms.assetid: 48a75bbb-0029-4c43-891d-dc8f4fc0ebe1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: aa8a9fbbcadfcdd3c266de4b1453ea7e5402e1af
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466655"
---
# <a name="browser-support-for-reporting-services-and-power-view"></a>Поддержка браузера для служб Reporting Services и Power View

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Узнайте, какие версии браузера поддерживаются для просмотра служб SQL Server Reporting Services, элементов управления ReportViewer и Power View и управления ими.

> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.

## <a name="browser-requirements-for-the-web-portal"></a>Требования веб-портала к браузеру

Далее приведен текущий список поддерживаемых браузеров для веб-портала.

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*
- Microsoft Edge (+)
- Microsoft Internet Explorer 10 или 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple iOS**  
*iPhone и iPad с iOS 9*

- Apple Safari (+)

**Google Android**  
*Смартфоны и планшетные ПК с Android 4.4 (KitKat) или более поздней версии*

- Google Chrome (+)

 **(+)** Последняя выпущенная версия

## <a name="browser-requirements-for-the-reportviewer-web-control-2015"></a>Требования к браузеру для элемента управления ReportViewer (2015)

 Далее приведен текущий список поддерживаемых браузеров для работы со средством просмотра отчетов (2015). Средство просмотра отчетов поддерживает просмотр отчетов с веб-портала [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] и библиотек SharePoint.  

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 или 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** Последняя выпущенная версия

::: moniker range="=sql-server-2016"

 При использовании продукта SharePoint, интегрированного со службами [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], ознакомьтесь с разделом  [Планирование поддержки браузеров в SharePoint 2016](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx).

::: moniker-end

### <a name="authentication-requirements"></a>Требования к проверке подлинности

 Браузеры поддерживают определенные схемы проверки подлинности, обработка которых сервером отчетов является обязательным условием успешного выполнения клиентского запроса. В следующей таблице показаны типы проверки подлинности по умолчанию, поддерживаемые каждым браузером в операционной системе Windows.

|**Тип браузера**|**Поддерживает**|**Выбор браузера по умолчанию**|**Выбор по умолчанию сервера**|
|----------------------|------------------|-------------------------|------------------------|
|**Microsoft Edge** (+)|Negotiate, Kerberos, NTLM, Basic|Согласование|Да. Используемые по умолчанию параметры проверки подлинности совместимы с Edge.|
|**Microsoft Internet Explorer**|Negotiate, Kerberos, NTLM, Basic|Согласование|Да. Параметры проверки подлинности по умолчанию совместимы с Internet Explorer.|
|**Google Chrome**(+)|Negotiate, NTLM, Basic|Согласование|Да. Параметры проверки подлинности по умолчанию совместимы с Chrome.|
|**Mozilla Firefox**(+)|NTLM, базовый|NTLM|Да. Параметры проверки подлинности по умолчанию совместимы с Firefox.|
|**Apple Safari**(+)|NTLM, базовый|Basic|Да. Параметры проверки подлинности по умолчанию совместимы с Safari.|

 **(+)** Последняя выпущенная версия

### <a name="script-requirements-for-viewing-reports"></a>Требования к скриптам для просмотра отчетов

 Чтобы использовать средство просмотра отчетов, настройте браузер для выполнения скриптов.

 Если поддержка скриптов отключена, появится сообщение об ошибке, похожее на следующее при открытии отчета:

- **Браузер не поддерживает выполнение скриптов, или выполнение скриптов запрещено в его настройках. Щелкните здесь для просмотра отчета без выполнения сценариев**.

 При просмотре отчета без поддержки выполнения скриптов отчет готовится к просмотру в формате HTML без таких средств просмотра отчетов, как панель инструментов отчета и схема документа.

> [!NOTE]
> Панель инструментов отчета является частью компонента «Средство просмотра HTML-страниц». По умолчанию панель инструментов появляется в верхней части каждого отчета, который отображается в окне браузера. C помощью средства просмотра отчета можно, в частности, выполнять поиск данных в отчете, прокручивать страницы и настраивать размер страниц для просмотра. Дополнительные сведения о панели инструментов отчета и средстве просмотра HTML-страниц см. в разделе [HTML Viewer and the Report Toolbar](../reporting-services/html-viewer-and-the-report-toolbar.md).

## <a name="browser-support-for-reportviewer-web-server-controls-in-visual-studio"></a>Поддержка браузеров для серверных веб-элементов управления ReportViewer в Visual Studio

 Серверный веб-элемент управления ReportViewer используется для внедрения функций отчетов в веб-приложение ASP.NET. Эти элементы управления имеются в Visual Studio. Они могут поддерживать браузеры и версии браузеров, которые отличаются от браузеров и версий браузеров, поддерживаемых другими компонентами, описанными в этом разделе. Тип браузера, используемого для просмотра приложения, определяет то, какие функции ReportViewer могут быть реализованы в приложении. Используйте таблицу, приведенную в этом разделе, для определения того, какие из поддерживаемых браузеров имеют ограничения по функциям отчетов. В ней также указаны поддерживаемые платформы.  

 Используйте браузер, в котором включена поддержка скриптов. Если браузер не может выполнять скрипты, отчеты в нем просматривать нельзя.

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 или 11
- Google Chrome (+)
- Mozilla Firefox (+)

 **(+)** Последняя выпущенная версия

## <a name="power-view-browser-support"></a>Поддержка Power View в браузерах

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Internet Explorer 10 или 11
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** Последняя выпущенная версия

::: moniker range="=sql-server-2016"

 Дополнительные сведения о поддержке браузеров в SharePoint 2016 см. в разделе [Планирование поддержки браузеров в SharePoint 2013](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx).

::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

[Поиск и просмотр отчетов на веб-портале](report-builder/finding-and-viewing-reports-in-the-web-portal-report-builder-and-ssrs.md)  
[Инструментальные средства служб Reporting Services](../reporting-services/tools/reporting-services-tools.md)  
[Веб-портал (собственный режим служб SSRS)](./web-portal-ssrs-native-mode.md)  
[Просмотра HTML-страниц и панель инструментов отчета](../reporting-services/html-viewer-and-the-report-toolbar.md)  
[Ссылка на параметр доступа по URL-адресу](../reporting-services/url-access-parameter-reference.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
---
title: Подготовка к реализации модуля доставки | Документы Майкрософт
description: Узнайте, как реализовать модуль доставки в Reporting Services. Узнайте о доступных интерфейсах и классах, а также об обязательных и дополнительных функциональных возможностях.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- interfaces [Reporting Services]
- delivery extensions [Reporting Services], implementing
ms.assetid: aee1608d-374f-4ad3-bc23-fe07fdaa52b7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f3c344307281fee1a49d251a9d4cf61c147efba3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065059"
---
# <a name="preparing-to-implement-a-delivery-extension"></a>Подготовка к реализации модуля доставки
  Перед реализацией модуля доставки служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] следует определить реализуемые интерфейсы. Вначале необходимо решить, как будет использоваться модуль доставки, какие параметры будут необходимы для модуля и какие функции будет необходимо реализовать для доставки уведомлений об отчетах.  
  
 Каждый модуль доставки служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] должен предоставлять следующие возможности:  
  
-   реализацию интерфейса <xref:Microsoft.ReportingServices.Interfaces.IExtension>, который представляет модуль и локализованное имя модуля;  
  
-   реализацию интерфейса <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension>, который создает модуль доставки, используемый для доставки пользователям уведомлений об отчетах;  
  
-   возможность обработки пользовательских данных для подписки.  
  
 Каждый модуль доставки можно улучшить, добавив следующие возможности:  
  
-   реализацию пользовательского элемента управления [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)], которая дает пользователям возможность использовать диспетчер отчетов для создания подписок на отчеты, использующих модуль доставки.  
  
 В следующей таблице описаны доступные интерфейсы и классы для модулей доставки.  
  
|Интерфейс или класс|Описание|  
|------------------------|-----------------|  
|<xref:Microsoft.ReportingServices.Interfaces.IExtension> Интерфейс|Представляет модуль в службах [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
|<xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension> Интерфейс|Представляет модуль доставки в службах [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].|  
|<xref:Microsoft.ReportingServices.Interfaces.IDeliveryReportServerInformation> Интерфейс|Содержит сведения о сервере отчетов, которые необходимы модулям доставки (например, список доступных модулей подготовки отчетов).|  
|Класс <xref:Microsoft.ReportingServices.Interfaces.Setting>|Представляет параметр модуля.|  
|Класс <xref:Microsoft.ReportingServices.Interfaces.Notification>|Содержит сведения о подписке, используемые модулями доставки для доставки отчетов.|  
|Класс <xref:Microsoft.ReportingServices.Interfaces.Report>|Представляет сведения об отчете и методы, которые позволяют модулям доставки доставлять пользователям отчеты.|  
|Класс <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile>|Представляет выходной файл модуля подготовки отчетов. Объект <xref:Microsoft.ReportingServices.Interfaces.RenderedOutputFile> содержит имя связанного файла и сведения о типе, необходимые модулю доставки для обработки потока, возвращаемого модулем подготовки отчетов.|  
|<xref:Microsoft.ReportingServices.Interfaces.ISubscriptionBaseUIUserControl> Интерфейс|Пользовательский элемент управления, служащий средством получения данных о подписке, относящихся к модулю доставки, от пользователя в диспетчере отчетов (например, адрес электронной почты или путь в общую папку).|  
  
## <a name="see-also"></a>См. также:  
 [Модули служб Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Реализация модуля доставки](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Библиотека модулей Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  

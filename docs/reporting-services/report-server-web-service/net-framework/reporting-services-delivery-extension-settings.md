---
title: Параметры модуля доставки служб Reporting Services | Документы Майкрософт
description: Reporting Services обеспечивают отправку отчетов пользователям по электронной почте и доставку файлов в общую папку для отправки отчетов в общую папку. Узнайте подробнее о параметрах модуля доставки.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- XML Web service [Reporting Services], delivery extension settings
- Report Server Web service, delivery extension settings
- delivery [Reporting Services]
- network share delivery [Reporting Services]
- file share delivery [Reporting Services]
- e-mail [Reporting Services]
- mailing reports [Reporting Services]
- sharing reports
- extensions [Reporting Services], delivery
- mail [Reporting Services]
- Web service [Reporting Services], delivery extension settings
ms.assetid: 68c31a85-261c-4ec4-b8df-1f9842b46f8a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6249d9a0edc81dac3cfe384716d425e7ba0f83e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063679"
---
# <a name="reporting-services-delivery-extension-settings"></a>Параметры модулей доставки служб Reporting Services
  В [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] включены модуль доставки по электронной почте и модуль доставки в общую папку. Доставка по электронной почте позволяет переслать отчет отдельным пользователям или группам по электронной почте. Доставка в общую папку позволяет автоматически отправлять подготовленные отчеты в сетевую папку. Любой из поддерживаемых модулей доставки можно использовать со стандартными подписками или с управляемыми данными подписок. Настройки доставки, характерные для типа модуля доставки, передаются при каждом вызове методов <xref:ReportService2010.ReportingService2010.CreateSubscription%2A>,<xref:ReportService2010.ReportingService2010.CreateDataDrivenSubscription%2A>,<xref:ReportService2010.ReportingService2010.SetSubscriptionProperties%2A> и <xref:ReportService2010.ReportingService2010.SetDataDrivenSubscriptionProperties%2A>. Для получения списка настроек доставки программным образом можно использовать метод <xref:ReportService2010.ReportingService2010.GetExtensionSettings%2A>.  
  
> [!NOTE]  
>  Настройки модуля доставки учитывают регистр.  
  
## <a name="e-mail-delivery-settings"></a>Настройки доставки электронной почты  
 В следующей таблице перечисляются настройки доставки для подписок, использующих электронную почту сервера отчетов.  
  
|Параметр|Значение|  
|-------------|-----------|  
|**Кому**|Адрес электронной почты, отображаемый в строке **Кому** в сообщении электронной почты. Несколько адресов разделяются точками с запятой. Обязательный элемент.|  
|**Копия**|Адрес электронной почты, отображаемый в строке **Копия** в сообщении электронной почты. Несколько адресов разделяются точками с запятой. Необязательный параметр.|  
|**Скрытая копия**|Адрес электронной почты, отображаемый в строке **Скрытая копия** в сообщении электронной почты. Несколько адресов разделяются точками с запятой. Необязательный параметр.|  
|**ReplyTo**|Адрес электронной почты, отображаемый в заголовке **ReplyTo** в сообщении электронной почты. Значение должно быть одиночным адресом электронной почты. Необязательный параметр.|  
|**IncludeReport**|Значение, показывающее, необходимо ли включать отчет в доставку по электронной почте. Значение **true** указывает, что отчет доставляется в тексте сообщения электронной почты.|  
|**RenderFormat**|Имя модуля подготовки отчетов, используемого для создания подготовленного отчета. Имя должно соответствовать одному из видимых модулей подготовки отчетов, установленных на сервере отчетов. Данное значение необходимо, если для параметра **IncludeReport** установлено значение **true**.|  
|**Приоритет**|Приоритет отправки сообщения электронной почты. Допустимые значения: **Низкий**, **Обычный** и **Высокой**. Значение по умолчанию — **Обычный**.|  
|**Тема**|Текст в строке темы сообщения электронной почты.|  
|**Комментарий**|Текст, включенный в тело сообщения электронной почты.|  
|**IncludeLink**|Значение, показывающее, необходимо ли включать ссылку на отчет в текст сообщения.|  
  
## <a name="file-share-delivery-settings"></a>Параметры доставки в общую папку  
 В следующей папке перечисляются параметры доставки в общую папку для подписок.  
  
|Параметр|Значение|  
|-------------|-----------|  
|**FILENAME**|Имя файла, сохраняемого на диск.|  
|**FILEEXTN**|Указывает, стоит ли включать расширение файла в отчет, готовый для просмотра. Имеет значение **true** или **false**.|  
|**PATH**|Путь папки или путь общей папки в формате UNC, в которую будет сохранен отчет.|  
|**RENDER_FORMAT**|Формат отчета, сохраняемого на диск.|  
|**USERNAME**|Имя пользователя, необходимое для доступа к сетевому ресурсу или диску.|  
|**PASSWORD**|Пароль, необходимый для доступа к сетевому ресурсу или диску.|  
|**WRITEMODE**|Режим записи, используемый при доступе к диску. Допустимые значения: **None**, **Overwrite** и **AutoIncrement**.|  
  
## <a name="see-also"></a>См. также:  
 [Технический справочник (службы SSRS)](../../../reporting-services/technical-reference-ssrs.md)   
 [Создание приложений с помощью веб-службы и .NET Framework](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
  
  

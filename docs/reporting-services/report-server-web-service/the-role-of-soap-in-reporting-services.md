---
title: Роль протокола SOAP в службах Reporting Services | Документы Майкрософт
description: Веб-служба сервера отчетов использует SOAP по HTTP. Диспетчер отчетов предоставляет интерфейс для базы данных сервера отчетов, в которой хранятся все отчеты и относящееся к ним содержимое.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- Web service [Reporting Services], SOAP
- SOAP [Reporting Services], role in Reporting Services
- Report Server Web service, SOAP
- XML Web service [Reporting Services], SOAP
ms.assetid: f229c3ef-f2ca-448f-98f1-b8df350b9992
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8f11ec9625a675e42d712bd1bb1bba953ce979c0
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934579"
---
# <a name="the-role-of-soap-in-reporting-services"></a>The Role of SOAP in Reporting Services
  Веб-служба сервера отчетов использует обмен сообщениями по протоколу SOAP для отправки текстовых команд по сети. Эти команды имеют вид XML-текста, передаваемого через Интернет по протоколу HTTP. Использование протокола связи SOAP в веб-службе сервера отчетов позволяет приложениям и компонентам обмениваться данными с сервером отчетов, используя широко признанную открытую инфраструктуру. Стандарт SOAP определен на веб-сайте www.w3.org/TR/SOAP.  
  
 В качестве клиента SOAP может выступать любое приложение, которое поддерживает протокол SOAP и может отправлять SOAP-запросы. Примером такого клиента SOAP является диспетчер отчетов. Он предоставляет интерфейс к базе данных сервера отчетов, в которой хранятся все отчеты и относящееся к ним содержимое. Пользователи этого приложения могут просматривать папки и отчеты в пространстве имен сервера отчетов и работать с отчетами. Диспетчер отчетов построен на инфраструктуре веб-службы сервера отчетов.  
  
 Сервер отчетов выступает в роли сервера SOAP — службы, которая поддерживает протокол SOAP, может принимать запросы от клиентов SOAP и формировать необходимые ответные сообщения. Сервер обрабатывает запросы и посылает клиенту закодированные ответные сообщения.  
  
 Сообщения SOAP в службах [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] могут принимать множество различных форм в зависимости от типа запроса, выполненного клиентом. В следующем примере показан простой запрос клиента SOAP для удаления элемента из базы данных сервера отчетов.  
  
```  
<soap:Envelope xmlns:soap="https://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItem xmlns="https://www.microsoft.com/sql/ReportingServer">  
            <item>/Samples/Report1</item>  
        </DeleteItem>  
    </soap:Body>  
</soap:Envelope>  
```  
  
 Протокол SOAP требует, чтобы сообщения помещались в элемент **Envelope**, а большая часть сообщения располагалась в элементе **Body**. В этом примере элемент Body содержит вызов метода <xref:ReportService2010.ReportingService2010.DeleteItem%2A>, который принимает строковый параметр, представляющий путь к удаляемому элементу. Можно создать класс-посредник клиента [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], в котором все операции SOAP инкапсулируются в методы. Следующий метод [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] представляет пример SOAP-запроса, приведенный ранее.  
  
```  
public void DeleteItem(string item);  
```  
  
 Ответ от сервера имеет приблизительно следующий вид:  
  
```  
<soap:Envelope xmlns:soap="https://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
    <soap:Body>  
        <DeleteItemResponse xmlns="https://www.microsoft.com/sql/ReportingServer" />  
    </soap:Body>  
</soap:Envelope>  
```  
  
 Метод <xref:ReportService2010.ReportingService2010.DeleteItem%2A> не имеет возвращаемого значения, поэтому возвращается пустой ответ.  
  
## <a name="see-also"></a>См. также:  
 [Доступ к API SOAP](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)   
 [Диспетчер отчетов (службы Reporting Services в основном режиме)](../web-portal-ssrs-native-mode.md)   
 [Сервер отчетов служб Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)   
 [Веб-службы сервера отчетов](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  

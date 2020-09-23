---
description: Настройка передачи файлов cookie для пользовательской проверки подлинности на веб-портале
title: Настройка передачи файлов cookie для пользовательской проверки подлинности на веб-портале | Документы Майкрософт
ms.date: 04/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- authentication [Reporting Services]
- extensions [Reporting Services], custom security
ms.assetid: 91aeb053-149e-4562-ae4c-a688d0e1b2ba
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 19b799424f0c860fb85ca44f3d539edb009b43ba
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88454506"
---
# <a name="configure-the-web-portal-to-pass-custom-authentication-cookies"></a>Настройка передачи файлов cookie для пользовательской проверки подлинности на веб-портале

При использовании пользовательского модуля проверки подлинности необходимо настроить веб-портал для передачи файлов cookie пользовательской проверки подлинности. В противном случае веб-портал будет передавать файлы cookie только через HTTP-запрос для сервера отчетов. Чтобы передать дополнительные куки-файлы, необходимо изменить файл RSReportServer.Config.

## <a name="modifying-the-rsreportserverconfig-file"></a>Изменение файла RSReportServer.Config

Чтобы включить передачу [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] дополнительных файлов cookie через сервер отчетов, необходимо добавить элемент \<**PassThroughCookies**> к настройкам конфигурации веб-портала в файле RSReportServer.config. Использование дополнительных куки-файлов удобно в решениях однократной проверки подлинности при входе, когда требуются не только куки-файлы проверки подлинности сервера отчетов, но также куки-файлы от сторонних систем проверки подлинности.

Чтобы включить передачу дополнительных файлов cookie через HTTP-запрос при использовании веб-портала, установите следующие элементы в файле RSReportServer.config:
  
```  
<UI>  
   <CustomAuthenticationUI>  
      ...  
      <PassThroughCookies>  
         <PassThroughCookie>cookiename1</PassThroughCookie>  
         <PassThroughCookie>cookiename2</PassThroughCookie>  
      </PassThroughCookies>  
   </CustomAuthenticationUI>  
      ...  
</UI>  
```  
  
## <a name="see-also"></a>См. также:

[Проверка подлинности с использованием сервера отчетов](../../reporting-services/security/authentication-with-the-report-server.md)   
[RsReportServer.config Configuration File](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Общие сведения о модулях безопасности](../../reporting-services/extensions/security-extension/security-extensions-overview.md)   
[Настройка и администрирование сервера отчетов (службы Reporting Services в собственном режиме)](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)  
Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)

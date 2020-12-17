---
title: Экспорт отчета с применением доступа по URL-адресу | Документы Майкрософт
description: Узнайте, как экспортировать отчет с помощью доступа по URL-адресу, указав формат отображения отчета с помощью параметра URL-адреса rs:Format.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- formats [Reporting Services], URL rendering
- URL access [Reporting Services], rendering formats
ms.assetid: 6a3b7fc3-3d91-4d12-8371-42ea12e74517
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d61f509fa07528b71cd7cbb23488bc14f33959ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472495"
---
# <a name="export-a-report-using-url-access"></a>Экспорт отчета с применением доступа по URL-адресу
  Можно также указать формат, в котором будет подготовлен отчет, используя параметр URL-адреса *rs:Format* .  Форматы HTML4.0 и HTML5 (модули подготовки отчетов) отображаются прямо в браузере, а для всех остальных форматов браузер предложит сохранить результат в виде локального файла.  
  
 Например, чтобы получить копию отчета в формате PDF прямо с сервера отчетов, работающего в собственном режиме:  
  
```  
https://myrshost/ReportServer?/myreport&rs:Format=PDF  
```  

::: moniker range="=sql-server-2016"
  
 И с сервера отчетов, работающего в режиме интеграции с SharePoint:  
  
```  
https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/myrereport.rdl&rs:Format=PDF  
```  
 
::: moniker-end
 
 Например, следующая URL-команда в браузере экспортирует отчет из именованного экземпляра сервера отчетов в формат PPTX.  
  
```  
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 Допустимые значения для этого параметра основаны на модулях подготовки отчетов, установленных на сервер отчетов, к которому осуществляется доступ. Поддерживаемые модули: HTML4.0, MHTML, IMAGE, EXCELOPENXML (xlsx), WORDOPENXML (docx), CSV, CSV, PDF, XML и NULL. Если указанный модуль подготовки отчетов не установлен на сервере отчетов, то отчет не будет подготовлен, а браузере выдаст ошибку.  
  
 Если в URL-адрес не включен параметр *Format* , то сервер отчетов определяет браузер и подготавливает для него отчет в соответствующем HTML-формате.  
  
## <a name="see-also"></a>См. также:  
 [Доступ по URL-адресу (службы SSRS)](../reporting-services/url-access-ssrs.md)   
 [Ссылка на параметр доступа по URL-адресу](../reporting-services/url-access-parameter-reference.md)  
  
  

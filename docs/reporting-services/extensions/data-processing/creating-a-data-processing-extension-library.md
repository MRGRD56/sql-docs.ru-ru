---
title: Создание библиотеки модулей обработки данных | Документы Майкрософт
description: Узнайте, как создать модуль обработки данных Reporting Services. Просмотрите пример кода и узнайте, какие требования к пространству имен и файлам библиотек необходимо выполнить.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], namespace assignments
- library [Reporting Services]
- assigning namespaces to extensions
ms.assetid: 82f4b71b-dd39-467d-8d8c-6771eb2b12de
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bd55657e325035821798c2b7d6a960a6f78cd59b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065449"
---
# <a name="creating-a-data-processing-extension-library"></a>Создание библиотеки модулей обработки данных
  Каждому созданному модулю обработки данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] необходимо присвоить уникальное пространство имен. Кроме того, он должен быть встроен в библиотеку или файл сборки. Конкретное имя пространства имен не имеет значения, однако оно должно быть уникальным и не должно использоваться в других расширениях. [!INCLUDE[msCoName](../../../includes/msconame-md.md)] использует пространства имен <xref:Microsoft.ReportingServices.DataProcessing> для модулей обработки данных, поставляемых со службами [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Для модулей обработки данных своей компании следует создавать собственные уникальные пространства имен.  
  
 В следующем примере показывается код, позволяющий начать создание модуля обработки данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], использующего пространства имен, содержащие интерфейсы обработки данных и служебные классы.  
  
```vb  
Imports System  
Imports Microsoft.ReportingServices.DataProcessing  
Imports Microsoft.ReportingServices.Interfaces  
  
Namespace CompanyName.ExtensionName  
   ...  
```  
  
```csharp  
using System;  
using Microsoft.ReportingServices.DataProcessing;  
using Microsoft.ReportingServices.Interfaces;  
  
namespace CompanyName.ExtensionName  
{  
   ...  
```  
  
 При компиляции модуля обработки данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] следует предоставить компилятору ссылку на файл Microsoft.ReportingServices.Interfaces.dll, поскольку в нем хранятся интерфейсы модуля обработки данных и классы. Пространство имен <xref:Microsoft.ReportingServices.DataProcessing> необходимо для реализации интерфейсов модулей обработки данных; пространство имен <xref:Microsoft.ReportingServices.Interfaces> необходимо для реализации интерфейса <xref:Microsoft.ReportingServices.Interfaces.IExtension>. Например, если бы все файлы, содержащие код (на языке C#), необходимый для реализации модуля обработки данных служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], находились в одном каталоге с расширением CS, то для компиляции файлов, хранимых в библиотеке CompanyName.ExtensionName.dll из данного каталога, нужно было выполнить следующую команду:  
  
```csharp  
csc /t:library /out:CompanyName.ExtensionName.dll *.cs /r:System.dll /r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
 В следующем примере кода показана команда, которая используется для файлов [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] с расширением VB.  
  
```vb  
vbc /t:library /out:CompanyName.ExtensionName.dll *.vb /r:System.dll /r:Microsoft.ReportingServices.Interfaces.dll  
```  
  
> [!NOTE]  
>  Также можно проектировать, разрабатывать и строить модуль обработки данных в среде [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. Дополнительные сведения о разработке сборок в среде [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] см. в документации по среде [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>См. также:  
 [Модули служб Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Реализация модуля обработки данных](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Библиотека модулей Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  

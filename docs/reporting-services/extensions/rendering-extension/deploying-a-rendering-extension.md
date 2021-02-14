---
title: Развертывание модуля подготовки отчетов | Документы Майкрософт
description: Описание развертывания модуля подготовки отчетов. Сведения о том, какие записи файла конфигурации следует добавить, чтобы сервер отчетов и конструктор отчетов нашли модуль.
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- deploying [Reporting Services], extensions
- rendering extensions [Reporting Services], deploying
ms.assetid: 9fb8c887-5cb2-476e-895a-7b0e2dd11398
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 56c22120fb08b538b845647f1a2300c22e17d017
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042216"
---
# <a name="deploying-a-rendering-extension"></a>Развертывание модуля подготовки отчетов
  После того как модуль подготовки отчетов служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] был написан и скомпилирован в библиотеку [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], необходимо сделать его видимым для сервера отчетов и конструктора отчетов. Это можно сделать, скопировав модуль в подходящий каталог и добавив записи в подходящие файлы конфигурации служб [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
## <a name="configuration-file-rendering-extension-element"></a>Настройка элемента модуля подготовки отчетов в файле  
 После компиляции модуля подготовки отчетов в формат .DLL в файл rsreportserver.config добавляется запись. По умолчанию этот файл находится в папке %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer. Родительский элемент — \<Render>. В элементе Render находятся элементы Extension для каждого модуля подготовки отчетов. Элемент **Extension** содержит два атрибута — Name и Type.  
  
 В следующей таблице описываются атрибуты элемента **Extension** для модулей подготовки отчетов.  
  
|Атрибут|Описание|  
|---------------|-----------------|  
|**Имя**|Уникальное имя элемента Extension. Длина атрибута **Name** не должна превышать 255 символов. Имя должно быть уникальным среди всех элементов, вложенных в элемент **Extensions** файла конфигурации. Если присутствует повторяющееся имя, сервер отчетов возвращает ошибку.|  
|**Тип**|Список с разделителями-запятыми, содержащий полное пространство имен и имя сборки.|  
|**Visible**|Значение **false** показывает, что модуль подготовки отчетов не должен быть видим в пользовательских интерфейсах. Если атрибут не указан, по умолчанию используется значение **true**.|  
|**LogAllExecutionRequests**|Значение **false** показывает, что запись регистрируется только для первого выполнения отчета в сеансе. Если атрибут не указан, по умолчанию используется значение **true**.<br /><br /> Например, этот параметр показывает, нужно ли регистрировать запись только для первой страницы, которая готовится к просмотру в составе отчета (значение **false**), или нужно создавать запись для каждой страницы, которая готовится к просмотру в составе отчета (значение **true**).|  
  
 Дополнительные сведения см. в разделе [Файл конфигурации RsReportServer.config](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
## <a name="deploying-the-extension-to-the-report-server"></a>Развертывание модуля на сервере отчетов  
 Сервер отчетов использует модули подготовки отчетов для экспорта отчетов в других форматах. Сборка модуля подготовки отчетов развертывается на сервере отчетов как закрытая сборка. Нужно также внести запись в файл конфигурации сервера отчетов rsreportserver.config.  
  
### <a name="to-deploy-the-assembly"></a>Развертывание сборки  
  
1.  Скопируйте сборку из промежуточной папки в каталог bin сервера отчетов, на котором будет использоваться модуль подготовки отчетов. По умолчанию каталог Bin сервера отчетов находится в %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer\Bin.  
  
2.  Скопировав файл сборки, откройте файл rsreportserver.config. Файл rsreportserver.config также расположен в каталоге bin сервера отчетов. Необходимо создать запись в файле конфигурации для файла сборки модуля. Файл можно открыть с помощью среды [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] или воспользоваться простым текстовым редактором.  
  
     Дополнительные сведения см. в разделе [Файл конфигурации RsReportServer.config](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
3.  В файле Rsreportserver.config найдите элемент **Render** . Запись для созданного модуля должна находиться в следующем разделе файла:  
  
    ```  
    <Extensions>  
       <Render>  
          <extension configuration>  
       </Render>  
    </Extensions>  
    ```  
  
4.  Добавьте запись для модуля подготовки отчетов. В новую запись должен входить элемент, для которого заданы параметры **Name** и **Type**, например  
  
    ```  
    <Extension Name="My Rendering Extension Name" Type="CompanyName.ExtensionName.MyRenderingProvider, AssemblyName" />  
    ```  
  
     Значение атрибута **Name** является уникальным именем модуля подготовки отчетов. Значение атрибута **Type** — это список с разделителями-запятыми, который содержит запись пространства имен с полным именем реализации <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension>, за которым следует имя сборки (без расширения DLL в имени файла). По умолчанию модули подготовки отчетов являются видимыми. Чтобы скрыть модуль в таких пользовательских интерфейсах, как диспетчер отчетов, добавьте атрибут **Visible** к элементу **Extension** и задайте для него значение **false**.  
  
## <a name="verifying-the-deployment"></a>Проверка развертывания  
 Можно также открыть диспетчер отчетов и убедиться, что модуль включен в список доступных типов экспорта отчета.  
  
## <a name="see-also"></a>См. также:  
 [Реализация модуля подготовки отчетов](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Общие сведения о модулях подготовки отчетов](../../../reporting-services/extensions/rendering-extension/rendering-extensions-overview.md)   
 [Реализация интерфейса IRenderingExtension](../../../reporting-services/extensions/rendering-extension/implementing-the-irenderingextension-interface.md)   
 [Рекомендации по обеспечению безопасности для модулей](../../../reporting-services/extensions/security-considerations-for-extensions.md)  
  
  

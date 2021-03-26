---
title: Устранение неполадок при подготовке отчетов служб Reporting Services
description: Эта статья описывает устранение проблем с разработкой, предварительным просмотром, экспортом, публикацией отчетов или их просмотром на сервере отчетов в собственном режиме или режиме SharePoint.
ms.date: 02/27/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
ms.assetid: a705d103-85b1-49b5-b27f-332b1040d029
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8812cfc8ac70da47da0414534ff380dd07686037
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104792945"
---
# <a name="troubleshoot--reporting-services-report-issues"></a>Устранение неполадок при подготовке отчетов служб Reporting Services
Этот раздел поможет в устранении проблем в работе [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] при проектировании отчета, предварительном просмотре отчета, публикации отчета на сервере отчетов в основном режиме или режиме SharePoint, а также при просмотре отчета на сервере отчетов или экспорте отчета в другой формат файла.  
## <a name="monitor-report-servers"></a>Наблюдение за серверами отчетов  
Для наблюдения за действиями сервера отчетов можно использовать средства системы и базы данных. Можно также просмотреть файлы журнала трассировки сервера отчетов или запросить журнал выполнения сервера отчетов о деталях конкретных отчетов. Если используется системный монитор, можно добавить счетчики производительности для веб-службы сервера отчетов и службы Windows, чтобы идентифицировать узкие места в обработке по запросу и по расписанию.  
Дополнительные сведения см. в разделе [Наблюдение за производительностью сервера отчетов](../report-server/monitoring-report-server-performance.md).  
  
  
## <a name="view-the-report-server-logs"></a>Просмотр журналов сервера отчетов  
[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion.md)] записывают многие внутренние и внешние события в файлы журналов, куда заносятся данные о конкретных отчетах, сведения об отладке, HTTP-запросы и ответы, а также события сервера отчетов. Можно также создавать журналы производительности и выбирать счетчики, которые указывают, какие данные должны собираться. Используемый по умолчанию каталог файлов журнала для установки по умолчанию — `<drive>\Program Files\Microsoft SQL Server\MSRS130.MSSQLSERVER\Reporting Services\LogFiles`.   
  
Дополнительные сведения см. в разделе [Файлы и источники журналов служб Reporting Services](../report-server/reporting-services-log-files-and-sources.md).  
  
Чтобы определить, связано ли ожидание при формировании отчета с получением данных, обработкой отчета или его подготовкой к просмотру, используйте журнал выполнения. Дополнительные сведения см. в статье [Журнал выполнения сервера отчетов и представление ExecutionLog3](../report-server/report-server-executionlog-and-the-executionlog3-view.md).   
  
## <a name="view-the-call-stack-for-report-processing-error-messages-on-the-report-server"></a>Просмотр сообщений об ошибках обработки отчетов в стеке вызова на сервере отчетов  
При просмотре опубликованного отчета в диспетчере отчетов можно увидеть сообщение об общей ошибке обработки или подготовки. Чтобы ознакомиться с дополнительными сведениями, можно просмотреть стек вызова.   
  
Чтобы просмотреть стек вызова, войдите на сервер отчетов при помощи учетных данных администратора, правой кнопкой мыши щелкните страницу "Диспетчер отчетов", а затем выберите **Просмотреть источник**. Стек вызова предоставляет подробный контекст сообщения об ошибке.  
  
## <a name="use-ssmanstudiofull-to-verify-queries-and-credentials"></a>Использование [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] для проверки запросов и учетных данных  
Можно использовать [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull.md)] для проверки сложных запросов перед их добавлением в отчет.   
  
Дополнительные сведения см. в статьях [Редактор запросов SQL Server Management Studio (SSMS)](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md) и [Управление объектами с помощью обозревателя объектов](../../ssms/object/manage-objects-by-using-object-explorer.md).  
  
## <a name="analyze-problem-reports-with-report-data-cached-on-the-client"></a>Анализ проблем с отчетами при помощи данных отчетов, кэшированных на клиенте  
Если автор создает отчет в Business Intelligence Development Studio, то клиент, создающий отчет, кэширует данные как RDL-файл данных, который используется при просмотре отчета. При каждом изменении запроса кэш обновляется. Для отладки в целях устранения проблем отчета иногда полезно предотвратить обновление данных отчета, чтобы данные не изменялись во время отладки.   
  
Для управления тем, может ли [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull.md)] использовать только кэшированные данные, добавьте следующий раздел в файл devenv.exe.config в [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio.md)]. По умолчанию это каталог `<drive>:Program Files\Microsoft Visual Studio 10.0\Common7\IDE`.   
  
```  
<system.diagnostics>  
      <switches>  
         <add name="Microsoft.ReportDesigner.ReportPreviewStore.ForceCache" value="1" />  
      </switches>  
   </system.diagnostics>  
```  
Если это значение равно 1, используются только кэшированные данные отчета. По окончании отладки отчета не забудьте удалить этот раздел.  
  
## <a name="see-also"></a>См. также:  
[Справочник по ошибкам и событиям (службы Reporting Services)](errors-and-events-reference-reporting-services.md)  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect-md.md)]

---
title: Наблюдение за производительностью сервера отчетов | Документы Майкрософт
description: Узнайте, как наблюдать за производительностью сервера отчетов, чтобы оценивать активность сервера, отслеживать тренды, диагностировать узкие места и собирать данные по конфигурации системы.
ms.date: 02/12/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- report servers [Reporting Services], performance
- counters [Reporting Services]
- monitoring performance [Reporting Services]
- SQL Server Reporting Services, performance
- performance [Reporting Services]
- Reporting Services, performance
ms.assetid: c1bc13d4-8297-4daf-bb19-4c1e5ba292a6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f24b3bb19150271532561d691a1ed1f4c89dfbc5
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838094"
---
# <a name="monitoring-report-server-performance"></a>Наблюдение за производительностью сервера отчетов

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

  Для наблюдения за производительностью сервера отчетов используются средства наблюдения за производительностью, позволяющие оценить активность сервера, наблюдать тренды, диагностировать узкие места системы и собирать данные, помогающие определить адекватность текущей конфигурации системы. Для настройки производительности сервера можно задать частоту очистки домена приложений сервера отчетов. Дополнительные сведения см. в разделе [Настройка доступной памяти для приложений сервера отчетов](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
## <a name="sources-of-performance-data"></a>Источники данных о производительности  
 Для получения всеобъемлющих сведений о работе системы используется комбинация определенных технологий и средств. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows Server предоставляют сведения о производительности с помощью следующих средств:  
  
-   Диспетчер задач  
  
-   Средство просмотра событий  
  
-   Системный монитор  
  
 Диспетчер задач предоставляет сведения о программах и процессах, выполняющихся на компьютере. Диспетчер задач можно использовать для контроля ключевых показателей производительности сервера отчетов. Можно также получить доступ к параметрам активности выполняющихся процессов и просматривать данные и графики использования ЦП и памяти. Дополнительные сведения об использовании диспетчера задач см. в документации по продукту [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 Средство просмотра событий и Монитор производительности можно использовать для создания журналов и предупреждений об обработке отчетов и потреблении ресурсов. Сведения о событиях Windows, формируемых службами [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], см. в разделе [Журнал приложений Windows](../../reporting-services/report-server/windows-application-log.md). Дополнительные сведения о Мониторе производительности см. в разделе "Счетчики производительности Windows" далее в этой статье.  
  
 Служебные программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], такие как [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md) и [Расширенные события](../../relational-databases/extended-events/extended-events.md), предоставляют также сведения о базе данных сервера отчетов и временных базах данных, используемых для кэширования и управления сеансами.  
  
## <a name="windows-performance-counters"></a>Счетчики производительности Windows  
 Контроль конкретных счетчиков производительности позволяет:  
  
-   оценить системные требования, необходимые для поддержки предполагаемой рабочей нагрузки;  
  
-   определить базовый уровень производительности для оценки влияния изменений конфигурации или обновлений приложений;  
  
-   контролировать производительность приложений при определенных нагрузках, естественных или искусственных;  
  
-   убедиться в том, что обновление оборудования оказывает необходимое влияние на производительность;  
  
-   убедиться в том, что изменения конфигурации системы оказывают необходимое влияние на производительность.  

  
## <a name="reporting-services-performance-objects"></a>Объекты производительности служб Reporting Services  
Службы SQL Server 2016 Reporting Services включают следующие объекты производительности.  
  
-   **MSRS 2016 Web Service** и **MSRS 2016 Web Service SharePoint Mode** для наблюдения за производительностью сервера отчетов. Эти объекты производительности включают коллекцию счетчиков, используемых для отслеживания работы сервера отчетов, обычно инициируемой интерактивными операциями просмотра отчетов. Эти счетчики сбрасываются, когда веб-служба сервера отчетов останавливается или перезапускается.  
  
-   **MSRS 2016 Windows Service** и **MSRS 2016 Windows Service SharePoint Mode** для наблюдения за проводимыми по расписанию операциями и доставкой отчетов. Эти объекты производительности включают коллекцию счетчиков, используемых для отслеживания обработки отчетов, обычно инициируемой операциями по расписанию. Назначенные операции включают подписку и доставку, создание снимков состояния выполнения отчетов, а также ведение журнала отчетов.  
  
-   **ReportServer: Service** и **reportserversharepoint: Service** — для наблюдения за связанными с HTTP событиями и управления памятью. Эти счетчики являются уникальными для служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], они отслеживают связанные с HTTP события для сервера отчетов, такие как запросы, соединения и попытки входа. Этот объект производительности также включает счетчики для отслеживания событий управления памятью.  
  
 Если на одном компьютере имеется несколько экземпляров сервера отчетов, их можно контролировать вместе или по отдельности. При добавлении счетчика необходимо выбрать включаемые в него экземпляры. Дополнительные сведения об использовании Монитора производительности (perfmon.msc) и добавлении счетчиков см. в документации по продукту [Монитор производительности [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749249(v=ws.11)).  
  
## <a name="other-performance-counters"></a>Другие счетчики производительности  
 Пользовательские счетчики производительности [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] предоставляются только для перечисленных выше объектов производительности Reporting Services. Указанные ниже счетчики производительности .NET Framework предоставляют дополнительные возможности наблюдения за производительностью сервера отчетов.
 
 > [!NOTE]
 > Для сервера отчетов Power BI и служб SQL Server Reporting Services 2017 и более поздних версий объекты производительности Reporting Services недоступны. Для наблюдения за производительностью сервера отчетов доступны [счетчики производительности .NET Framework](/dotnet/framework/debug-trace-profile/performance-counters). 
 
|Объект производительности|Примечания|  
|------------------------|-----------|  
|**.NET CLR Data** и **.NET CLR Memory**|Веб-портал использует счетчики производительности [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Дополнительные сведения см. в документе по [улучшению производительности и стабильности приложений .NET](https://www.microsoft.com/download/details.aspx?id=11711).|  
|**Процесс**|Для отслеживания времени обработки по идентификатору обработки нужно добавить счетчики производительности **Elapsed Time** и **ID Process** в экземпляр ReportingServicesService.|  
  
## <a name="sharepoint-events"></a>События SharePoint  
 Кроме объектов производительности служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , можно также настроить события SharePoint, если сервер отчетов запущен в режиме интеграции с SharePoint, а среда создания отчетов настроена для использования продукта SharePoint. В данном разделе используйте события для сервера отчетов в режиме интеграции с SharePoint для просмотра диагностических событий компонента, которые могут предоставить полезные сведения, в случае если среда подготовки отчетов интегрирована с SharePoint.  
  
## <a name="in-this-section"></a>В этом разделе  
 [Счетчики производительности для объектов производительности веб-службы MSRS 2016 и службы Windows MSRS 2016 &#40;собственный режим&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)  
 Содержит описание счетчиков производительности, используемых веб-службой сервера отчетов.  
  
 [Счетчики производительности для объектов производительности веб-службы MSRS 2016 и службы Windows MSRS 2016 в режиме интеграции с SharePoint &#40;режим интеграции с SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
 Содержит описание счетчиков производительности, используемых службой Windows сервера отчетов.  
  
 [Счетчики производительности для объектов производительности ReportServer:Service и ReportServerSharePoint:Service](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
 Содержит описание счетчиков производительности в службах [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], связанных с HTTP и с памятью.  
  
## <a name="see-also"></a>См. также раздел  
 [Настройка доступной памяти для приложений сервера отчетов](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md)   
 [Сервер отчетов служб Reporting Services (основной режим)](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
 [Инструментальные средства служб Reporting Services](../../reporting-services/tools/reporting-services-tools.md)  

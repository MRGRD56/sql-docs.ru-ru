---
title: Наблюдение за использованием ресурсов (системный монитор) | Документация Майкрософт
description: Использование системного монитора для определения производительности объектов SQL Server, счетчиков производительности и поведения других объектов, таких как процессоры и память.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- monitoring performance [SQL Server], resource usage
- System Monitor [SQL Server], about Windows System Monitor
- resource usage monitoring [SQL Server]
- System Monitor [SQL Server]
- counters [SQL Server], resource usage subjects
- performance counters [SQL Server], resource usage subjects
- Windows System Monitor [SQL Server], about Windows System Monitor
- monitoring [SQL Server], server resource usage
- monitoring resource usage [SQL Server]
- Windows System Monitor [SQL Server]
- database monitoring [SQL Server], resource usage
- database performance [SQL Server], resource usage
- tuning databases [SQL Server], resource usage
- server performance [SQL Server], resource usage
ms.assetid: f2993a28-0b81-46f2-aec0-6877fe990387
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9ba5319dcc0f7a039e6e37ced535c4600f92136d
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506016"
---
# <a name="monitor-resource-usage-system-monitor"></a>Наблюдение за использованием ресурсов (системный монитор)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В состав серверной операционной системы Microsoft Windows входит графическое средство для измерения производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]— системный монитор. Он позволяет просматривать объекты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , счетчики производительности, а также поведения других объектов, таких как процессоры, память, кэш, потоки и процессы. С каждым из этих объектов связан набор счетчиков, измеряющих степень использования устройства, длину очередей, задержки и другие показатели производительности и внутренней перегрузки.  
  
> [!NOTE]  
>  Этот системный монитор заменяет системный монитор Windows NT 4.0.  
  
## <a name="benefits-of-system-monitor"></a>Преимущества системного монитора  
 Системный монитор может быть полезным для одновременного контроля счетчиков операционной системы Windows и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для определения корреляции между производительностью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и Windows. Например, контроль счетчиков Windows для операций ввода-вывода на диске и одновременно счетчиков диспетчера буферов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может выявить закономерности всей системы.  
  
 Системный монитор позволяет получить статистические данные о текущей активности и производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Применение системного монитора предоставляет следующие возможности.  
  
-   Просматривать данные одновременно на любом числе компьютеров.  
  
-   Просматривать и изменять диаграммы, отражающие текущую активность, отображать значения счетчиков, обновляемых с частотой, которую определяет пользователь.  
  
-   Экспортировать данные из диаграмм, журналов, журналов предупреждений и отчетов в электронные таблицы или приложения баз данных для дальнейшей обработки и печати.  
  
-   Добавлять системные предупреждения, которые вносят событие в журнал предупреждений и уведомляют пользователей, формируя сетевые предупреждения.  
  
-   Запускать предопределенные приложения в первый раз или каждый раз, когда значение счетчика становится выше или ниже заданного значения.  
  
-   Создавать файлы журналов, содержащие данные о различных объектах в разных компьютерах.  
  
-   Присоединять к одному файлу выбранные разделы из других файлов журналов для формирования долговременного архива.  
  
-   Просматривать отчеты о текущей активности или создавать отчеты на основе данных существующих файлов журналов.  
  
-   Сохранять отдельные диаграммы, предупреждения, журналы, параметры отчетов или данные для запуска всего рабочего пространства для повторного использования.  
  
    > [!NOTE]  
    >  В системах позднее Windows NT 4.0 системный монитор заменил монитор производительности. Для выполнения указанных задач можно использовать системный монитор или монитор производительности.  
  
## <a name="system-monitor-performance"></a>Производительность системного монитора  
 При мониторинге [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и операционной системы Microsoft Windows для исследования вопросов, связанных с производительностью, первоначальные усилия должны быть сосредоточены на трех основных областях.  
  
-   Активность работы с диском  
  
-   Использование процессора  
  
-   Использование памяти  
  
 Контроль компьютера, в котором выполняется системный монитор, может слегка влиять на производительность компьютера. Поэтому или регистрируйте данные системного монитора на другом диске (или компьютере), чтобы уменьшить воздействие процедуры контроля на контролируемый компьютер, или выполняйте системный монитор с удаленного компьютера. Контролируйте только те счетчики, которые необходимы. При мониторинге слишком большого числа счетчиков в процессе мониторинга возникают дополнительные затраты ресурсов, что влияет на производительность того компьютера, за которым производится наблюдение.  
  
## <a name="system-monitor-tasks"></a>Задачи системного монитора  
  
|Описание задачи|Раздел|  
|----------------------|-----------|  
|Описываются случаи применения системного монитора и обсуждается снижение производительности при использовании системного монитора.|[Запуск системного монитора](../../relational-databases/performance-monitor/run-system-monitor.md)|  
|Описаны способы отслеживания счетчиков дискового пространства для определения активности диска и количества операций ввода-вывода, создаваемых компонентами SQL Server.|[Отслеживание использования диска](../../relational-databases/performance-monitor/monitor-disk-usage.md)|  
|Описаны способы отслеживания экземпляра Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , чтобы определить, находятся ли уровни загрузки ЦП в стандартных диапазонах.|[Отслеживание использования ЦП](../../relational-databases/performance-monitor/monitor-cpu-usage.md)|  
|Описаны способы мониторинга экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для подтверждения того, что память используется в допустимых пределах.|[Отслеживание использования памяти](../../relational-databases/performance-monitor/monitor-memory-usage.md)|  
|Описаны способы создания предупреждения, которое будет выводиться, когда счетчик системного монитора достигает порогового значения.|[Создание предупреждения для базы данных SQL Server](../../relational-databases/performance-monitor/create-a-sql-server-database-alert.md)|  
|Описаны способы создания диаграмм, предупреждений, журналов и отчетов для отслеживания экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|[Создание диаграмм, предупреждений, журналов и отчетов](../../relational-databases/performance-monitor/create-charts-alerts-logs-and-reports.md)|  
|Приводится список объектов и счетчиков, используемых системным монитором для отслеживания деятельности на компьютерах, где установлены экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|[Использование объектов SQL Server](../../relational-databases/performance-monitor/use-sql-server-objects.md)|  
|Приводится список объектов и счетчиков, используемых системным монитором для отслеживания действий OLTP в памяти.|[Счетчики производительности XTP (In-Memory OLTP) для SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)|  
  
  

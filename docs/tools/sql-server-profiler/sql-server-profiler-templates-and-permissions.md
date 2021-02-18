---
title: Шаблоны и разрешения приложения SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Узнайте, как работает SQL Server Profiler, как использовать его для трассировки событий и где можно найти дополнительные сведения о его возможностях.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6d00378a-5d74-463b-9ed6-a2685306a9d2
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 679b8c0b120b3cea55a2cf8621e5619bc6c282bb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342001"
---
# <a name="sql-server-profiler-templates-and-permissions"></a>Шаблоны и разрешения приложения SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] показывает, как сервер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет внутреннее разрешение запросов. Благодаря этому администраторы могут точно узнать, какие инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] или многомерные выражения передаются на сервер и как он обращается к базе данных или кубу, чтобы сформировать результирующий набор.  
  
 Приложение [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]позволяет:  
  
-   создать трассировку на основе многоразового шаблона;  
  
-   наблюдать результаты трассировки во время ее работы;  
  
-   сохранять результаты трассировки в таблицу;  
  
-   запускать, останавливать, приостанавливать и изменять результаты трассировки по мере необходимости;  
  
-   воспроизводить результаты трассировки.  
  
 Приложение [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] позволяет наблюдать только за нужными событиями. Если трассировки становятся слишком большими, их можно отфильтровать так, чтобы собирались только необходимые сведения. Мониторинг слишком большого потока событий может привести к перегрузке сервера и процесса трассировки, кроме того, файл или таблица трассировки могут вырасти до огромных размеров, особенно если процесс наблюдения ведется в течение долгого периода времени.  
  
> [!NOTE]  
>  Значения столбца трассировки, имеющие размер больше 1 ГБ, вызывают ошибку и усекаются в выводе трассировки.  
  
## <a name="in-this-section"></a>в этом разделе  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Шаблоны приложения SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler-templates.md)|Содержит сведения о предопределенных шаблонах трассировки, которые входят в состав приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Разрешения, необходимые для запуска приложения SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)|Содержит сведения о разрешениях, которые требуются для работы с приложением [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Сохранение трассировок и шаблонов трассировок](../../tools/sql-server-profiler/save-traces-and-trace-templates.md)|Содержит сведения о сохранении результатов трассировки и сохранении определений трассировки в виде шаблона.|  
|[Изменение шаблонов трассировки](../../tools/sql-server-profiler/modify-trace-templates.md)|Содержит сведения об изменении шаблонов трассировки при помощи приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] или инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|[Запуск трассировки](../../tools/sql-server-profiler/start-a-trace.md)|Содержит сведения о том, что происходит при запуске, приостановке или завершении трассировки.|  
|[Сопоставление трассировки с журналом производительности Windows](../../tools/sql-server-profiler/correlate-a-trace-with-windows-performance-log-data.md)|Содержит сведения о проверке корреляции данных журнала производительности Windows с трассировкой при помощи приложения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Просмотр и анализ трассировок с помощью приложения SQL Server Profiler](../../tools/sql-server-profiler/view-and-analyze-traces-with-sql-server-profiler.md)|Содержит сведения об использовании трассировки для устранения неполадок, отображении имен и поиске событий в трассировке.|  
|[Анализ взаимоблокировок в приложении SQL Server Profiler](../../tools/sql-server-profiler/analyze-deadlocks-with-sql-server-profiler.md)|Содержит сведения об использовании приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] для определения причины возникновения взаимоблокировки.|  
|[Анализ запросов с помощью результатов инструкции SHOWPLAN в приложении SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md)|Содержит сведения об использовании приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] для сбора и отображения результатов инструкций Showplan и Showplan Statistics.|  
|[Фильтрация трассировок с помощью приложения SQL Server Profiler](../../tools/sql-server-profiler/filter-traces-with-sql-server-profiler.md)|Содержит сведения о настройке фильтров по столбцам данных для фильтрации результатов трассировки с помощью приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|[Воспроизведение трассировок](../../tools/sql-server-profiler/replay-traces.md)|Содержит описание процесса воспроизведения трассировки и требования, которые к нему предъявляются.|  
  
## <a name="see-also"></a>См. также:  
 [Приложение SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
 [Запуск приложения SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md)  
  
  

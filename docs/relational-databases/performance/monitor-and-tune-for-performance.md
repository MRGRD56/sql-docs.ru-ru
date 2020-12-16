---
title: Мониторинг и настройка производительности | Документация Майкрософт
description: Сведения о мониторинге баз данных для оценки производительности сервера, использовании периодических моментальных снимков и непрерывном сборе данных для отслеживания тенденций производительности.
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- instances of SQL Server, monitoring performance
- monitoring server performance [SQL Server]
- Database Engine [SQL Server], performance
- monitoring performance [SQL Server], about performance
- server performance [SQL Server]
- monitoring performance [SQL Server]
- database performance [SQL Server], about performance
- tuning databases [SQL Server], about performance
- status information [SQL Server], performance monitoring
- database monitoring [SQL Server], about performance
- monitoring [SQL Server], queries performance
- server performance [SQL Server], about performance
- tuning databases [SQL Server]
- database performance [SQL Server]
- monitoring [SQL Server], server performance
- database monitoring [SQL Server]
- monitoring server performance [SQL Server], about monitoring server performance
ms.assetid: 87f23f03-0f19-4b2e-bfae-efa378f7a0d4
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6b8bd69685150772cd6049ed161e34795c7d0b77
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461915"
---
# <a name="monitor-and-tune-for-performance"></a>Наблюдение и настройка производительности
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Наблюдение за базами данных выполняется с целью оценки производительности сервера. Эффективное наблюдение подразумевает регулярное создание моментальных снимков текущей производительности для обнаружения процессов, вызывающих неполадки, и постоянный сбор данных для отслеживания тенденций роста или изменения производительности.  
  
 Постоянная оценка производительности базы данных помогает добиться оптимальной производительности путем минимизации времени ответа и максимального увеличения пропускной способности. Приблизительный сетевой трафик, дисковый ввод-вывод и загрузка ЦП — ключевые факторы, влияющие на производительность. Следует тщательно проанализировать требования приложения, понять логическую и физическую структуру данных, оценить использование базы данных и добиться компромисса между такими конфликтующими нагрузками, как оперативная обработка транзакций (OLTP) и поддержка решений.  
  
## <a name="monitoring-and-tuning-databases-for-performance"></a>Мониторинг и настройка производительности баз данных  
 В состав Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и операционной системы Microsoft Windows входят служебные программы, позволяющие следить за текущим состоянием базы данных и измерять производительность, если это состояние меняется. Для наблюдения за [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно использовать целый ряд средств и методик. Наблюдение за [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет решать следующие задачи:  
  
-   Определять возможности увеличения производительности. Например, выполняя мониторинг времени ответа для часто используемых запросов, можно определить, требуется ли изменить текст запроса или индексы таблицы.  
  
-   Оценивать активность пользователей. Например, выполняя мониторинг пользователей, которые подключаются к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], можно определить, правильно ли настроены параметры безопасности, и проверить работу приложений и систем разработки. Контролируя выполнение SQL-запросов, можно определить, правильно ли они написаны, и проверить результаты, которые они возвращают.  
  
-   Устранять проблемы или отлаживать компоненты приложений, например хранимые процедуры.  
  
## <a name="monitoring-in-a-dynamic-environment"></a>Мониторинг в динамической среде  
Изменение этих условий приведет к изменению производительности. По результатам оценки можно заметить изменения производительности при увеличении числа пользователей, изменении методов доступа пользователей и методов соединения, при увеличении объема содержимого базы данных, изменении клиентского приложения и данных в приложении, а также при усложнении запросов и увеличении объема сетевого трафика. С помощью средств контроля производительности можно связывать изменения отдельных показателей производительности с изменениями условий и сложных запросов. **Примеры:**  
  
-   Отслеживая время отклика на часто используемые запросы, можно определить, нужно ли изменять запросы или индексы опрашиваемых таблиц.  
  
-   Отслеживая выполнение запросов [!INCLUDE[tsql](../../includes/tsql-md.md)] можно определить правильность их написания, а также соответствие ожидаемым результатам.  
  
-   Отслеживая пользователей, пытающихся подключиться к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], можно проверить надежность защиты и протестировать приложения или системы разработки.  
  
Время отклика — это время ожидания возврата пользователю первой строки результирующего набора в форме визуального подтверждения обработки запроса. Пропускная способность — это общее количество запросов, которые сервер может обработать за единицу времени.  
  
С увеличением числа пользователей растет соперничество за ресурсы сервера, что в свою очередь увеличивает время ответа и уменьшает общую пропускную способность.  
  
## <a name="monitoring-and-performance-tuning-tasks"></a>Задачи наблюдения и настройки производительности  
  
|Раздел| Задача|  
|-----------|----------------------|  
|[Мониторинг компонентов SQL Server](../../relational-databases/performance/monitor-sql-server-components.md)|Необходимые действия для мониторинга компонентов SQL Server, такие как монитор активности, расширенные события, динамические административные представления и функции и т. д.|  
|[Средства контроля и настройки производительности](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)|Список средств наблюдения и настройки, доступных в SQL Server, например статистики динамических запросов и помощник по настройке ядра СУБД.|  
|[Обновление баз данных с помощью помощника по настройке запросов](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)|Поддержание стабильной производительности рабочей нагрузки во время обновления до нового уровня совместимости базы данных.|  
|[Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)|Использование хранилища запросов для автоматической регистрации журнала запросов, планов и статистики выполнения и сохранение этих данных для просмотра.|  
|[Формирование базовых показателей производительности](../../relational-databases/performance/establish-a-performance-baseline.md)|Инструкции по формированию базовых показателей производительности.|  
|[Локализация проблем производительности](../../relational-databases/performance/isolate-performance-problems.md)|Локализация проблем производительности базы данных.|  
|[Выявление узких мест](../../relational-databases/performance/identify-bottlenecks.md)|Наблюдение за производительностью сервера и отслеживание его работы для выявления узких мест.|  
|[Использование динамических административных представлений для определения статистики использования и производительности представлений](../../relational-databases/performance/use-dmvs-determine-usage-performance-views.md)|Рассматриваются методы и скрипты, используемые для получения информации о производительности запросов.|  
|[Мониторинг производительности и действий сервера](../../relational-databases/performance/server-performance-and-activity-monitoring.md)|Использование средств наблюдения за производительностью и активностью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и Windows.|  
|[Отслеживание использования ресурсов](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)|Использование системного монитора (также известного как perfmon) для измерения производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью счетчиков производительности.|  

  
## <a name="see-also"></a>См. также раздел  
 [Автоматизация администрирования в масштабах предприятия](../../ssms/agent/automated-administration-across-an-enterprise.md)    
 [Сравнение и анализ планов выполнения](../../relational-databases/performance/compare-and-analyze-execution-plans.md)    
 [Отображение и сохранение планов выполнения](../../relational-databases/performance/display-and-save-execution-plans.md)    
  
  

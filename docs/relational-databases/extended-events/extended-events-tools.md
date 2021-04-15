---
title: Средства расширенных событий
description: Эти средства создают сеансы расширенных событий SQL Server и управляют ими. Помимо сеансов, которые создает пользователь, на сервере существует системный сеанс по умолчанию для сбора данных о работоспособности системы.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- extended events [SQL Server], using
- extended events [SQL Server], options for using
ms.assetid: d312a9ff-50ba-4721-baef-50bfd3169d38
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 338290ac9544c2919502d85ea0c5e4162b1549ac
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490041"
---
# <a name="extended-events-tools"></a>Средства расширенных событий

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Для создания сеансов расширенных событий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и управления ими можно использовать следующие средства:  
  
-   Инструкции языка описания данных DDL. Позволяют создавать и изменять сеанс расширенных событий.  
  
-   Динамическое административное представление, представления каталогов и системные таблицы. Позволяют получать данные и метаданные сеансов с помощью инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] . Системные таблицы помогают определить существующие эквиваленты расширенных событий для классов и столбцов событий трассировки SQL.  
  
-   Узел **Расширенные события** в обозревателе объектов. Позволяет запускать, останавливать и удалять сеансы, а также импортировать и экспортировать шаблоны сеансов.  
  
-   Поставщик [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell. Предоставляет широкий набор функций для создания, изменения сеансов расширенных событий и управления ими. Дополнительные сведения см. в статье [Использование поставщика PowerShell для расширенных событий](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Позволяет создавать и выполнять образцы кода, приведенные в разделах справочника по расширенным событиям. Дополнительные сведения см. в статье [Семантический поиск](../../ssms/object/object-explorer.md).  
  
 Помимо сеансов, которые создает пользователь, на сервере существует системный сеанс по умолчанию для сбора данных о работоспособности системы. В этом сеансе собираются системные данные, которые можно использовать для решения проблем производительности. Дополнительные сведения см. в статье [Использование сеанса system_health](../../relational-databases/extended-events/use-the-system-health-session.md).  
  
## <a name="ddl-statements"></a>Инструкции DDL  
 Следующие инструкции DDL можно использовать для создания, изменения и удаления сеансов расширенных событий.  
  
|Имя|Описание|  
|----------|-----------------|  
|[CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)|Создает объект сеанса расширенных событий, определяющий источник событий, цели и параметры сеанса событий.|  
|[ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)|Запускает или останавливает сеанс событий или изменяет конфигурацию сеанса.|  
|[DROP EVENT SESSION (Transact-SQL)](../../t-sql/statements/drop-event-session-transact-sql.md)|Удаляет сеанс событий.|  
  
## <a name="catalog-views"></a>Представления каталога  
 Следующие представления каталога используются для получения метаданных, сформированных при создании сеанса событий.  
  
|Имя|Описание|  
|----------|-----------------|  
|[sys.server_event_sessions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)|Содержит список определений всех сеансов событий.|  
|[sys.server_event_session_actions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-session-actions-transact-sql.md)|Возвращает строку для каждого действия над каждым событием из сеанса событий.|  
|[sys.server_event_session_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-session-events-transact-sql.md)|Возвращает строку для каждого события в сеансе событий.|  
|[sys.server_event_session_fields (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-session-fields-transact-sql.md)|Возвращает строку для каждого настраиваемого столбца, явно установленного на события и цели.|  
|[sys.server_event_session_targets (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-event-session-targets-transact-sql.md)|Возвращает строку для каждой цели события для сеанса событий.|  
  
## <a name="dynamic-management-views"></a>Динамические административные представления  
 Следующие динамические административные представления используются для получения метаданных и данных сеанса. Метаданные получают из представлений каталога, а данные сеанса создаются при запуске и работе сеанса событий.  
  
> [!NOTE]  
>  Эти представления не содержат данных сеанса до запуска сеанса.  
  
|Имя|Описание|  
|----------|-----------------|  
|[sys.dm_os_dispatcher_pools (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-dispatcher-pools-transact-sql.md)|Возвращает сведения о пулах диспетчера сеансов.|  
|[sys.dm_xe_objects (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)|Возвращает строку для каждого объекта, представленного пакетом событий.|  
|[sys.dm_xe_object_columns (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)|Возвращает сведения о схеме для всех объектов.|  
|[sys.dm_xe_packages (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-packages-transact-sql.md)|Содержит список всех пакетов, зарегистрированных подсистемой расширенных событий.|  
|[sys.dm_xe_sessions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-sessions-transact-sql.md)|Возвращает сведения об активном сеансе расширенных событий.|  
|[sys.dm_xe_session_targets (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)|Возвращает сведения о целевых объектах сеанса.|  
|[sys.dm_xe_session_events (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-events-transact-sql.md)|Возвращает сведения о событиях сеанса.|  
|[sys.dm_xe_session_event_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-event-actions-transact-sql.md)|Возвращает сведения о действиях сеанса событий.|  
|[sys.dm_xe_map_values (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-map-values-transact-sql.md)|Содержит сопоставления внутренних цифровых ключей с понятным текстом.|  
|[sys.dm_xe_session_object_columns (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-object-columns-transact-sql.md)|Отображает значения конфигурации объектов, привязанных к сеансу.|  
  
## <a name="system-tables"></a>Системные таблицы  
 Следующие системные таблицы используются для получения сведений об эквивалентах расширенных событий для классов и столбцов событий трассировки SQL.  
  
|Имя|Описание|  
|----------|-----------------|  
|[trace_xe_event_map (Transact-SQL)](../../relational-databases/system-tables/extended-events-tables-trace-xe-event-map.md)|Содержит одну строку для каждого события из числа расширенных событий, сопоставленного с классом событий трассировки SQL.|  
|[trace_xe_action_map (Transact-SQL)](../../relational-databases/system-tables/extended-events-tables-trace-xe-action-map.md)|Содержит одну строку для каждого действия из числа расширенных событий, сопоставленного с идентификатором столбца трассировки SQL.|  
  
## <a name="see-also"></a>См. также  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Таблицы расширенных событий SQL Server (Transact-SQL)](../system-tables/system-tables-transact-sql.md)   
 [Использование сеанса system_health](../../relational-databases/extended-events/use-the-system-health-session.md)   
 [Использование поставщика PowerShell для расширенных событий](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)  
  

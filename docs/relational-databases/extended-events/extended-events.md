---
title: Обзор XEvents — SQL Server
description: Архитектура расширенных событий SQL Server позволяет собирать данные, необходимые для обнаружения и устранения проблем с производительностью. Она является настраиваемой и масштабируемой.
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: overview
helpviewer_keywords:
- extended events [SQL Server]
- xe
- XEvents
ms.assetid: bf3b98a6-51ed-4f2d-9c26-92f07f1fa947
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d861263a44b7cbf2d13c3e7f46a573b02a575a45
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492429"
---
# <a name="extended-events-overview"></a>Общие сведения о расширенных событиях

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Благодаря архитектуре расширенных событий пользователи могут собирать необходимый объем данных для устранения неполадок или проблем с производительностью. Подсистема расширенных событий допускает настройку и масштабирование в широком диапазоне.

Дополнительные сведения о расширенных событиях см. в разделе [Краткое руководство. Расширенные события в SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).

## <a name="benefits-of-ssnoversion-extended-events"></a>Преимущества системы расширенных событий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Расширенные события — это упрощенная система мониторинга производительности, в которой применяется минимальный объем ресурсов. Система расширенных событий имеет два графических пользовательских интерфейса, которые позволяют создавать, изменять, выводить и анализировать данные сеанса. Эти интерфейсы имеют следующие имена:

- Мастер создания нового сеанса
- Создание сеанса

## <a name="extended-events-concepts"></a>Общие сведения о расширенных событиях  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Подсистема расширенных событий основана на таких основных понятиях, как событие и объект-получатель событий, а также использует понятия трассировки событий Windows (ETW) и вводит несколько новых понятий.  
  
 В следующей таблице даны определения понятий, применяемых в расширенной подсистеме событий.  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Пакеты обработки расширенных событий SQL Server](../../relational-databases/extended-events/sql-server-extended-events-packages.md)|Описывает пакеты расширенных событий, которые содержат объекты. Эти объекты используются для получения и обработки данных во время сеанса расширенных событий.|  
|[Цели расширенных событий SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))|Описывает объекты-получатели событий, получающие данные во время сеанса событий.|  
|[Подсистема расширенных событий SQL Server](../../relational-databases/extended-events/sql-server-extended-events-engine.md)|Описывает подсистему, которая реализует сеанс расширенных событий, а также управляет им.|  
|[Сеансы расширенных событий SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md)|Описывает сеанс расширенных событий.|  
| &nbsp; | &nbsp; |
  
## <a name="extended-events-architecture"></a>Архитектура расширенной подсистемы событий  

Расширенные события представляют собой нашу общую систему обработки событий для серверных систем. Инфраструктура расширенных событий поддерживает корреляцию данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а при определенных условиях — корреляцию данных из операционной системы и приложений баз данных. В случае с операционной системой выходные данные расширенных событий могут направляться в службу трассировки событий Windows (ETW). Служба ETW может обеспечивать сопоставление данных событий с данными событий операционной системы или приложений.  

Все приложения имеют точки выполнения, которые с успехом используются как внутри приложения, так и вне его. Внутри приложения можно поставить асинхронную обработку в очередь, используя сведения, собранные во время начального выполнения задачи. Вне приложения средства мониторинга получают данные из точек выполнения. Это информация о характеристиках поведения и производительности отслеживаемого приложения.  

 Расширенные события поддерживают использование данных событий вне процесса. Эти данные используются, как правило, следующими средствами.  
  
-   Средствами трассировки, например приложением SQL Trace и системным монитором.  
  
-   Инструментами ведения журналов, например журналом событий Windows или журналом ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Пользователями, выполняющими администрирование продукта или разрабатывающими приложения для него.  
  
 Ключевые аспекты расширенных событий:  
  
-   Подсистема расширенных событий не зависит от событий. Подсистема может привязать любое событие к любой цели, так как не ограничена содержимым события. Дополнительные сведения о подсистеме расширенных событий см. в разделе [SQL Server Extended Events Engine](../../relational-databases/extended-events/sql-server-extended-events-engine.md).  
  
-   События отделены от объектов-получателей событий, называемых в расширенных событиях *целями* . Это означает, что любая цель может получать любые события. Кроме того, любое возникающее событие может автоматически потребляться целью, которая регистрирует его и предоставляет дополнительный контекст событий. Дополнительные сведения см. в статье [SQL Server Extended Events Targets](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).  
  
-   События отделены от действий, предпринимаемых при возникновении события. Следовательно, любое действие можно связать с любым событием.  
  
-   Предикаты могут применять динамические фильтры, когда необходимо получить данные события. Динамическая фильтрация делает инфраструктуру расширенных событий более гибкой. Дополнительные сведения см. в разделе [SQL Server Extended Events Packages](../../relational-databases/extended-events/sql-server-extended-events-packages.md).  
  
 Расширенные события могут синхронно создавать данные событий (и асинхронно обрабатывать эти данные), чем обеспечивается гибкое решение для обработки событий. Кроме того, расширенные события предоставляют следующие возможности.  
  
-   Унифицированный подход к обработке событий во всей серверной системе, одновременно разрешающий пользователям изолировать отдельные события для устранения неполадок.  
  
-   Интеграция и поддержка существующих инструментов приложения трассировки событий Windows.  
  
-   Полностью настраиваемый механизм обработки событий на основе языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Возможность динамического мониторинга активных процессов с минимальным влиянием на эти процессы.  
  
-   Сеанс работоспособности системы по умолчанию, который выполняется без каких-либо заметных последствий для производительности. В этом сеансе собираются системные данные, которые можно использовать для решения проблем производительности. Дополнительные сведения см. в статье [Использование сеанса system_health](../../relational-databases/extended-events/use-the-system-health-session.md).  
  
## <a name="extended-events-tasks"></a>Задачи расширенной подсистемы событий  

Использование среды [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] или языка [!INCLUDE[tsql](../../includes/tsql-md.md)] для выполнения инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] языка описания данных DDL, динамических административных представлений и функций или представлений каталога дает возможность создавать простые и комплексные решения по диагностике расширенных событий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Описание задачи|Раздел|  
|----------------------|-----------|  
|**Обозреватель объектов** позволяет управлять сеансами события.|[Управление сеансами событий в обозревателе объектов](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)|  
|Описано, как создать сеанс расширенных событий.|[Создание сеанса расширенных событий](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130))|  
|Описывает, как просматривать и обновлять целевые данные.| [Расширенный просмотр целевых данных из расширенных событий в SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)|  
|Описывает использование средств расширенной подсистемы событий для создания сеансов и управления сеансами расширенных событий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Средства расширенных событий](../../relational-databases/extended-events/extended-events-tools.md)|  
|Описано, как изменить сеанс расширенных событий.|[Изменение сеанса расширенных событий](../../relational-databases/extended-events/alter-an-extended-events-session.md)|  
|Описывает, как получить информацию о полях, связанных с событиями.|[Получение полей для всех событий](/previous-versions/sql/sql-server-2016/bb677249(v=sql.130))|  
|Описывает, как узнать, какие события доступны в зарегистрированных пакетах.|[Просмотр событий для зарегистрированных пакетов](./selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)|  
|Описывает, как определить, какие цели расширенных событий доступны в зарегистрированных пакетах.|[Просмотр целей расширенных событий для зарегистрированных пакетов](/previous-versions/sql/sql-server-2016/bb677247(v=sql.130))|  
|Описывает, как с помощью приведенной ниже процедуры можно просматривать события и действия расширенных событий, аналогичных каждому событию трассировки SQL со связанными столбцами.|[Просмотр эквивалентов расширенных событий для классов событий трассировки SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)|  
|Описывает, как выяснить, какие параметры можно задать при использовании аргумента ADD TARGET в инструкции CREATE EVENT SESSION или ALTER EVENT SESSION.|[Получение настраиваемых параметров для аргумента ADD TARGET](/previous-versions/sql/sql-server-2016/bb677176(v=sql.130))|  
|Описано, как преобразовать существующий скрипт приложения трассировки SQL в сеанс расширенных событий.|[Преобразование существующего скрипта трассировки SQL в сеанс расширенных событий](../../relational-databases/extended-events/convert-an-existing-sql-trace-script-to-an-extended-events-session.md)|  
|Описывает, как определить, какие запросы удерживают данную блокировку и каковы план запроса и стек [!INCLUDE[tsql](../../includes/tsql-md.md)] во время получения блокировки.|[Определение запросов, содержащих блокировки](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)|  
|Описывает, как определить источник блокировок, приводящих к ухудшению производительности базы данных.|[Поиск объектов, на которые наложено наибольшее число блокировок](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)|  
|Описывает, как использовать расширенные события совместно со средством трассировки событий для Windows (ETW) для наблюдения за активностью системы.|[Мониторинг активности системы с помощью расширенных событий](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)|  
|Использование представлений каталога и динамических административных представлений для расширенных событий | [Использование SELECT и JOIN в системных представлениях для расширенных событий в SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) |
| &nbsp; | &nbsp; |

Выполните следующий запрос Transact-SQL (T-SQL), чтобы вывести список всех возможных расширенных событий с их описанием:

```sql
SELECT
     obj1.name as [XEvent-name],
     col2.name as [XEvent-column],
     obj1.description as [Descr-name],
     col2.description as [Descr-column]
  FROM
               sys.dm_xe_objects        as obj1
      JOIN sys.dm_xe_object_columns as col2 on col2.object_name = obj1.name
  ORDER BY
    obj1.name,
    col2.name
```


## <a name="code-examples-can-differ-for-azure-sql-database"></a>Примеры кода для базы данных SQL Azure могут отличаться

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>См. также:

[Приложения уровня данных](../../relational-databases/data-tier-applications/data-tier-applications.md)  
[Поддержка приложений уровня данных для объектов и версий SQL Server](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))  
[Развертывание приложения уровня данных](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)  
[Наблюдение за приложениями уровня данных](../../relational-databases/data-tier-applications/monitor-data-tier-applications.md)  
&nbsp;  
[Динамические административные представления расширенных событий](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)  
[Представления каталога расширенных событий (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)  
&nbsp;  
[XELite: кроссплатформенная библиотека для чтения событий XEvent из XEL-файлов или обновляющихся потоков SQL](https://www.nuget.org/packages/Microsoft.SqlServer.XEvent.XELite/), выпущена в мае 2019 г.  
Командлет [Read-SQLXEvent PowerShell](https://www.powershellgallery.com/packages/SqlServer.XEvent), выпущен в июне 2019 г.  
[Скрытые возможности SQL. Сравнение отслеживания причинных связей и последовательностей событий для сеансов XEvent (запись блога опубликована 1 апреля 2019 г.)](https://bobsql.com/sql-mysteries-causality-tracking-vs-event-sequence-for-xevent-sessions/)
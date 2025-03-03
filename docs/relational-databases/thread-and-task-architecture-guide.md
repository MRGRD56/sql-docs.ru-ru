---
title: Руководство по архитектуре потоков и задач | Документация Майкрософт
description: Сведения об архитектуре потоков и задач в SQL Server, в том числе о планировании задач, горячем добавлении ЦП, а также рекомендации по использованию компьютеров с более чем 64 ЦП.
ms.custom: ''
ms.date: 09/23/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, thread and task architecture
- thread and task architecture guide
- task scheduling
- working threads
- Large Deficit First scheduling
- LDF scheduling
- scheduling, SQL Server
- tasks, SQL Server
- threads, SQL Server
- quantum, SQL Server
ms.assetid: 925b42e0-c5ea-4829-8ece-a53c6cddad3b
author: pmasl
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ec3a20e10fd625b588c4bae79f73265a8c292324
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793123"
---
# <a name="thread-and-task-architecture-guide"></a>руководство по архитектуре потоков и задач
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

## <a name="operating-system-task-scheduling"></a>Планирование задач операционной системы
Потоки — это наименьшие единицы обработки, которые могут быть выполнены операционной системой. Они позволяют разделять логику приложения на несколько параллельных путей выполнения. Потоки полезны, когда сложные приложения имеют много задач, которые могут выполняться одновременно. 

Когда операционная система выполняет экземпляр приложения, она создает модуль, называемый процессом, для управления экземпляром. Процесс имеет поток выполнения. Это ряд программных команд, выполненных кодом приложения. Например, если простое приложение содержит один набор инструкций, которые можно выполнить последовательно, этот набор инструкций обрабатывается как одна **задача** и существует только один путь выполнения (или **поток**) приложения. Более сложные приложения могут иметь несколько **задач**, которые могут выполняться одновременно, а не последовательно. Для этого приложение может запустить для каждой задачи отдельные процессы, которые являются ресурсоемкими, или запустить отдельные потоки, которые являются относительно менее ресурсоемкими. Кроме этого, каждый поток можно запланировать для выполнения независимо от других потоков, связанных с процессом.

Потоки позволяют сложным приложениям более эффективно использовать ЦП, даже на компьютерах с одним ЦП. С одним ЦП только один поток может выполняться одновременно. Если один поток выполняет длительную операцию, которая не использует ЦП, например считывание с диска или запись на диск, другой поток может выполняться, пока первая операция не завершится. Возможность выполнять потоки, в то время как другие потоки ожидают завершения операции, позволяет приложению увеличить использование ЦП. Это особенно касается многопользовательских приложений, интенсивно использующих операции дискового ввода-вывода, например сервера базы данных. Компьютеры с несколькими ЦП могут одновременно выполнять один поток для каждого ЦП. Например, если компьютер имеет восемь ЦП, он может выполнять одновременно восемь потоков.

## <a name="sql-server-task-scheduling"></a>Планирование задач SQL Server
В области [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]**запрос** является логическим представлением запроса или пакета. Запрос также представляет операции, необходимые системным потокам, таким как контрольная точка или модуль записи журнала. Запросы находятся в различных состояниях в течение всего времени существования и могут накапливать время ожидания, когда ресурсы, необходимые для выполнения запроса, недоступны, например присутствуют [блокировки](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md#locks) или [кратковременные блокировки](../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md#latches). Дополнительные сведения о состояниях запросов см. в статье [sys.dm_exec_requests (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).

**Задача** представляет единицу работы, которую необходимо завершить для выполнения запроса. Одному запросу можно назначить одну или несколько задач. 
-  В параллельных запросах существует несколько активных задач, которые выполняются не последовательно, а параллельно, с одной **родительской** (или координирующей) задачей и несколькими **дочерними**. План выполнения для параллельного запроса может иметь последовательные ветви — области плана с операторами, которые не выполняются параллельно. Родительская задача отвечает и за выполнение этих последовательных операторов.
-  В последовательных запросах в каждый момент времени выполнения будет существовать только одна активная задача.     
Задачи находятся в различных состояниях в течение всего времени существования. Дополнительные сведения о состояниях задач см. в статье [sys.dm_os_tasks (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Задачи в приостановленном состоянии ожидают ресурсов, необходимых для выполнения задачи. До того времени они недоступны. Дополнительные сведения об ожидающих задачах см. в разделе [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md).

**Рабочий поток** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], также известный как рабочая роль или поток, является логическим представлением потока операционной системы. При выполнении **последовательных запросов** [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] порождает рабочую роль для выполнения активной задачи (1:1). При выполнении **параллельных запросов** в [построчном режиме](../relational-databases/query-processing-architecture-guide.md#execution-modes) [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] назначает рабочую роль для координации дочерних рабочих ролей, ответственных за выполнение назначенных им задач (также 1:1), которая называется **родительским потоком** (или координирующим потоком). Родительский поток имеет связанную с ним родительскую задачу. Родительский поток — это точка входа запроса, и она существует даже до того, как ядро начало анализировать запрос. Основные задачи родительского потока: 
-  Координирование параллельного сканирования.
-  Запуск дочерних параллельных рабочих ролей.
-  Сбор строк из параллельных потоков и их отправка клиенту.
-  Выполнение локальных и глобальных операций агрегирования.    

> [!NOTE]
> Если план запроса содержит последовательные и параллельные ветви, то одна из параллельных задач будет отвечать за выполнение последовательной ветви. 

Количество рабочих потоков, порожденных для каждой задачи, зависит от следующих факторов:
-   Имеет ли запрос право на параллелизм, что определяется оптимизатором запросов.
-   Какова действительная доступная [степень параллелизма (DOP)](../relational-databases/query-processing-architecture-guide.md#DOP) в системе на основе текущей нагрузки. Она может отличаться от предполагаемой степени параллелизма, оценка которой выполняется на основе конфигурации сервера для максимальной степени параллелизма (MAXDOP). Например, конфигурация сервера для MAXDOP может составлять 8, но доступное значение DOP в среде выполнения может составлять только 2, что влияет на производительность запросов. 

> [!NOTE]
> Ограничение параметра **max degree of parallelism (MAXDOP)** задается для каждой задачи, а не для каждого запроса. Это означает, что при выполнении параллельных запросов один запрос может порождать несколько задач вплоть до ограничения MAXDOP и каждая задача будет использовать одну рабочую роль. Дополнительные сведения о MAXDOP см. в статье [Настройка параметра конфигурации сервера max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

**Планировщик**, также известный как планировщик SOS, управляет рабочими потоками, требующими времени обработки для выполнения работы от имени задач. Каждый планировщик сопоставляется с отдельным процессором (ЦП). Время, в течение которого рабочая роль может оставаться активной в планировщике, называется квантом времени операционной системы (не более 4 мс). По истечении кванта времени рабочая роль передает время другим рабочим ролям, которым требуется доступ к ресурсам ЦП, и изменяет свое состояние. Это сотрудничество между рабочими ролями для повышения уровня доступа к ресурсам ЦП называется **совместное планирование**, также известное как планирование в режиме без вытеснения. В свою очередь, изменение состояния рабочей роли распространяется на задачу, связанную с этой рабочей ролью, и на запрос, связанный с задачей. Дополнительные сведения о состояниях рабочих ролей см. в статье [sys.dm_os_workers (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md). Дополнительные сведения о планировщиках см. в статье [sys.dm_os_schedulers (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 

Таким образом, **запрос** может порождать одну или несколько **задач** для выполнения единиц работы. Каждая задача назначается **рабочему потоку**, который отвечает за выполнение этой задачи. Каждый рабочий поток должен быть запланирован (помещен в **планировщик**) для активного выполнения задачи. 

> [!NOTE]
> Рассмотрим следующий сценарий.   
> -  Рабочая роль 1 — это длительно выполняемая задача, например запрос на чтение с упреждающим чтением таблиц на основе дисков. Рабочая роль 1 находит необходимые страницы данных в буферном пуле, поэтому она не должна ждать операций ввода-вывода и может использовать полный квант времени до приостановки.   
> -  Рабочая роль 2 выполняет более кратковременные задачи на уровне долей миллисекунд и поэтому должна ожидать исчерпания своего полного кванта времени.     
>
> В этом сценарии и до [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]рабочей роли 1 разрешено монопольно использовать планировщик за счет большего общего времени квантов.   
> Начиная с [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], совместное планирование включает в себя планирование по методу Large Deficit First (LDF, преимущество при дефиците). Планирование LDF позволяет отслеживать модели использования квантов, при этом планировщик не используется монопольно ни одним рабочим потоком. В этом случае рабочей роли 2 разрешается использовать квант повторно до того, как рабочей роли 1 будет разрешено использовать дополнительный квант, поэтому рабочая роль 1 не сможет монопольно использовать планировщик в недружественной модели.

### <a name="scheduling-parallel-tasks"></a>Планирование параллельных задач
Представьте, что [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] настроен с MaxDOP 8 и сходство ЦП настроено для 24 ЦП (планировщиков) в узлах NUMA 0 и 1. Планировщики с 0 по 11 относятся к узлу NUMA 0, планировщики с 12 по 23 относятся к узлу NUMA 1. Приложение отправляет в [!INCLUDE[ssde_md](../includes/ssde_md.md)] следующий запрос:

```sql
SELECT h.SalesOrderID, h.OrderDate, h.DueDate, h.ShipDate
FROM Sales.SalesOrderHeaderBulk AS h 
INNER JOIN Sales.SalesOrderDetailBulk AS d ON h.SalesOrderID = d.SalesOrderID 
WHERE (h.OrderDate >= '2014-3-28 00:00:00');
```

> [!TIP]
> Этот пример запроса можно выполнить с помощью [примера базы данных AdventureWorks2016_EXT](../samples/adventureworks-install-configure.md). Таблицы `Sales.SalesOrderHeader` и `Sales.SalesOrderDetail` увеличены 50 раз и переименованы в `Sales.SalesOrderHeaderBulk` и `Sales.SalesOrderDetailBulk`.

План выполнения показывает [хэш-соединение](../relational-databases/performance/joins.md#hash) между двумя таблицами и каждый из операторов, выполняемый параллельно, что указывается желтым кругом с двумя стрелками. Каждый оператор параллелизма представляет собой отдельную ветвь в плане. Таким образом, в следующем плане выполнения есть три ветви. 

![План параллельного запроса](../relational-databases/media/schedule-parallel-query-plan.png)

> [!NOTE]
> Если представить план выполнения как дерево, то **ветвь** — это область плана, которая группирует один или несколько операторов между операторами параллелизма, также называемыми итераторами обмена. Дополнительные сведения об операторах плана см. в [справочнике логических и физических операторов Showplan](../relational-databases/showplan-logical-and-physical-operators-reference.md). 

Хотя в этом плане выполнения есть три ветви, в любой момент выполнения в этом плане могут одновременно выполняться только две ветви.
1.  Ветвь, в которой *сканирование кластеризованного индекса* используется в `Sales.SalesOrderHeaderBulk` (во входных данных сборки соединения), выполняется отдельно.
2.  Ветвь, в которой *сканирование кластеризованного индекса* используется в `Sales.SalesOrderDetailBulk` (во входных данных пробы соединения), выполняется параллельно с ветвью, в которой был создан объект *Bitmap* и сейчас выполняется оператор *Hash Match*.

Showplan XML показывает, что в узле NUMA 0 было зарезервировано и использовано 16 рабочих потоков:

```xml
<ThreadStat Branches="2" UsedThreads="16">
  <ThreadReservation NodeId="0" ReservedThreads="16" />
</ThreadStat>
```

Резервирование потоков гарантирует, что [!INCLUDE[ssde_md](../includes/ssde_md.md)] будет иметь достаточно рабочих потоков для выполнения всех задач, которые потребуются для запроса. Потоки могут быть зарезервированы на нескольких узлах NUMA или только на одном. Резервирование потока происходит во время выполнения до начала выполнения и зависит от нагрузки планировщика. Количество зарезервированных рабочих потоков в целом выводится из формулы ***параллельные ветви* * *DOP времени выполнения***, и в нем не учитывается родительский рабочий поток. Количество рабочих потоков в каждой ветви ограничено значением MaxDOP. В этом примере существуют две параллельные ветви, а для MaxDOP установлено значение 8, поэтому количество рабочих потоков **2 * 8 = 16**.

Для справки просмотрите динамический план выполнения из [динамической статистики запросов](../relational-databases/performance/live-query-statistics.md), где одна ветвь завершила выполнение и две ветви выполняются параллельно.

![Динамический план параллельного запроса](../relational-databases/media/schedule-parallel-query-live-plan.png)

[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] назначит рабочему потоку выполнение активной задачи (1:1), что можно наблюдать во время выполнения запроса, запросив динамическое административное представление [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md), как показано в следующем примере.

```sql
SELECT parent_task_address, task_address, 
       task_state, scheduler_id, worker_address
FROM sys.dm_os_tasks
WHERE session_id = <insert_session_id>
ORDER BY parent_task_address, scheduler_id;
```

> [!TIP]
> Столбец `parent_task_address` всегда имеет значение NULL для родительской задачи. 

> [!TIP]
> В очень загруженном [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] можно увидеть количество активных задач, превышающее ограничение, установленное зарезервированными потоками. Эти задачи могут относиться к ветви, которая больше не используется и находится в промежуточном состоянии, ожидая очистки. 

[!INCLUDE[ssResult](../includes/ssresult-md.md)] Обратите внимание, что для выполняемых в настоящий момент ветвей существует 17 активных задач: 16 дочерних задач, соответствующих зарезервированным потокам, и одна родительская, или координирующая, задача.

|parent_task_address|task_address|task_state|scheduler_id|worker_address|
|--------|--------|--------|--------|--------|
|NULL|**0x000001EF4758ACA8**|SUSPENDED|3|0x000001EFE6CB6160|
|0x000001EF4758ACA8|0x000001EFE43F3468|SUSPENDED|0|0x000001EF6DB70160|
|0x000001EF4758ACA8|0x000001EEB243A4E8|SUSPENDED|0|0x000001EF6DB7A160|
|0x000001EF4758ACA8|0x000001EC86251468|SUSPENDED|5|0x000001EEC05E8160|
|0x000001EF4758ACA8|0x000001EFE3023468|SUSPENDED|5|0x000001EF6B46A160|
|0x000001EF4758ACA8|0x000001EFE3AF1468|SUSPENDED|6|0x000001EF6BD38160|
|0x000001EF4758ACA8|0x000001EFE4AFCCA8|SUSPENDED|6|0x000001EF6ACB4160|
|0x000001EF4758ACA8|0x000001EFDE043848|SUSPENDED|7|0x000001EEA18C2160|
|0x000001EF4758ACA8|0x000001EF69038108|SUSPENDED|7|0x000001EF6AEBA160|
|0x000001EF4758ACA8|0x000001EFCFDD8CA8|SUSPENDED|8|0x000001EFCB6F0160|
|0x000001EF4758ACA8|0x000001EFCFDD88C8|SUSPENDED|8|0x000001EF6DC46160|
|0x000001EF4758ACA8|0x000001EFBCC54108|SUSPENDED|9|0x000001EFCB886160|
|0x000001EF4758ACA8|0x000001EC86279468|SUSPENDED|9|0x000001EF6DE08160|
|0x000001EF4758ACA8|0x000001EFDE901848|SUSPENDED|10|0x000001EFF56E0160|
|0x000001EF4758ACA8|0x000001EF6DB32108|SUSPENDED|10|0x000001EFCC3D0160|
|0x000001EF4758ACA8|0x000001EC8628D468|SUSPENDED|11|0x000001EFBFA4A160|
|0x000001EF4758ACA8|0x000001EFBD3A1C28|SUSPENDED|11|0x000001EF6BD72160|

Обратите внимание, что всем 16 дочерним задачам назначены разные рабочие потоки (их можно увидеть в столбце `worker_address`), но все рабочие роли назначены одному и тому же пулу из восьми планировщиков (0, 5, 6, 7, 8, 9, 10, 11), а родительская задача назначена планировщику, не входящему в этот пул (3).

> [!IMPORTANT]
> После планирования первого набора параллельных задач в конкретной ветви [!INCLUDE[ssde_md](../includes/ssde_md.md)] будет использовать тот же пул планировщиков для выполнения дополнительных задач в других ветвях. Это означает, что один и тот же набор планировщиков будет использоваться для всех параллельных задач в рамках всего плана выполнения, ограниченных только значением MaxDOP.  
> [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] всегда пытается назначить планировщики из одного и того же узла NUMA для выполнения задач и назначать их последовательно (при циклическом переборе), если планировщики доступны. Однако рабочий поток, назначенный родительской задаче, может быть помещен в узел NUMA, отличный от используемого для других задач.

Рабочий поток может оставаться активным в планировщике только в течение его кванта времени (4 мс); после истечения этого кванта времени он должен уступить планировщик, чтобы мог стать активным рабочий поток, назначенный другой задаче. Когда квант времени истекает и рабочий поток становится неактивным, соответствующая задача помещается в очередь FIFO в состоянии RUNNABLE до тех пор, пока рабочий поток снова не перейдет в состояние RUNNING, при условии что задаче не потребуется доступ к ресурсам, которые на этот момент недоступны, например из-за блокировки. В этом случае задача будет переведена в состояние SUSPENDED вместо RUNNABLE до тех пор, пока эти ресурсы не станут доступны.  

> [!TIP] 
> Что касается выходных данных динамического административного представления, показанного выше, все активные задачи находятся в состоянии SUSPENDED. Дополнительные сведения об ожидающих задачах можно получить, выполнив запрос динамического административного представления [sys.dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md). 

В общей сложности параллельный запрос породит несколько задач. Каждая задача должна быть назначена одному рабочему потоку. Каждый рабочий поток должен быть назначен одному планировщику. Следовательно, количество используемых планировщиков не может превышать количество параллельных задач для каждой ветви, устанавливаемое конфигурацией MaxDOP или указанием запроса. Координирующий поток не влияет на ограничение MaxDOP. 

### <a name="allocating-threads-to-a-cpu"></a>Распределение потоков ЦП
По умолчанию каждый экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] запускает один поток. Операционная система распределяет потоки экземпляров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] поровну между ЦП компьютера в зависимости от загрузки. Если сходство процессов включено на уровне операционной системы, операционная система назначает каждый поток конкретному ЦП. Компонент [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], в свою очередь, назначает **рабочие потоки** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] **планировщикам**, которые равномерно распределяют потоки между ЦП по принципу циклического перебора.
    
Для обеспечения многозадачности, например, когда несколько приложений используют один и тот же набор ЦП, операционная система иногда распределяет рабочие потоки между разными ЦП. Безусловно, такая организация работы эффективна с точки зрения операционной системы, но может привести к снижению производительности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] при больших нагрузках системы, поскольку в кэше каждого процессора неоднократно перезагружаются данные. В этих условиях назначение ЦП определенных потоков может повысить производительность, устраняя повторную загрузку процессоров и уменьшая количество переносов потоков между ЦП (а значит, уменьшая число переключений контекста). Такая связь между потоком и процессором называется соответствием процессоров. Если функция подобия была включена, операционная система назначает поток конкретному ЦП. 

[Параметр affinity mask](../database-engine/configure-windows/affinity-mask-server-configuration-option.md) задается с помощью инструкции [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md). Если параметр affinity mask не задан, экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] распределяет рабочие потоки равномерно между планировщиками, которые не были исключены.

> [!CAUTION]
> Не используйте маску сходства процессоров в операционной системе и маску сходства в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]одновременно. Эти настройки предназначены для достижения одного результата, и если их значения будут несогласованными, результат может быть непредсказуем. Дополнительные сведения см. в разделе [Параметр affinity mask](../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

Пул потоков помогает оптимизировать производительность при подключении к серверу большого числа пользователей. Обычно для каждого запроса в операционной системе создается отдельный поток. Однако в случае сотен соединений с сервером, использование одного потока на каждый запрос приводит к потреблению большого числа системных ресурсов. [Параметр max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) позволяет [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] создавать пул рабочих потоков, чтобы обслужить большое число запросов, что улучшает производительность. 

### <a name="using-the-lightweight-pooling-option"></a>параметр «использование упрощенных пулов»
Нагрузка, возникающая при контекстном переключении потоков, может быть не очень высока. Для большинства экземпляров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] установка параметра "использование упрощенных пулов" в значение 0 или 1 не вызовет заметного изменения производительности. Выиграть от установки параметра [использование упрощенных пулов](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) могут только те экземпляры [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], которые выполняются там, где:    
* установлен производительный многопроцессорный сервер;
* все процессоры работают почти на пределе производительности;
* высок уровень переключения контекста.

В таких системах может наблюдаться небольшое повышение эффективности, если для параметра "Использование упрощенных пулов" установлено значение 1.

> [!IMPORTANT]
> Не используйте планирование в режиме волокон для выполнения распространенных операций. Это может привести к снижению производительности, мешая нормальной работе переключения контекста. Кроме того, некоторые компоненты [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не могут работать правильно в режиме волокон. Дополнительные сведения см. в разделе об [использовании упрощенных пулов](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

## <a name="thread-and-fiber-execution"></a>Выполнение потоков и волокон
В Microsoft Windows используется числовая система приоритетов, распределяющая выполняемые потоки по уровням от 1 до 31. Значение 0 зарезервировано для использования операционной системой. При наличии в очереди на выполнение нескольких потоков Windows сначала обслуживает поток с наивысшим приоритетом.

Каждый экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] по умолчанию имеет уровень приоритета 7, что соответствует стандартному уровню. Это значение по умолчанию задает потокам [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] достаточно высокий уровень приоритета, позволяющий им получать достаточно ресурсов ЦП, не снижая производительности других приложений. 

> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../includes/ssnotedepfuturedontuse-md.md)]  

Параметр конфигурации [priority boost](../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) используется для повышения приоритета потоков экземпляра [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] до уровня 13. Это наивысший приоритет. Этот параметр назначает потокам [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] более высокий приоритет по отношению к другим приложениям. Таким образом, потоки [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] всегда будут обслуживаться в первую очередь, а потоки других приложений не смогут занимать ресурсы системы ранее потоков с более высоким приоритетом. Представленные выше меры могут повысить производительность сервера, на котором выполняются только экземпляры [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Однако если в приложении [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] выполняется ресурсоемкая операция, то не рекомендуется присваивать высокий приоритет другим приложениям, чтобы они не вытесняли поток [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

Если на компьютере выполняется несколько экземпляров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], то повышение приоритетов одних может привести к снижению производительности других. Кроме того, включение параметра priority boost может привести к простою других приложений и компонентов, выполняемых на компьютере. Таким образом, данный параметр рекомендуется использовать только в строго определенных условиях.


## <a name="hot-add-cpu"></a>ЦП с поддержкой горячей замены
ЦП с поддержкой горячей замены — это возможность динамически добавлять центральные процессоры в запущенную систему. Добавление центральных процессоров может осуществляться физически — путем добавления нового оборудования, логически — путем аппаратного секционирования в сети, виртуально — через уровень виртуализации. Начиная с [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] поддерживает ЦП с поддержкой горячей замены.

Требования для ЦП с поддержкой горячей замены:  
* оборудование с поддержкой ЦП с поддержкой горячей замены;
* 64-разрядный выпуск Windows Server 2008 Datacenter или Windows Server 2008 Enterprise Edition для операционных систем на платформе Itanium;
* [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise.
* [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не может быть настроен на использование программной архитектуры NUMA. Дополнительные сведения о программной архитектуре NUMA см. в разделе [Архитектура Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md).

После добавления центральных процессоров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не начинает автоматически использовать их. Это предотвращает использование [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] центральных процессоров, добавленных для каких-либо других целей. После добавления ЦП выполните инструкцию [RECONFIGURE](../t-sql/language-elements/reconfigure-transact-sql.md), что позволит [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] распознать новые ЦП в качестве доступных ресурсов.

> [!NOTE]
> Для использования новых ЦП необходимо изменить настройки параметра [affinity64 mask](../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) .
 
## <a name="best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus"></a>Рекомендации по использованию SQL Server на компьютерах, которые имеют более 64 процессоров

### <a name="assigning-hardware-threads-with-cpus"></a>Назначение аппаратных потоков процессорам
Не используйте параметры конфигурации сервера affinity mask и affinity64 mask для привязки процессоров к определенным потокам. Эти параметры ограничены 64 процессорами. Вместо этого следует использовать параметр `SET PROCESS AFFINITY` инструкции [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md).

### <a name="managing-the-transaction-log-file-size"></a>Управление размером файла журнала транзакций
Не следует полагаться на функцию автоувеличения для увеличения размера файла журнала транзакций. Увеличение журнала транзакций должно происходить последовательно. Увеличение журнала может помешать выполнению операций записи транзакций до тех пор, пока увеличение журнала не будет завершено. Вместо этого заранее выделите место для файлов журнала, установив размер файла в значение, достаточное для поддержки обычной рабочей нагрузки в среде.

### <a name="setting-max-degree-of-parallelism-for-index-operations"></a>Установка максимальной степени параллелизма для операций с индексами
На компьютерах, имеющих несколько процессоров, с помощью временного изменения модели восстановления базы данных на модель восстановления с неполным протоколированием или простую модель восстановления можно улучшить производительность операций с индексами, таких как создание или перестроение индексов. Операции с индексами могут создавать значительную нагрузку на журнал, и конфликт журнала может повлиять на выбранную [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] наилучшую степень параллелизма.

Помимо настройки параметра конфигурации сервера **max degree of parallelism (MAXDOP)** , рассмотрите возможность настройки параллелизма для операций с индексами с помощью [параметра MAXDOP](../t-sql/statements/alter-index-transact-sql.md). Дополнительные сведения см. в статье [Настройка параллельных операций с индексами](../relational-databases/indexes/configure-parallel-index-operations.md). Дополнительные сведения и рекомендации по настройке параметра конфигурации сервера max degree of parallelism см. в статье [Настройка параметра конфигурации сервера max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

### <a name="setting-the-maximum-number-of-worker-threads"></a>Задание максимального числа рабочих потоков
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] динамически настраивает параметр конфигурации сервера **max worker threads** при запуске. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] использует количество доступных ЦП и архитектуру системы для определения конфигурации сервера во время запуска с помощью документированной [формулы](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md#Recommendations).

Это расширенный параметр, и изменять его следует только опытным администраторам баз данных или сертифицированным по SQL Server специалистам. Если вы считаете, что есть проблема с производительностью, вероятно, причина не в доступности рабочих потоков. Скорее всего, их ожидание вызвано чем-то наподобие операций ввода-вывода. Рекомендуется найти причину проблемы производительности, прежде чем изменять параметр max worker threads. Тем не менее, если необходимо вручную задать максимальное число рабочих потоков, это значение конфигурации всегда должно быть в семь раз больше числа ЦП, имеющихся в системе. Дополнительные сведения см. в статье [Настройка параметра max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).

### <a name="using-sql-trace-and-sql-server-profiler"></a>Использование приложения SQL Trace и SQL Server Profiler
В рабочей среде использовать трассировку SQL и SQL Profiler не рекомендуется. Издержки использования этих средств также увеличиваются по мере увеличения числа ЦП. Если в рабочей среде необходимо использовать приложение трассировки SQL, следует ограничить до минимума число отслеживаемых событий. Следует внимательно тестировать каждое событие трассировки под нагрузкой и избегать сочетания событий, которые могут значительно повлиять на производительность.

> [!IMPORTANT]
> Трассировка SQL и [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] являются устаревшими. Пространство имен *Microsoft.SqlServer.Management.Trace*, которое содержит объекты трассировки Microsoft SQL Server и Replay, также устаревшее. 
> [!INCLUDE[ssNoteDepFutureAvoid](../includes/ssnotedepfutureavoid-md.md)] 
> Вместо этого используйте расширенные события. Дополнительные сведения о [расширенных событиях](../relational-databases/extended-events/extended-events.md) см. в разделе [Быстрое начало. Расширенные события в SQL Server](../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) и [SSMS XEvent Profiler](../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] для рабочей нагрузки служб Analysis Services не устарел и будет по-прежнему поддерживаться.

### <a name="setting-the-number-of-tempdb-data-files"></a>Установка количества файлов данных базы данных tempdb
Количество файлов зависит от числа (логических) процессоров на компьютере. Как правило, если число логических процессоров меньше или равно восьми, используйте равное ему число файлов данных. Если число логических процессоров больше восьми, используйте восемь файлов данных, а затем, если состязание сохраняется, увеличивайте число файлов данных на значение, кратное 4, пока состязание не уменьшится до приемлемого уровня, или внесите изменения в рабочую нагрузку или код. Кроме того, помните о других рекомендациях для tempdb, доступных в разделе [Оптимизация производительности базы данных tempdb в SQL Server](../relational-databases/databases/tempdb-database.md#optimizing-tempdb-performance-in-sql-server). 

Однако можно уменьшить ресурсы, используемые для управления базой данных, внимательно изучив параллелизм базы данных tempdb. Например, если в системе используется 64 ЦП, при этом только 32 запроса используют tempdb, увеличение числа файлов tempdb до 64 не приведет к улучшению производительности.

### <a name="sql-server-components-that-can-use-more-than-64-cpus"></a>Компоненты SQL Server, которые поддерживают более 64 ЦП
В приведенной ниже таблице перечисляются компоненты [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] и указывается, поддерживают ли они более 64 процессоров.

|Имя процесса   |Исполняемая программа |Использование более 64 процессоров |  
|----------|----------|----------|  
|Компонент SQL Server Database Engine |Sqlserver.exe  |Да |  
|Службы Reporting Services |Rs.exe |Нет |  
|Службы Analysis Services  |As.exe |Нет |  
|Службы Integration Services   |Is.exe |Нет |  
|Компонент Service Broker |Sb.exe |Нет |  
|Компонент Full-text Search   |Fts.exe    |Нет |  
|Агент SQL Server   |Sqlagent.exe   |Нет |  
|SQL Server Management Studio   |Ssms.exe   |Нет |  
|программа установки SQL Server   |Setup.exe  |Нет |  

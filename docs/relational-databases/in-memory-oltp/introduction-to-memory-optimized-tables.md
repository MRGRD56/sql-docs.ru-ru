---
title: Введение в таблицы, оптимизированные для памяти | Документация Майкрософт
description: Узнайте о таблицах, оптимизированных для памяти, которые являются устойчивыми и поддерживают атомарные, согласованные, изолированные и устойчивые транзакции.
ms.custom: ''
ms.date: 12/02/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ef1cc7de-63be-4fa3-a622-6d93b440e3ac
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c601abb45b9e9721d62ba610570f8cbf00e2b6b5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489373"
---
# <a name="introduction-to-memory-optimized-tables"></a>Введение в таблицы, оптимизированные для памяти
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Оптимизированные для памяти таблицы создаются с помощью инструкции [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 Таблицы, оптимизированные для памяти, по умолчанию полностью устойчивы. Как и транзакции на (стандартных) дисковых таблицах, транзакции на оптимизированных для памяти таблицах полностью соответствуют классификации ACID (atomic, consistent, isolated, durable — атомарные, целостные, изолированные и устойчивые). Оптимизированные для памяти таблицы и скомпилированные в собственном коде хранимые процедуры поддерживают только подмножество компонентов [!INCLUDE[tsql](../../includes/tsql-md.md)].
 
Начиная с SQL Server 2016 и в базе данных SQL Azure нет ограничений для [параметров сортировки или кодовых страниц](../../relational-databases/collations/collation-and-unicode-support.md) , относящихся к OLTP в памяти.
  
 Первичное хранилище для таблиц, оптимизированных для памяти, — это основная память. Строки из таблицы считываются и записываются в память. Вторая копия табличных данных хранится на диске, но только с целью увеличения устойчивости. Дополнительные сведения о надежных таблицах см. в разделе [Создание и управление хранилищем для оптимизированных для памяти объектов](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md) . Данные в таблицах, оптимизированных для памяти, считываются с диска только в ходе восстановления базы данных (например, после перезапуска сервера).  
  
 Чтобы еще больше повысить производительность, OLTP в памяти поддерживает устойчивые таблицы с отложенной устойчивостью транзакций. Отложенные устойчивые транзакции сохраняются на диск вскоре после фиксации транзакции и возврата управления клиенту. Платой за повышение производительности является то, что зафиксированные транзакции, не сохраненные на диск, теряются при сбое или отказе сервера.  
  
 Помимо устойчивых оптимизированных для памяти таблиц [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] также поддерживает неустойчивые таблицы в памяти, которые не заносятся в журнал, а их данные не сохраняются на диске. Это означает, что транзакции на этих таблицах не требуют каких-либо дисковых операций ввода-вывода, но их данные не будут восстановлены при сбое или отработке отказа сервера.  
  
 OLTP-операции в памяти интегрированы с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для эффективной работы в любых областях, таких как разработка, развертывание, управление и поддержка. База данных может содержать как объекты в памяти, так и объекты на диске.  
  
 Строки в оптимизированных для памяти таблицах имеют версии. То есть каждая строка в таблице потенциально имеет несколько версий. Все версии строк сохраняются в одной структуре данных таблицы. Управление версиями строк используется, чтобы разрешить параллельный режим чтения и записи для одной строки. Дополнительные сведения о параллельном режиме чтения и записи для одной строки см. в разделе [Операции с таблицами, оптимизированными для памяти](../../relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md).  
  
 На следующем рисунке показана работа с несколькими версиями строк. На схеме показана таблица с тремя строками, у каждой строки своя версия.  
  
![Управление версиями.](../../relational-databases/in-memory-oltp/media/hekaton-tables-1.gif "Управление версиями.")  
  
 Таблица содержит три строки: r1, r2 и r3. r1 содержит три версии, r2 — 2 версии, и r3 — 4 версии. Обратите внимание, что разные версии одной и той же строки не обязательно занимают последовательные области памяти. Различные версии строк могут быть распределены по всей структуре данных таблицы.  
  
 Структуру данных таблицы, оптимизированной для памяти, можно рассматривать как коллекцию версий строк. Строки в таблицах, сохраняемых на диске, разбиты по страницам и расширениям, при этом доступ к отдельным строкам осуществляется через номер страницы и смещение. Доступ к версиям строк в таблицах с оптимизацией для памяти осуществляется посредством указателей памяти, занимающих 8 байт.  
  
 Доступ к данным в таблицах, оптимизированных для памяти, можно получить двумя способами:  
  
-   Использование хранимых процедур, скомпилированных в собственном коде.  
  
-   С помощью интерпретированного [!INCLUDE[tsql](../../includes/tsql-md.md)]вне компилированной в собственном коде хранимой процедуры. Эти инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] могут находиться в интерпретируемых хранимых процедурах или представлять собой специализированные инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
## <a name="accessing-data-in-memory-optimized-tables"></a>Доступ к данным в оптимизированных для памяти таблицах  

Доступ к оптимизированным для памяти таблицам наиболее эффективным образом можно осуществлять из скомпилированных в собственном коде хранимых процедур ([Скомпилированные в собственном коде хранимые процедуры](./a-guide-to-query-processing-for-memory-optimized-tables.md)). К оптимизированным для памяти таблицам можно обращаться через обычный интерпретируемый [!INCLUDE[tsql](../../includes/tsql-md.md)]. Термин «интерпретируемый [!INCLUDE[tsql](../../includes/tsql-md.md)] » означает доступ к оптимизированным для памяти таблицам без использования скомпилированной хранимой процедуры. Примеры интерпретируемого доступа [!INCLUDE[tsql](../../includes/tsql-md.md)] — доступ к оптимизированной для памяти таблице из триггера DML или специального пакета [!INCLUDE[tsql](../../includes/tsql-md.md)] , представления и функции с табличным значением.  
  
 В следующей таблице представлены средства собственного и интерпретируемого доступа [!INCLUDE[tsql](../../includes/tsql-md.md)] для различных объектов.  
  
|Компонент|Доступ с помощью хранимой процедуры, скомпилированной в собственном коде|Интерпретируемый доступ [!INCLUDE[tsql](../../includes/tsql-md.md)]|Доступ по CLR-адресу|  
|-------------|-------------------------------------------------------|-------------------------------------------|----------------|  
|Таблица, оптимизированная для памяти|Да|Да|Нет*|  
|Табличный тип, оптимизированный для памяти|Да|Да|Нет|  
|Хранимая процедура, скомпилированная в собственном коде|Вложение скомпилированных в собственном коде хранимых процедур поддерживается. Синтаксис EXECUTE можно использовать внутри хранимых процедур при условии, что соответствующие процедуры также скомпилированы в собственном коде.|Да|Нет*|  
  
 \* Нельзя получить доступ к оптимизированной для памяти таблице или хранимой процедуре, скомпилированной в собственном коде, из контекстного соединения (соединения из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при выполнении модуля CLR). Однако можно создать и открыть другое соединение, из которого можно получить доступ к оптимизированным для памяти таблицам и хранимым процедурам, скомпилированным в собственном коде.  
  
## <a name="performance-and-scalability"></a>Производительность и масштабируемость  

Следующие факторы влияют на повышение производительности при использовании OLTP в памяти:  
  
*Взаимодействие*. Выигрыш в производительности приложения с множеством вызовов коротких хранимых процедур может быть меньше, чем у приложения с меньшим числом вызовов и более функциональными хранимыми процедурами.  
  
*[!INCLUDE[tsql](../../includes/tsql-md.md)]Выполнение*. OLTP в памяти достигает наивысшей производительности при использовании хранимых процедур, скомпилированных в собственном коде, вместо интерпретированных хранимых процедур или выполнения запроса. Иногда удобно получать доступ к оптимизированным для памяти таблицам из таких хранимых процедур.  
  
*Просмотр диапазона или поиск точек*. Некластеризованные, оптимизированные для памяти индексы поддерживают просмотр диапазона и упорядоченные просмотры. С уточняющими запросами оптимизированные для памяти хэш-индексы более производительны, чем оптимизированные для памяти некластеризованные индексы. Оптимизированные для памяти некластеризованные индексы имеют более высокую производительность, чем дисковые индексы.

- Начиная с версии SQL Server 2016 план запроса оптимизированной для памяти таблицы может сканировать таблицу в параллельном режиме. Это повышает производительность аналитических запросов.
  - Хэш-индексы также стали поддерживать сканирование в параллельном режиме в SQL Server 2016.
  - Некластеризованные индексы также стали поддерживать сканирование в параллельном режиме в SQL Server 2016.
  - Индексы columnstore поддерживают сканирование в параллельном режиме с момента своего появления в SQL Server 2014.
  
*Операции с индексами*. Операции с индексами не регистрируются и находятся только в памяти.  
  
*Параллелизм*. Производительность приложений, которая зависит от параллелизма компонента уровня СУБД, например от конфликтов кратковременной блокировки, значительно повышается при переходе на OLTP в памяти.  
  
В следующей таблице перечислены проблемы с производительностью и масштабируемостью, которые часто встречаются в реляционных базах данных, и описывается, как OLTP в памяти может улучшить производительность.  
  
|Проблема|Влияние OLTP в памяти|  
|-----------|----------------------------|  
|Производительность<br /><br /> Высокая интенсивность использования ресурсов (ЦП, ввода-вывода, сети или памяти).|ЦП<br /> Скомпилированные в собственном коде хранимые процедуры могут значительно снизить уровень загрузки ЦП, так как им требуется значительно меньше инструкций на выполнение команды [!INCLUDE[tsql](../../includes/tsql-md.md)] , чем интерпретированным хранимым процедурам.<br /><br /> In-Memory OLTP может снизить затраты на оборудование для масштабированных рабочих нагрузок, поскольку один сервер потенциально может обеспечить пропускную способность 5–10 серверов.<br /><br /> Ввод-вывод<br /> OLTP в памяти может уменьшить влияние узких мест в операциях ввода-вывода при обработке страниц данных или индекса. Кроме того, назначение контрольных точек для объектов OLTP в памяти происходит непрерывно и не приводит к неожиданному увеличению количества операций ввода-вывода. Однако если рабочее множество важных для производительности таблиц не помещается в памяти, то OLTP в памяти не улучшит производительность, поскольку данные должны быть резидентными. Если возникает задержка в операциях ввода-вывода при ведении журнала, то OLTP в памяти может снизить эту задержку, так как в журнал записывается меньше данных. Если одна или несколько таблиц, оптимизированных для памяти, настроены как устойчивые, журналирование данных можно исключить.<br /><br /> Память<br /> OLTP в памяти не повышает производительность. OLTP в памяти может вызывать дополнительную нагрузку на память, так как объекты должны быть резидентными.<br /><br /> Сеть<br /> OLTP в памяти не повышает производительность. Данные нужно передавать с уровня данных на уровень приложения.|  
|Масштабируемость<br /><br /> Большинство проблем с масштабированием в приложениях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вызываются проблемами параллелизма, такими как конфликт в блокировках, кратковременных блокировках и спин-блокировках.|Конфликты кратковременных блокировок<br /> Типичный сценарий — конфликт на последней странице индекса при параллельной вставке строк в порядке значений ключей. OLTP в памяти не использует кратковременные блокировки при доступе к данным, поэтому проблемы масштабируемости, связанные с конфликтом за кратковременные блокировки, полностью устраняются.<br /><br /> Конфликт спин-блокировок<br /> OLTP в памяти не использует кратковременные блокировки при доступе к данным, поэтому проблемы масштабируемости, связанные с состязанием за спин-блокировки, полностью устраняются.<br /><br /> Конфликты, связанные с блокировкой<br /> OLTP в памяти устраняет критические препятствия между операциями чтения и записи в приложении базы данных, поскольку используется новая форма управления оптимистической конкуренцией для реализации всех уровней изоляции транзакций. OLTP в памяти не использует базу данных TempDB для хранения версий строк.<br /><br /> Если ошибка масштабирования вызвана конфликтом между двумя операциями записи, например двумя параллельными транзакциями, которые пытаются обновить одну и ту же строку, OLTP в памяти позволяет завершить одну транзакцию и отклоняет другую. Поврежденную транзакцию необходимо представить повторно явно или неявно. В любом случае необходимо внести изменения в приложение.<br /><br /> Если в приложении часто возникают конфликты между двумя операциями записи, значимость оптимистической блокировки уменьшается. Приложение не подходит для работы с OLTP в памяти. В большинстве OLTP-приложений отсутствуют конфликты записи, если конфликт не вызывается укрупнением блокировки.|  
  
##  <a name="row-level-security-in-memory-optimized-tables"></a><a name="rls"></a> Безопасность на уровне строк в оптимизированных для памяти таблицах  

[Безопасность на уровне строк](../../relational-databases/security/row-level-security.md) поддерживается в таблицах, оптимизированных для памяти. Применение политик безопасности на уровне строк в таблицах, оптимизированных для памяти, практически не отличается от применения политик в таблицах на дисках, за исключением того, что встроенные функции с табличным значением, используемые в качестве предикатов безопасности, должны быть скомпилированы в собственном коде (созданы с помощью параметра WITH NATIVE_COMPILATION). Подробные сведения см. в подразделе [Совместимость с разными компонентами](../../relational-databases/security/row-level-security.md#Limitations) раздела [Безопасность на уровне строк](../../relational-databases/security/row-level-security.md) .  
  
 В таблицы в памяти включены различные встроенные функции безопасности, необходимые для безопасности на уровне строк. Подробные сведения см. в разделе [Встроенные функции в модулях, скомпилированных в собственном коде](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md#bfncsp).  
  
 **EXECUTE AS CALLER** . Все собственные модули теперь поддерживают и используют EXECUTE AS CALLER по умолчанию, даже если указание не задано. Это происходит потому, что предполагается, что все функции предиката безопасности на уровне строк будут использовать предложение EXECUTE AS CALLER, чтобы функция (и все встроенные функции, используемые в ней) вычислялась в контексте вызывающего пользователя.   
Производительность предложения EXECUTE AS CALLER немного снижена (приблизительно на 10 %) из-за проверок разрешений вызывающего объекта. Если модуль явно указывает предложение EXECUTE AS OWNER или EXECUTE AS SELF, это позволит избежать проверок разрешений и сопутствующего снижения производительности. Однако использование любого из этих параметров вместе с встроенными функциями, указанными выше, повлечет за собой значительное снижение производительности из-за необходимости переключения контекста.  
  
## <a name="scenarios"></a>Сценарии

Краткое описание типичных сценариев, в которых [!INCLUDE[hek_1](../../includes/hek-1-md.md)] может улучшить производительность, см. в разделе [Выполняющаяся в памяти OLTP](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
## <a name="see-also"></a>См. также:

[Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  

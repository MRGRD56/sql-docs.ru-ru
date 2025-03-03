---
title: Безопасность на уровне строк
description: Узнайте, как безопасность на уровне строк позволяет использовать членство в группе или контекст выполнения для управления доступом к строкам в таблице базы данных в SQL Server.
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- access control predicates
- row level security
- security [SQL Server], predicate based access control
- row level security described
- predicate based security
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6d650a8313a98bcbb44b28c5797c02e48f57a4f9
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2021
ms.locfileid: "107561106"
---
# <a name="row-level-security"></a>Безопасность на уровне строк

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  ![Предупреждение о безопасности на уровне строк](../../relational-databases/security/media/row-level-security-graphic.png "Предупреждение о безопасности на уровне строк")  
  
Безопасность на уровне строк позволяет использовать членство в группе или контекст выполнения для управления доступом к строкам в таблице базы данных.
  
Безопасность на уровне строк (RLS) упрощает проектирование и кодирование безопасности в приложении. Безопасность на уровне строк помогает реализовать ограничения на доступ к строкам данных. Например, вы можете предоставить работникам доступ только к тем строкам данных, которые связаны с работой их отдела. Еще один пример — предоставление доступа для клиентов только к тем данным, которые относятся к их компании.  
  
Логика ограничения находится на уровне базы данных, а не на отдалении от данных на другом уровне приложения. Система базы данных применяет ограничения доступа каждый раз, когда выполняется попытка доступа к данным с любого уровня. Это делает систему безопасности более надежной и устойчивой за счет уменьшения контактной зоны системы безопасности.  
  
Реализуйте безопасность на уровне строк с помощью инструкции [CREATE SECURITY POLICY](../../t-sql/statements/create-security-policy-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] и предикатов в виде [встроенных функций с табличным значением](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md).  

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([получить](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)), [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].
  
> [!NOTE]
> Azure Synapse поддерживает только предикаты фильтров. Предикаты блокирования сейчас не поддерживаются в Azure Synapse.

## <a name="description"></a><a name="Description"></a> Описание

Безопасность на уровне строк поддерживает два типа предикатов безопасности.  
  
- Предикаты фильтров автоматически фильтруют строки, доступные для операций чтения (SELECT, UPDATE и DELETE).  
  
- Предикаты BLOCK явно блокируют операции записи (AFTER INSERT, AFTER UPDATE, BEFORE UPDATE, BEFORE DELETE), которые нарушают предикат.  
  
 Доступ к данным на уровне строк в таблице ограничен предикатом безопасности, определяемым как встроенная функция с табличным значением. Эта функция затем вызывается и принудительно исполняется политикой безопасности. При использовании предикатов фильтров приложение не учитывает строки, отфильтрованные из результирующего набора. При фильтрации всех строк возвращается набор NULL. Что касается предикатов блокировки, любые операции, которые нарушают предикат, будут завершаться ошибкой.  
  
 Предикаты фильтров применяются при считывании данных из базовой таблицы. Они влияют на все операции Get: **SELECT**, **DELETE** и **UPDATE**. Пользователи не могут выбирать, удалять и обновлять отфильтрованные строки. Но они могут обновлять строки так, чтобы они были отфильтрованы позже. Предикаты блокировки влияют на все операции записи.  
  
- Предикаты AFTER INSERT и AFTER UPDATE могут блокировать обновление строк значениями, нарушающими предикат.  
  
- Предикаты BEFORE UPDATE могут блокировать обновление строк, нарушающих предикат на данный момент.  
  
- Предикаты BEFORE DELETE могут блокировать операции удаления.  
  
 Предикаты фильтров и блокировки, а также политики безопасности имеют следующие особенности:  
  
- Вы можете определить функцию предиката, которая соединяется с другой таблицей или вызывает функцию. Если политика безопасности создана с использованием команды `SCHEMABINDING = ON` (по умолчанию), соединение или функция будут доступными из запроса и будут работать должным образом без каких-либо дополнительных проверок разрешений. Если политика безопасности создана с использованием `SCHEMABINDING = OFF`, для отправки запросов в целевую таблицу вам потребуются разрешения **SELECT** в этих дополнительных таблицах и функциях. Если функция предиката вызывает скалярную функцию CLR, также потребуется разрешение **EXECUTE**.
  
- Вы можете выполнить запрос к таблице, имеющей предикат безопасности, который определен, но отключен. Все отфильтрованные или заблокированные строки не затрагиваются.  
  
- Когда пользователь dbo, член роли **db_owner** или владелец таблицы выполняет запрос к таблице, для которой определена или включена политика безопасности, строки фильтруются или блокируются в соответствии с этой политикой.  
  
- Попытка изменить схему таблицы, связанной политикой безопасности привязки к схеме, приведет к ошибке. Тем не менее можно изменить столбцы, на которые не ссылается предикат.  
  
- Попытки добавить предикат в таблицу, в которой уже есть один определенный предикат для указанной операции, приведет к ошибке (независимо от того, включен ли предикат или отключен).
  
- Попытка изменить функцию, которая используется в качестве предиката в таблице в рамках привязанной к схеме политики безопасности, приведет к ошибке.  
  
- Определение нескольких активных политик безопасности, содержащих неперекрывающиеся предикаты, завершается успешно.  
  
 Предикаты фильтров имеют следующие особенности.  
  
- Определение политики безопасности, которая фильтрует строки таблицы. Приложение не учитывает отфильтрованные строки в операциях **SELECT**, **UPDATE** и **DELETE**, включая ситуации, когда все строки исключены. Приложение может применить операцию **INSERT** к строкам, даже если они будут отфильтрованы во время любой другой операции.  
  
 Предикаты блокировки имеют следующие особенности.  
  
- Предикаты блокировки для операций UPDATE разбиваются на отдельные операции BEFORE и AFTER. Таким образом, нельзя, например, запретить пользователям обновлять строки, добавляя значение, которое больше текущего. Если требуется такая логика, необходимо использовать триггеры с промежуточными таблицами [DELETED и INSERTED](../triggers/use-the-inserted-and-deleted-tables.md), чтобы создать ссылки на новые и старые значения.  
  
- Оптимизатор не будет проверять предикат блокировки AFTER UPDATE, если не изменяется ни один из столбцов, используемых функцией предиката. Пример: Алиса не должна иметь возможность изменить зарплату на значение, превышающее 100 000. Алиса может изменить адрес сотрудника, зарплата которого уже превышает 100 000, если столбцы, ссылки на которые указаны в предикате, не были изменены.  
  
- Изменения не вносились для пакетных API, в том числе BULK INSERT. Это означает, что предикаты блокировки AFTER INSERT будут применяться к операциям пакетной вставки так же, как к обычным операциям вставки.  
  
## <a name="use-cases"></a><a name="UseCases"></a> Способы применения

 Ниже приведены примеры конструирования использования RLS.  
  
- Больницы могут создать политику безопасности, которая позволяет медсестрам просматривать строки данных только их пациентов.  
  
- Банк может создать политику для ограничения доступа к строкам финансовых данных на основе бизнес-подразделения сотрудника либо на основе роли сотрудника в компании.  
  
- Мультитенантное приложение может создать политику для обеспечения логического разделения строк каждого клиента от строк любых других клиентов. Эффективность достигается путем хранения данных для многих клиентов в одной таблице. Каждый клиент может видеть только свои строки данных.  
  
 Предикаты фильтров RLS функционально эквивалентны добавлению предложения **WHERE** . Предикат может по сложности сравниваться с определением деловой практики или предложение может быть простым как `WHERE TenantId = 42`.  
  
 При использовании более формальных терминов можно сказать, что RLS представляет управление доступом на основе предиката. При этом поддерживаются гибкие централизованные вычисления на основе предиката. Предикат может учитывать метаданные или другие критерии, определяемые администратором по своему усмотрению. Предикат используется как критерий для определения, имеет ли пользователь необходимый доступ к данным, на основе пользовательских атрибутов. Управление доступом на основе метки можно реализовать с помощью управления доступом на основе предиката.  
  
## <a name="permissions"></a><a name="Permissions"></a> Permissions

 Создание, изменение или удаление политик безопасности требует разрешения **ALTER ANY SECURITY POLICY** . Создание или удаление политики безопасности требует разрешения **ALTER** для схемы.  
  
 Кроме того, для каждого добавляемого предиката требуются следующие разрешения:  
  
- Разрешения **SELECT** и **REFERENCES** для функции используются как предикат.  
  
- Разрешение **REFERENCES** для целевой таблицы, которая привязывается к политике.  
  
- Разрешение **REFERENCES** для каждого столбца из целевой таблицы, используемого в качестве аргументов.  
  
 Политики безопасности применяются ко всем пользователям, включая пользователей dbo в базе данных. Пользователи dbo могут изменять или удалять политики безопасности, однако можно проводить аудит их изменений в политиках безопасности. Если привилегированным пользователям (например, sysadmin или db_owner) нужно видеть все строки для устранения неполадок или проверки данных, необходимо создать политику безопасности, разрешающую эти действия.  
  
 Если политика безопасности создается с использованием команды `SCHEMABINDING = OFF`, то для отправки запроса в целевую таблицу пользователям потребуется разрешение  **SELECT** или **EXECUTE** в функции предиката и любых дополнительных таблицах, представлениях и функциях, используемых в функции предиката. Если политика безопасности создана с использованием `SCHEMABINDING = ON` (по умолчанию), при запросе целевой таблицы пользователями эти проверки разрешений не проводятся.  
  
## <a name="best-practices"></a><a name="Best"></a> Рекомендации  
  
- Мы настоятельно рекомендуем создавать отдельную схему для объектов, функции предиката и политики безопасности RLS. Это поможет разделить разрешения, требуемые для работы этих специальных объектов из целевых таблиц. В базах данных с несколькими клиентами может потребоваться дополнительное разделение для различных политик и функций предикатов, но это не является стандартным решением для каждого случая.
  
- Разрешение **ALTER ANY SECURITY POLICY** предназначено для пользователей с высоким уровнем привилегий (например, для администратора политик безопасности). Администратору политик безопасности не требуется разрешение **SELECT** для защищаемых политиками таблиц.  
  
- Избегайте преобразования типов в функциях предиката, чтобы исключить потенциальные ошибки выполнения.  
  
- Там где возможно, избегайте рекурсии в функциях предиката, чтобы не допустить снижения производительности. Оптимизатор запросов попытается обнаружить прямые рекурсии, но не обязательно найдет косвенные рекурсии (т. е. когда вторая функция вызывает функцию предиката).  
  
- Избегайте использования излишних соединений таблиц в функциях предиката для повышения производительности.  
  
 Избегайте реализации логики предикатов, зависящей от [параметров SET](../../t-sql/statements/set-statements-transact-sql.md) конкретного сеанса: хотя такое использование на практике маловероятно, функции предиката, логика которых зависит от некоторых параметров **SET** отдельных сеансов, могут вызвать утечку информации, если пользователи могут выполнять произвольные запросы. Например, функция предиката, которая неявно преобразует строку в **datetime** , может выполнять фильтрацию разных строк на основе параметра **SET DATEFORMAT** для текущего сеанса. Как правило, функции предикатов должны подчиняться следующим правилам.  
  
- Функции предикатов не должны неявно преобразовать строки символов в типы данных **date**, **smalldatetime**, **datetime**, **datetime2** или **datetimeoffset** (и наоборот), поскольку на эти преобразования влияют параметры [SET DATEFORMAT (Transact-SQL)](../../t-sql/statements/set-dateformat-transact-sql.md) и [SET LANGUAGE (Transact-SQL)](../../t-sql/statements/set-language-transact-sql.md). Вместо этого используйте функцию **CONVERT** и явно задайте параметр стиля.  
  
- Функции предикатов не должны зависеть от значения первого дня недели, поскольку это значение зависит от параметра [SET DATEFIRST (Transact-SQL)](../../t-sql/statements/set-datefirst-transact-sql.md).  
  
- Функции предикатов не должны зависеть от арифметических или агрегатных выражений, возвращающих значение **NULL** в случае ошибки (например, при переполнении или делении на ноль), так как это поведение определяется параметрами [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md), [SET NUMERIC_ROUNDABORT (Transact-SQL)](../../t-sql/statements/set-numeric-roundabort-transact-sql.md) и [SET ARITHABORT (Transact-SQL)](../../t-sql/statements/set-arithabort-transact-sql.md).  
  
- Функции предикатов не должны сравнивать сцепленные строки с параметром **NULL**, так как это поведение определяется параметром [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).  

## <a name="security-note-side-channel-attacks"></a><a name="SecNote"></a> Примечание по безопасности. Атаки на стороне канала

### <a name="malicious-security-policy-manager"></a>Злонамеренный диспетчер политики безопасности

важно иметь в виду, что злонамеренный диспетчер политики безопасности с достаточными разрешениями для создания политики безопасности конфиденциальных столбцов и имеющий разрешения на создание или изменение встроенных функций, возвращающих табличные значения, может вступить в тайный сговор с другим пользователем, который имеет отдельные разрешения на таблицу для выполнения эксфильтрации данных с помощью злонамеренно созданных встроенных функций, возвращающих табличные значения, разработанные для использования атак на стороне канала с целью вывода данных. Такие атаки потребуют сговора (или предоставления избыточных разрешений злоумышленнику) и, скорее всего, нескольких итераций изменения политики (требующих разрешения на удаление предиката, чтобы разорвать привязку схем), а также изменения встроенных функций с табличным значением и многократного выполнения инструкций SELECT в целевой таблице. Мы рекомендуем назначать только необходимые разрешения и отслеживать все подозрительные действия, например постоянное изменение политик и встроенных функций с табличными значениями, затрагивающее безопасность на уровне строк.  
  
### <a name="carefully-crafted-queries"></a>Тщательно созданные запросы

можно вызвать утечку информации с помощью тщательно созданных запросов. Например, `SELECT 1/(SALARY-100000) FROM PAYROLL WHERE NAME='John Doe'` дает возможность злоумышленнику узнать, что заработная платы Джона До (John Doe) составляет 100 000 долларов. Даже при наличии предиката безопасности для предотвращения ситуаций, когда злонамеренный пользователь может напрямую выполнять запросы о заработной плате других людей, пользователь может определить, когда запрос возвращает исключение деления на ноль.  

## <a name="cross-feature-compatibility"></a><a name="Limitations"></a> Совместимость с разными компонентами

 Как правило, безопасность на уровне строк будет работать должным образом в разных компонентах. Однако существует несколько исключений. В этом разделе приводится несколько замечаний и пояснений по использованию безопасности на уровне строк в некоторых других компонентах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
- **DBCC SHOW_STATISTICS** предоставляет статистику по нефильтрованным данным и может вызвать утечку информации, которая должна быть защищена политикой безопасности. По этой причине доступ к просмотру объектов статистики для таблицы с политикой безопасности на уровне строк ограничен. Пользователь должен быть владельцем таблицы, членом предопределенной роли сервера sysadmin, предопределенной роли базы данных db_owner или предопределенной роли базы данных db_ddladmin.  
  
- **Filestream.** Безопасность на уровне строк не совместима с компонентом Filestream.  
  
- **PolyBase:** безопасность на уровне строк поддерживается для внешних таблиц в Azure Synapse и SQL Server 2019 CU7 или более поздней версии. 

- **Таблицы, оптимизированные для памяти.** Встроенная функция с табличным значением, которая используется в качестве предиката безопасности для таблицы, оптимизированной для памяти, должна быть определена с помощью параметра `WITH NATIVE_COMPILATION`. Этот параметр позволяет блокировать функции языка, не поддерживаемые в оптимизированных для памяти таблицах, и выдавать соответствующую ошибку во время создания. Дополнительные сведения см. в разделе **Безопасность на уровне строк в таблицах, оптимизированных для памяти** статьи [Вводные сведения о таблицах, оптимизированных для памяти](../../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md).  
  
- **Индексированные представления.** Как правило, политики безопасности можно создавать на основе представлений, а представления — на основе таблиц, связанных политиками безопасности. Тем не менее индексированные представления нельзя создать на основе таблиц с политикой безопасности, так как операции поиска строк через индекс будут обходить политику.  
  
- **Отслеживание измененных данных.** Система отслеживания измененных данных может вызвать утечку целых строк, которые должны быть отфильтрованы для доступа членов **db_owner** или пользователей, являющихся членами "шлюзовой" роли, указанной при включении этой системы для таблицы. (Примечание. Для этой функции можно явно задать значение **NULL**, чтобы все пользователи имели доступ к измененным данным.) В результате параметр **db_owner** и члены такой шлюзовой роли могут просматривать все изменения данных в таблице даже при наличии политики безопасности для таблицы.  
  
- **Отслеживание изменений.** Отслеживание изменений может вызвать утечку первичного ключа строк, которые должны быть отфильтрованы для доступа пользователей с разрешениями **SELECT** и **VIEW CHANGE TRACKING**. Доступ к фактическим значениям данных не предоставляется, становится известно только то, что столбец A был обновлен (вставлен или удален) для строки с первичным ключом B. Это создает проблему, если первичный ключ содержит конфиденциальные элементы, например номер социального страхования. Тем не менее на практике для получения последних данных инструкция **CHANGETABLE** почти всегда объединена с исходной таблицей.  
  
- **Полнотекстовый поиск.** Запросы, использующие следующие функции полнотекстового и семантического поиска, будут выполняться со сниженной производительностью из-за введения дополнительного соединения для применения безопасности на уровне строк и блокирования утечки первичных ключей строк, которые должны быть отфильтрованы: **CONTAINSTABLE**, **FREETEXTTABLE**, semantickeyphrasetable, semanticsimilaritydetailstable, semanticsimilaritytable.  
  
- **Индексы Columnstore.** Безопасность на уровне строк совместима с кластеризованными и некластеризованными индексами columnstore. Тем не менее, поскольку безопасность на уровне строк применяет функцию, оптимизатор может изменить план запроса таким образом, чтобы пакетный режим не использовался.  
  
- **Секционированные представления.** Предикаты блокировки нельзя определить в секционированных представлениях, а секционированные представления нельзя создавать на основе таблиц, использующих предикаты блокировки. Предикаты фильтров совместимы с секционированными представлениями.  
  
- **Темпоральные таблицы.** Темпоральные таблицы совместимы с безопасностью на уровне строк. Тем не менее предикаты безопасности в текущей таблице не реплицируются автоматически в прежнюю таблицу. Чтобы применить политику безопасности для текущей и прежней таблиц, необходимо по отдельности добавить предикат безопасности в каждую таблицу.  
  
## <a name="examples"></a><a name="CodeExamples"></a> Примеры  
  
### <a name="a-scenario-for-users-who-authenticate-to-the-database"></a><a name="Typical"></a> A. Сценарий для пользователей, проходящих проверку подлинности в базе данных

 В этом примере создаются три пользователя, создается и заполняется таблица с шестью строками, а затем создается встроенная функция с табличным значением и политика безопасности для таблицы. Пример затем показывает, как фильтруются отдельные инструкции для разных пользователей.  
  
 Создайте три учетные записи пользователей, демонстрирующие разные возможности доступа.  


```sql  
CREATE USER Manager WITHOUT LOGIN;  
CREATE USER SalesRep1 WITHOUT LOGIN;  
CREATE USER SalesRep2 WITHOUT LOGIN;
GO
```

Создайте таблицу для хранения данных.  

```sql
CREATE SCHEMA Sales
GO
CREATE TABLE Sales.Orders 
    (  
    OrderID int,  
    SalesRep nvarchar(50),  
    Product nvarchar(50),  
    Quantity smallint  
    );  
```

 Заполните таблицу шестью строками данных, показывающими три заказа для каждого торгового представителя.  

```sql
INSERT INTO Sales.Orders  VALUES (1, 'SalesRep1', 'Valve', 5);
INSERT INTO Sales.Orders  VALUES (2, 'SalesRep1', 'Wheel', 2);
INSERT INTO Sales.Orders  VALUES (3, 'SalesRep1', 'Valve', 4);
INSERT INTO Sales.Orders  VALUES (4, 'SalesRep2', 'Bracket', 2);
INSERT INTO Sales.Orders  VALUES (5, 'SalesRep2', 'Wheel', 5);
INSERT INTO Sales.Orders  VALUES (6, 'SalesRep2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales.Orders;
```

Предоставьте доступ для чтения к таблице для каждого из пользователей.  

```sql
GRANT SELECT ON Sales.Orders TO Manager;  
GRANT SELECT ON Sales.Orders TO SalesRep1;  
GRANT SELECT ON Sales.Orders TO SalesRep2; 
GO
```

Создайте новую схему и встроенную функцию с табличным значением. Функция возвращает 1, если строка в столбце SalesRep та же, что и пользователь, выполняющий запрос (`@SalesRep = USER_NAME()`) или, если пользователь, выполняющий запрос, является пользователем Manager (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.tvf_securitypredicate(@SalesRep AS nvarchar(50))  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS tvf_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
GO
```

Создайте политику безопасности, добавляя функцию в качестве предиката фильтра. Состоянию должно быть присвоено значение ON для включения политики.

```sql
CREATE SECURITY POLICY SalesFilter  
ADD FILTER PREDICATE Security.tvf_securitypredicate(SalesRep)
ON Sales.Orders
WITH (STATE = ON);  
GO
```

Дайте разрешение на SELECT функции fn_securitypredicate 
```sql
GRANT SELECT ON Security.tvf_securitypredicate TO Manager;  
GRANT SELECT ON Security.tvf_securitypredicate TO SalesRep1;  
GRANT SELECT ON Security.tvf_securitypredicate TO SalesRep1;  

```

Теперь протестируйте предикат фильтрации при выборе из таблицы Sales, как для каждого пользователя.

```sql
EXECUTE AS USER = 'SalesRep1';  
SELECT * FROM Sales.Orders;
REVERT;  
  
EXECUTE AS USER = 'SalesRep2';  
SELECT * FROM Sales.Orders;
REVERT;  
  
EXECUTE AS USER = 'Manager';  
SELECT * FROM Sales.Orders;
REVERT; 
```

Пользователь Manager должен видеть все шесть строк. Пользователи Sales1 и Sales2 должны видеть только свои продажи.

Измените политику безопасности, чтобы отключить политику.

```sql
ALTER SECURITY POLICY SalesFilter  
WITH (STATE = OFF);  
```

Теперь пользователи Sales1 и Sales2 могут видеть все шесть строк.

Подключение к базе данных SQL для очистки ресурсов

```sql
DROP USER SalesRep1;
DROP USER SalesRep2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter;
DROP TABLE Sales.Orders;
DROP FUNCTION Security.tvf_securitypredicate;
DROP SCHEMA Security;
DROP SCHEMA Sales;
```

### <a name="b-scenarios-for-using-row-level-security-on-an-azure-synapse-external-table"></a><a name="external"></a> Б. Сценарии использования безопасности на уровне строк во внешней таблице Azure Synapse

В этом кратком примере создаются три пользователя и внешняя таблица с шестью строками, а затем создается встроенная функция с табличным значением и политика безопасности для внешней таблицы. Пример показывает как фильтруются отдельные инструкции для разных пользователей. 

### <a name="prerequisites"></a>Предварительные требования

1. Вам потребуется выделенный пул SQL. См. статью [Создание выделенного пула SQL](/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal).
1. Сервер, на котором размещен выделенный пул SQL, должен быть зарегистрирован в AAD, и у вас должна быть учетная запись хранения Azure с разрешениями на уровне участника для данных BLOB-объектов хранилища. Следуйте инструкциям, которые приводятся [здесь](/azure/azure-sql/database/vnet-service-endpoint-rule-overview#steps).
1. Создайте файловую систему для учетной записи хранения Azure. Используйте Обозреватель службы хранилища для просмотра учетной записи хранения. Щелкните правой кнопкой мыши на контейнерах и выберите *Создать файловую систему*.  

Выполнив все предварительные условия, создайте три учетные записи пользователей, демонстрирующие разные возможности доступа.

```sql
--run in master
CREATE LOGIN Manager WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales1 WITH PASSWORD = '<user_password>'
GO
CREATE LOGIN Sales2 WITH PASSWORD = '<user_password>'
GO

--run in master and your dedicated SQL pool database
CREATE USER Manager FOR LOGIN Manager;  
CREATE USER Sales1  FOR LOGIN Sales1;  
CREATE USER Sales2  FOR LOGIN Sales2 ;
```

Создайте таблицу для хранения данных.  

```sql
CREATE TABLE Sales  
    (  
    OrderID int,  
    SalesRep sysname,  
    Product varchar(10),  
    Qty int  
    );  
```

Заполните таблицу шестью строками данных, показывающими три заказа для каждого торгового представителя.  

```sql
INSERT INTO Sales VALUES (1, 'Sales1', 'Valve', 5);
INSERT INTO Sales VALUES (2, 'Sales1', 'Wheel', 2);
INSERT INTO Sales VALUES (3, 'Sales1', 'Valve', 4);
INSERT INTO Sales VALUES (4, 'Sales2', 'Bracket', 2);
INSERT INTO Sales VALUES (5, 'Sales2', 'Wheel', 5);
INSERT INTO Sales VALUES (6, 'Sales2', 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sales;
```

Создайте внешнюю таблицу Azure Synapse на основе только что созданной таблицы Sales.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<user_password>';

CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';

CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://<file_system_name@storage_account>.dfs.core.windows.net', CREDENTIAL = msi_cred);

CREATE EXTERNAL FILE FORMAT MSIFormat  WITH (FORMAT_TYPE=DELIMITEDTEXT);
  
CREATE EXTERNAL TABLE Sales_ext WITH (LOCATION='<your_table_name>', DATA_SOURCE=ext_datasource_with_abfss, FILE_FORMAT=MSIFormat, REJECT_TYPE=Percentage, REJECT_SAMPLE_VALUE=100, REJECT_VALUE=100)
AS SELECT * FROM sales;
```

Дайте разрешение на SELECT трем пользователям созданной внешней таблицы Sales_ext.

```sql
GRANT SELECT ON Sales_ext TO Sales1;  
GRANT SELECT ON Sales_ext TO Sales2;  
GRANT SELECT ON Sales_ext TO Manager;
```

Создайте новую схему и встроенную функцию с табличным значением (это действие вы могли выполнить в примере А). Функция возвращает 1, если строка в столбце SalesRep та же, что и пользователь, выполняющий запрос (`@SalesRep = USER_NAME()`), или если пользователь, выполняющий запрос, является пользователем Manager (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@SalesRep AS sysname)  
    RETURNS TABLE  
WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result
WHERE @SalesRep = USER_NAME() OR USER_NAME() = 'Manager';  
```

Создайте политику безопасности для внешней таблицы, используя встроенную функцию с табличным значением в качестве предиката фильтра. Состоянию должно быть присвоено значение ON для включения политики.

```sql
CREATE SECURITY POLICY SalesFilter_ext
ADD FILTER PREDICATE Security.fn_securitypredicate(SalesRep)
ON dbo.Sales_ext  
WITH (STATE = ON);
```

Теперь протестируйте предикат фильтра, выбрав его из внешней таблицы Sales_ext. Выполните вход от имени каждого пользователя: Sales1, Sales2 и manager. Выполните следующую команду от имени каждого пользователя.

```sql
SELECT * FROM Sales_ext;
```

Пользователь Manager должен видеть все шесть строк. Пользователи Sales1 и Sales2 должны видеть данные только своих продаж.

Измените политику безопасности, чтобы отключить политику.

```sql
ALTER SECURITY POLICY SalesFilter_ext  
WITH (STATE = OFF);  
```

Теперь пользователи Sales1 и Sales2 могут видеть все шесть строк.

Подключение к базе данных Azure Synapse для очистки ресурсов

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter_ext;
DROP TABLE Sales;
DROP EXTERNAL TABLE Sales_ext;
DROP EXTERNAL DATA SOURCE ext_datasource_with_abfss ;
DROP EXTERNAL FILE FORMAT MSIFormat;
DROP DATABASE SCOPED CREDENTIAL msi_cred; 
DROP MASTER KEY;
```

Подключитесь к логической базе данных master, чтобы очистить ресурсы.

```sql
DROP LOGIN Sales1;
DROP LOGIN Sales2;
DROP LOGIN Manager;
```

### <a name="c-scenario-for-users-who-connect-to-the-database-through-a-middle-tier-application"></a><a name="MidTier"></a> В. Сценарий для пользователей, подключающихся к базе данных через приложение среднего уровня

> [!NOTE]
> Функции предикатов блокировки из этого примера сейчас не поддерживаются для Azure Synapse, поэтому вставка строк с неправильным идентификатором пользователя не блокируется.

В этом примере показано, как приложение среднего уровня может реализовать фильтрацию подключений, когда пользователи приложения (или клиенты) совместно используют того же пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (приложение). Приложение задает идентификатор пользователя текущего приложения в [SESSION_CONTEXT (Transact-SQL)](../../t-sql/functions/session-context-transact-sql.md) после подключения к базе данных, а затем политики безопасности прозрачно фильтруют строки, которые не должны быть видимыми для данного идентификатора, а также запрещают пользователю вставлять строки для другого ИД пользователя. Другие изменения приложения не требуются.  
  
 Создайте таблицу для хранения данных.

```sql
CREATE TABLE Sales (  
    OrderId int,  
    AppUserId int,  
    Product varchar(10),  
    Qty int  
);  
```

Заполните таблицу шестью строками данных, показывающими три заказа для каждого пользователя приложения.

```sql
INSERT Sales VALUES
    (1, 1, 'Valve', 5),
    (2, 1, 'Wheel', 2),
    (3, 1, 'Valve', 4),  
    (4, 2, 'Bracket', 2),
    (5, 2, 'Wheel', 5),
    (6, 2, 'Seat', 5);  
```

Создайте пользователя с низким уровнем привилегий, который будет использоваться приложением для подключения.

```sql
-- Without login only for demo  
CREATE USER AppUser WITHOUT LOGIN;
GRANT SELECT, INSERT, UPDATE, DELETE ON Sales TO AppUser;  
  
-- Never allow updates on this column  
DENY UPDATE ON Sales(AppUserId) TO AppUser;  
```

Создайте новую схему и предикат функции, которая будет использовать идентификатор пользователя приложения, сохраняемый в **SESSION_CONTEXT** , для фильтрации строк.

```sql
CREATE SCHEMA Security;  
GO  
  
CREATE FUNCTION Security.fn_securitypredicate(@AppUserId int)  
    RETURNS TABLE  
    WITH SCHEMABINDING  
AS  
    RETURN SELECT 1 AS fn_securitypredicate_result  
    WHERE  
        DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('AppUser')
        AND CAST(SESSION_CONTEXT(N'UserId') AS int) = @AppUserId;
GO  
```

Создайте политику безопасности, которая добавляет эту функцию в качестве предиката фильтра и предиката блокировки для `Sales`. Предикату блокировки требуется только операция **AFTER INSERT**, поскольку **BEFORE UPDATE** и **BEFORE DELETE** уже отфильтрованы, а **AFTER UPDATE** не требуется, так как для столбца `AppUserId` нельзя указать другие значения из-за разрешения столбца, которое было задано ранее.

```sql
CREATE SECURITY POLICY Security.SalesFilter  
    ADD FILTER PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales,  
    ADD BLOCK PREDICATE Security.fn_securitypredicate(AppUserId)
        ON dbo.Sales AFTER INSERT
    WITH (STATE = ON);  
```

Теперь мы можем имитировать фильтрацию подключения путем выбора из таблицы `Sales` после задания разных идентификаторов пользователей в **SESSION_CONTEXT**. На практике приложение отвечает за задание идентификатора текущего пользователя в **SESSION_CONTEXT** после открытия подключения.

```sql
EXECUTE AS USER = 'AppUser';  
EXEC sp_set_session_context @key=N'UserId', @value=1;  
SELECT * FROM Sales;  
GO  
  
/* Note: @read_only prevents the value from changing again until the connection is closed (returned to the connection pool)*/
EXEC sp_set_session_context @key=N'UserId', @value=2, @read_only=1;
  
SELECT * FROM Sales;  
GO  
  
INSERT INTO Sales VALUES (7, 1, 'Seat', 12); -- error: blocked from inserting row for the wrong user ID  
GO  
  
REVERT;  
GO  
```

Очистите ресурсы базы данных.

```sql
DROP USER AppUser;

DROP SECURITY POLICY Security.SalesFilter;
DROP TABLE Sales;
DROP FUNCTION Security.fn_securitypredicate;
DROP SCHEMA Security;
```

### <a name="d-scenario-for-using-a-lookup-table-for-the-security-predicate"></a><a name="Lookup"></a> Г. Сценарий применения таблицы подстановки для предиката безопасности

В этом примере таблица подстановки применяется для связи между идентификатором пользователя и фильтруемым значением, чтобы не пришлось указывать сам идентификатор пользователя в таблице фактов. Здесь создаются три пользователя, а также заполняется таблица фактов с шестью строками и таблица подстановки с двумя строками. Затем создается встроенная функция с табличным значением, которая объединяет таблицы фактов и подстановки для получения идентификатора пользователя и политики безопасности для таблицы. Пример затем показывает, как фильтруются отдельные инструкции для разных пользователей.  
  
Создайте три учетные записи пользователей, демонстрирующие разные возможности доступа.  

```sql  
CREATE USER Manager WITHOUT LOGIN;  
CREATE USER Sales1 WITHOUT LOGIN;  
CREATE USER Sales2 WITHOUT LOGIN;  
```

Создайте пример схемы и таблицу фактов для хранения данных.  

```sql
CREATE SCHEMA Sample;

CREATE TABLE Sample.Sales  
    (  
    OrderID int,  
    Product varchar(10),  
    Qty int 
    );    
```

 Заполните таблицу фактов шестью строками данных.  

```sql
INSERT INTO Sample.Sales VALUES (1, 'Valve', 5);
INSERT INTO Sample.Sales VALUES (2, 'Wheel', 2);
INSERT INTO Sample.Sales VALUES (3, 'Valve', 4);
INSERT INTO Sample.Sales VALUES (4, 'Bracket', 2);
INSERT INTO Sample.Sales VALUES (5, 'Wheel', 5);
INSERT INTO Sample.Sales VALUES (6, 'Seat', 5);
-- View the 6 rows in the table  
SELECT * FROM Sample.Sales;
```

Создайте таблицу для хранения данных подстановки, в нашем примере — для связи между сущностями Salesrep и Product.  

```sql
CREATE TABLE Sample.Lk_Salesman_Product
  ( Salesrep sysname, 
    Product varchar(10)
  ) ;
```

 Внесите в таблицу подстановки пример данных, в которых товары сопоставляются с каждым из торговых представителей.  

```sql
INSERT INTO Sample.Lk_Salesman_Product VALUES ('Sales1', 'Valve');
INSERT INTO Sample.Lk_Salesman_Product VALUES ('Sales2', 'Wheel');
-- View the 2 rows in the table
SELECT * FROM Sample.Lk_Salesman_Product;
```

Предоставьте каждому из пользователей доступ на чтение к таблице фактов.  

```sql
GRANT SELECT ON Sample.Sales TO Manager;  
GRANT SELECT ON Sample.Sales TO Sales1;  
GRANT SELECT ON Sample.Sales TO Sales2;  
```

Создайте новую схему и встроенную функцию с табличным значением. Эта функция возвращает 1, если пользователь запрашивает таблицу фактов Sales, а значение в столбце SalesRep таблицы Lk_Salesman_Product совпадает с идентификатором пользователя, выполняющего запрос (`@SalesRep = USER_NAME()`) при объединении с таблицей фактов по столбцу Product, если выполняющий запрос пользователь является менеджером (`USER_NAME() = 'Manager'`).

```sql
CREATE SCHEMA Security ;

CREATE FUNCTION Security.fn_securitypredicate
         (@Product AS varchar(10))
RETURNS TABLE
WITH SCHEMABINDING
AS 
           RETURN ( SELECT 1 as Result
                     FROM Sample.Sales f
            INNER JOIN Sample.Lk_Salesman_Product s
                     ON s.Product = f.Product
            WHERE ( f.product = @Product
                    AND s.SalesRep = USER_NAME() )
                 OR USER_NAME() = 'Manager'
                   ) ;
 
```

Создайте политику безопасности, добавляя функцию в качестве предиката фильтра. Состоянию должно быть присвоено значение ON для включения политики.

```sql
CREATE SECURITY POLICY SalesFilter 
ADD FILTER PREDICATE Security.fn_securitypredicate(Product)
ON Sample.Sales
WITH (STATE = ON) ;
```

Дайте разрешение на SELECT функции fn_securitypredicate 
```sql
GRANT SELECT ON security.fn_securitypredicate TO Manager;  
GRANT SELECT ON security.fn_securitypredicate TO Sales1;  
GRANT SELECT ON security.fn_securitypredicate TO Sales2;  
```

Теперь протестируйте предикат фильтрации при выборе из таблицы Sales, как для каждого пользователя.

```sql
EXECUTE AS USER = 'Sales1'; 
SELECT * FROM Sample.Sales;
-- This will return just the rows for Product 'Valve' (as specified for ‘Sales1’ in the Lk_Salesman_Product table above)
REVERT;

EXECUTE AS USER = 'Sales2'; 
SELECT * FROM Sample.Sales;
-- This will return just the rows for Product 'Wheel' (as specified for ‘Sales2’ in the Lk_Salesman_Product table above)
REVERT; 

EXECUTE AS USER = 'Manager'; 
SELECT * FROM Sample.Sales;
-- This will return all rows with no restrictions
REVERT;
```

Пользователь Manager должен видеть все шесть строк. Пользователи Sales1 и Sales2 должны видеть только свои продажи.

Измените политику безопасности, чтобы отключить политику.

```sql
ALTER SECURITY POLICY SalesFilter  
WITH (STATE = OFF);  
```

Теперь пользователи Sales1 и Sales2 могут видеть все шесть строк.

Подключение к базе данных SQL для очистки ресурсов

```sql
DROP USER Sales1;
DROP USER Sales2;
DROP USER Manager;

DROP SECURITY POLICY SalesFilter;
DROP FUNCTION Security.fn_securitypredicate;
DROP TABLE Sample.Sales;
DROP TABLE Sample.Lk_Salesman_Product;
DROP SCHEMA Security; 
DROP SCHEMA Sample;
```

## <a name="see-also"></a>См. также:

 [CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)</br>
 [ALTER SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-security-policy-transact-sql.md)</br>
 [DROP SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-security-policy-transact-sql.md)</br>
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)</br>
 [SESSION_CONTEXT (Transact-SQL)](../../t-sql/functions/session-context-transact-sql.md)</br>
 [sp_set_session_context (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)</br>
 [sys.security_policies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)</br>
 [sys.security_predicates (Transact-SQL)](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)</br>
 [Создание определяемых пользователем функций (компонент Database Engine)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md)

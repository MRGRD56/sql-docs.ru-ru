---
title: Должна ли таблица или хранимая процедура быть перенесена в выполняющуюся в памяти OLTP
description: Используйте отчет об анализе производительности транзакций в SQL Server Management Studio для оценки, можно ли улучшить производительность приложения базы данных с помощью выполняющейся в памяти OLTP.
ms.custom: seo-dt-2019
ms.date: 08/02/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- Analyze, Migrate, Report
- AMR
ms.assetid: c1ef96f1-290d-4952-8369-2f49f27afee2
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8a6c0e0e9db4e04061691cf331323a4df34f432
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492334"
---
# <a name="determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp"></a>Определение, должна ли таблица или хранимая процедура быть перенесена в In-Memory OLTP
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Отчет об анализе производительности транзакции в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] позволяет оценить, улучшится ли производительность приложения базы данных с помощью выполняющейся в памяти OLTP. В отчете также показано, сколько работы необходимо выполнить, чтобы включить выполняющуюся в памяти OLTP в приложении. После определения дисковой таблицы, которая переносится в In-Memory OLTP, можно для упрощения миграции таблицы использовать [советник по оптимизации для выполнения в памяти](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md). Аналогичным образом [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md) позволяет перенести хранимую процедуру в изначально скомпилированную хранимую процедуру. Дополнительные сведения о методологиях миграции см. в разделе [Выполняемая в памяти OLTP — стандартные шаблоны рабочей нагрузки и вопросы миграции](/previous-versions/dn673538(v=msdn.10)).  
  
 Отчет об анализе производительности транзакции выполняется непосредственно через рабочую базу данных или тестовую базу данных с активной рабочей нагрузкой, которая аналогична рабочей нагрузке.  
  
 С помощью отчета и помощников миграции можно выполнять следующие задачи:  
  
-   Анализ рабочей нагрузки, позволяющий определить самые перегруженные участки, производительность которых может улучшить выполняющаяся в памяти OLTP. В отчете анализа производительности транзакций приводятся таблицы и хранимые процедуры, для которых преобразование в выполняющуюся в памяти OLTP будет наиболее полезным.  
  
-   Помощь с планированием и выполнением миграции в OLTP в памяти. Миграция из дисковой таблицы в оптимизированную для памяти таблицу может занять много времени. Помощник по оптимизации для памяти позволит определить несовместимые компоненты в таблице, которые необходимо удалить до перемещения таблицы в In-Memory OLTP. Помощник также позволяет понять последствия переноса таблицы в оптимизированную для памяти таблицу для приложения.  
  
     Можно проверить, будет ли применение In-Memory OLTP полезно для приложения, если потребуется спланировать миграцию в In-Memory OLTP или перенести некоторые таблицы и хранимые процедуры в In-Memory OLTP.  
  
    > [!IMPORTANT]  
    >  Производительность системы базы данных зависит от многих факторов, которые не все могут отслеживаться и измеряться сборщиком данных о производительности транзакции. Поэтому отчет анализа производительности транзакции не гарантирует, что фактическое улучшение производительности будет соответствовать прогнозируемому, если были сделаны какие-либо прогнозы.  
  
 Отчет об анализе производительности транзакции и помощники миграции устанавливаются в составе SQL Server Management Studio (SSMS) при выборе **Средства управления — основные** или **Средства управления — расширенные** при установке [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] либо при [скачивании SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md).    
  
## <a name="transaction-performance-analysis-reports"></a>Отчеты об анализе производительности транзакции  
 Отчеты об анализе производительности транзакции можно создать в **обозревателе объектов** , щелкнув правой кнопкой мыши базу данных и выбрав пункт **Отчеты**, затем — **Стандартные отчеты** и **Обзор анализа производительности транзакций**. Для создания эффективного отчета об анализе у базы данных должна быть активная или недавно выполненная рабочая нагрузка.  
  
### <a name="tables"></a>Таблицы
  
 Подробный отчет для таблицы состоит из трех разделов.  
  
-   Раздел статистики сканирования  
  
     Этот раздел содержит одну таблицу с собранными статистическими данными о сканированиях таблицы базы данных. Это следующие столбцы:  
  
    -   Процент от общего количества обращений. Процент операций сканирования и поиска в этой таблице от активности всей базы данных. Чем выше это значение, тем интенсивнее эта таблица использовалась по сравнению с другими таблицами в базе данных.  
  
    -   Статистика поиска или статистика сканирования диапазона. В этом столбце указывается число уточняющих запросов и сканирований диапазона (индекса и таблиц), выполненных для таблицы во время профилирования. Среднее значение для одной транзакции является приблизительным.  
    
-   Раздел статистики блокировки  
  
     В этом разделе представлена таблица, в которой отображаются блокировки для таблицы базы данных. Дополнительные сведения о блокировках и кратковременных блокировках базы данных см. в разделе "Архитектура блокировок". Существуют следующие столбцы.  
  
    -   Процент от общего ожидания. Процент ожидания блокировок и кратковременных блокировок в этой таблице по сравнению с активностью базы данных. Чем выше это значение, тем интенсивнее эта таблица использовалась по сравнению с другими таблицами в базе данных.  
  
    -   Статистика кратковременных блокировок. В этих столбцах указывается число ожиданий кратковременных блокировок для запросов к этой таблице. Сведения о кратковременных блокировках см. в разделе "Кратковременные блокировки". Чем больше это значение, тем больше конфликтов кратковременных блокировок в таблице.  
  
    -   Статистика блокировок. В этой группе столбцов указывается число получений и ожиданий блокировок для запросов к этой таблице. Дополнительные сведения о блокировках см. в разделе "Основные сведения о блокировках в SQL Server". Чем больше ожиданий, тем больше конфликтов блокировок в таблице.  
  
-   Раздел трудностей с миграцией  
  
     Этот раздел содержит таблицу со сведениями о трудностях, связанных с преобразованием этой таблицы базы данных в оптимизированную для памяти таблицу. Чем выше оценка трудности, тем сложнее преобразовать таблицу. Чтобы просмотреть сведения о преобразовании этой таблицы базы данных, используйте помощник по оптимизации памяти.  
  
Статистика сканирования и конфликтов в отчете по таблице собирается и объединяется из sys.dm_db_index_operational_stats (Transact-SQL).  

### <a name="stored-procedures"></a>Хранимые процедуры

 Хранимая процедура с высоким коэффициентом расхода времени ЦП — кандидат для миграции. В отчете отображаются все ссылки на таблицу, поскольку скомпилированные хранимые процедуры могут ссылаться только на таблицы, оптимизированные для памяти, что может увеличить расходы времени при миграции.  
  
 Подробный отчет для хранимой процедуры состоит из двух разделов.  
  
-   Раздел статистики выполнения  
  
     Этот раздел содержит таблицу с собранными статистическими данными о выполнении хранимой процедуры. Существуют следующие столбцы.  
  
    -   Время кэширования. Время, в течение которого план выполнения был кэширован. Если хранимая процедура выходит из кэша плана и заново в него входит, время будет указано для каждого кэша.  
  
    -   Всего времени ЦП. Общее время ЦП, затраченное хранимой процедурой во время профилирования. Чем больше это значение, тем больше ресурсов ЦП использовала хранимая процедура.  
  
    -   Общее время выполнения. Общее время выполнения, затраченное на хранимую процедуру во время профилирования. Чем больше разница между этим значением и временем ЦП, тем менее эффективно хранимая процедура использует ЦП.  
  
    -   Всего промахов кэша. Число промахов кэша (операций чтения из физического хранилища), вызванное выполнением хранимой процедуры во время профилирования.  
  
    -   Число выполнений. Количество выполнений этой хранимой процедуры во время профилирования.  
  
-   Раздел ссылок на таблицы  
  
     Этот раздел содержит таблицу, в которой показаны таблицы, на которые ссылается эта хранимая процедура. Перед преобразованием хранимой процедуры в скомпилированную в собственном коде хранимую процедуру все эти таблицы должны быть преобразованы в оптимизированные для памяти таблицы, при этом они должны находиться на одном сервере и в одной базе данных.  
  
 Статистика выполнения в подробном отчете по хранимой процедуре собирается и объединяется из sys.dm_exec_procedure_stats (Transact-SQL). Ссылки извлекаются из sys.sql_expression_dependencies (Transact-SQL).  
  
 Чтобы отобразить сведения о преобразовании хранимой процедуры в хранимую процедуру, скомпилированную в собственном коде, используйте помощник по компиляции в собственный код.  
  
## <a name="generating-in-memory-oltp-migration-checklists"></a>Создание контрольных списков миграции выполняющейся в памяти OLTP  
 Контрольные списки миграции позволяют определить любые функции таблицы или хранимых процедур, которые не поддерживаются с оптимизированными для памяти таблицами или скомпилированными в собственном коде хранимыми процедурами. С помощью помощников по оптимизации памяти и компиляции в собственный код можно создать контрольный список для одной таблицы на диске или интерпретируемой хранимой процедуры T-SQL. Кроме того, можно создать контрольные списки миграции для нескольких таблиц и хранимых процедур в базе данных.  
  
 Создать контрольный список миграции в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] можно, используя команду **Создать контрольные списки миграции выполняющейся в памяти OLTP** или PowerShell.  
  
**Создание контрольного списка миграции с помощью команды пользовательского интерфейса**  
  
1.  В **обозревателе объектов** щелкните правой кнопкой мыши базу данных, отличную от системной базы данных, выберите пункт **Задачи**, а затем — **Создать контрольные списки миграции, выполняющейся в памяти OLTP**.  
  
2.  В диалоговом окне "Создание контрольных списков миграции, выполняющейся в памяти OLTP" нажмите кнопку "Далее", чтобы перейти на страницу **Настройка параметров создания контрольных списков** . На этой странице сделайте следующее:  
  
    1.  В поле **Сохранить контрольный список в** введите путь к папке.  
  
    2.  Выберите параметр **Создавать контрольные списки для указанных таблиц и хранимых процедур** .  
  
    3.  В поле раздела разверните узлы **Таблица** и **Хранимая процедура** .  
  
    4.  Выберите в списке несколько объектов.  
  
3.  Нажмите кнопку **Далее** и убедитесь, что список задач соответствует параметрам на странице **Настройка параметров создания контрольных списков** .  
  
4.  Нажмите кнопку **Готово**, а затем убедитесь, что отчеты контрольных списков миграции созданы только для выбранных объектов.  

 Точность отчетов можно проверить, сравнив их с отчетами, созданными с помощью помощника по оптимизации памяти и помощника по собственной компиляции. Дополнительные сведения см. в разделах [Memory Optimization Advisor](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) и [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md).  
  
**Создание контрольного списка миграции с помощью SQL Server PowerShell**  
  
1.  В **обозревателе объектов** щелкните базу данных и выберите команду **Запустить PowerShell**. Должно отобразиться следующее сообщение:  
  
    ```  
    PS SQLSERVER: \SQL\{Instance Name}\DEFAULT\Databases\{two-part DB Name}>  
    ```  
  
2.  Введите следующую команду.  
  
    ```  
    Save-SqlMigrationReport -FolderPath "<folder_path>"  
    ```  
  
3.  Убедитесь, что выполняются следующие условия.  
  
    -   Создается путь к папке, если он еще не существует.  
  
    -   Отчет о миграции контрольного списка создается для всех таблиц и хранимых процедур в базе данных и сохраняется в расположении, указанном с помощью параметра folder_path.  
  
**Создание контрольного списка миграции с помощью Windows PowerShell**  
  
1.  Запустите сеанс Windows PowerShell с повышенными правами.  
  
2.  Введите команды, указанные ниже. Объектом может быть таблица или хранимая процедура.  
  
    ```  
    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SMO')  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -FolderPath "<folder_path1>"  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -Object <object_name> -FolderPath "<folder_path2>"  
  
    ```  
  
3.  Убедитесь, что выполняются следующие условия.  
  
    -   Отчет о миграции контрольного списка создается для всех таблиц и хранимых процедур в базе данных и сохраняется в расположении, указанном с помощью параметра folder_path.  
  
    -   Отчет о миграции контрольного списка для объекта, заданного с помощью параметра <object_name>, сохраняется в расположении, указанном с помощью параметра folder_path2.  
  
## <a name="see-also"></a>См. также:  
 [Миграция в In-Memory OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  

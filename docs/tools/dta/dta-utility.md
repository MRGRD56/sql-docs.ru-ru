---
title: dta
description: Программа dta — это версия командной строки для помощника по настройке ядра СУБД, которая позволяет использовать его функции в приложениях и скриптах.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- physical design structures [SQL Server]
- command prompt utilities [SQL Server], dta
- dta utility
- tuning databases [SQL Server], Database Engine Tuning Advisor
- workloads [SQL Server], analyzing
- dta utility, about dta utility
- performance [SQL Server], Database Engine Tuning Advisor
- Database Engine Tuning Advisor [SQL Server], command prompt
- optimizing databases [SQL Server]
ms.assetid: a0b210ce-9b58-4709-80cb-9363b68a1f5a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/09/2017
ms.openlocfilehash: ff53b6711375e5f06368333392da33bb0f5108b9
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890748"
---
# <a name="dta-utility"></a>dta

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Программа **dta** — это версия помощника по настройке ядра СУБД для командной строки. Программа **dta** предназначена для использования функций помощника по настройке ядра СУБД в приложениях и скриптах.  

Как и помощник по настройке ядра СУБД, программа **dta** производит анализ рабочей нагрузки и выдает рекомендации по изменению структур физического проектирования для улучшения производительности сервера при данной рабочей нагрузке. Рабочей нагрузкой может выступать кэш планов, файл трассировки или таблица  приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] либо скрипт [!INCLUDE[tsql](../../includes/tsql-md.md)] . Структуры физического проектирования включают в себя индексы, индексированные представления и секционирование. После проведения анализа рабочей нагрузки программа **dta** выдает рекомендации по изменению физической структуры баз данных и может сформировать необходимый скрипт для внесения этих изменений. Рабочие нагрузки можно указать из командной строки с помощью аргумента **-if** или **-it** . Кроме того, можно указать из командной строки входной XML-файл с помощью аргумента **-ix** . В этом случае рабочая нагрузка указывается во входном XML-файле.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
dta  
[ -? ] |  
[  
      [ -S server_name[ \instance ] ]  
      { { -U login_id [-P password ] } | -E  }  
      { -D database_name [ ,...n ] }  
      [ -d database_name ]   
      [ -Tl table_list | -Tf table_list_file ]  
      { -if workload_file | -it workload_trace_table_name  |   
        -ip | -iq }  
      { -ssession_name | -IDsession_ID }  
      [ -F ]  
      [ -of output_script_file_name ]  
      [ -or output_xml_report_file_name ]  
      [ -ox output_XML_file_name ]  
      [ -rl analysis_report_list [ ,...n ] ]  
      [ -ix input_XML_file_name ]  
      [ -A time_for_tuning_in_minutes ]  
      [ -n number_of_events ]
      [ -l time_window_in_hours ]  
      [ -m minimum_improvement ]  
      [ -fa physical_design_structures_to_add ]  
      [ -fi filtered_indexes]  
      [ -fc columnstore_indexes]  
      [ -fp partitioning_strategy ]  
      [ -fk keep_existing_option ]  
      [ -fx drop_only_mode ]  
      [ -B storage_size ]  
      [ -c max_key_columns_in_index ]  
      [ -C max_columns_in_index ]  
      [ -e | -e tuning_log_name ]  
      [ -N online_option]  
      [ -q ]  
      [ -u ]  
      [ -x ]  
      [ -a ]  
]  
```  
  
## <a name="arguments"></a>Аргументы  
 **-?**  
 Отображает сведения об использовании.  
  
 **-A** _time_for_tuning_in_minutes_  
 Задает предел времени настройки в минутах. Программа **dta** использует указанное время для настройки рабочей нагрузки и создания скрипта рекомендованных изменений физических структур. По умолчанию программа **dta** предполагает время настройки, равное 8 часам. Задание нулевого значения разрешает неограниченное время настройки. Программа **dta** должна закончить настройку всей рабочей нагрузки до истечения предела времени. Тем не менее, чтобы обеспечить настройку всей рабочей нагрузки, рекомендуется установить неограниченное время настройки (-A 0).  
  
 **-a**  
 Производит настройку рабочей нагрузки и применяет рекомендации без запроса на подтверждение.  
  
 **-B** _storage_size_  
 Задает максимальный размер пространства в мегабайтах, которое может быть использовано при рекомендованном индексировании и секционировании. При настройке нескольких баз данных учитываются рекомендации по пространству всех баз данных. По умолчанию программа **dta** предполагает использование наименьшего размера хранилищ из следующих:  
  
-   троекратный текущий размер необработанных данных, включая общий размер куч и кластеризованных индексов в таблицах базы данных;  
  
-   свободное место на всех присоединенных дисках плюс размер необработанных данных.  
  
 В размере хранилища по умолчанию не учитываются некластеризованные индексы и индексированные представления.  
  
 **-C** _max_columns_in_index_  
 Задает максимальное количество столбцов в индексах, предлагаемых программой **dta** . Максимальное значение равно 1024. По умолчанию значение этого аргумента равно 16.  
  
 **-c** _max_key_columns_in_index_  
 Задает максимальное количество ключевых столбцов в индексах, предлагаемых программой **dta** . Значение по умолчанию равно 16 — максимально допустимое значение. Кроме того, программа **dta** учитывает создание индексов с включенными столбцами. Количество рекомендованных индексов с включенными столбцами может превысить количество столбцов, заданное в этом аргументе.  
  
 **-D** _database_name_  
 Задает имя каждой настраиваемой базы данных. Первой базой данных является база данных по умолчанию. Можно указать несколько баз данных, разделив их имена запятыми, например:  
  
```  
dta -D database_name1, database_name2...  
```  
  
 Кроме того, можно указать несколько баз данных с помощью аргумента **-D** для каждой базы данных, например:  
  
```  
dta -D database_name1 -D database_name2... n  
```  
  
 Аргумент **-D** является обязательным. Если аргумент **-d** указан не был, программа **dta** сначала производит подключение к базе данных, указанной с помощью первого предложения `USE database_name` в рабочей нагрузке. В случае отсутствия явного предложения `USE database_name` в рабочей нагрузке необходимо указать аргумент **-d** .  
  
 Например, если имеется рабочая нагрузка, не содержащая явного предложения `USE database_name` , и используется следующая команда **dta** , то рекомендация сформирована не будет:  
  
```  
dta -D db_name1, db_name2...  
```  
  
 Однако если использовать ту же рабочую нагрузку и следующую команду **dta** с аргументом **-d** , то рекомендация будет сформирована:  
  
```  
dta -D db_name1, db_name2 -d db_name1  
```  
  
 **-d** _database_name_  
 Задает первую базу данных, к которой программа **dta** подключается при настройке рабочей нагрузки. Для этого аргумента может быть задана только одна база данных. Пример:  
  
```  
dta -d AdventureWorks2012 ...  
```  
  
 В случае указания нескольких баз данных программа **dta** возвращает ошибку. Аргумент **-d** является необязательным.  
  
 При использовании входного XML-файла можно задать первую базу данных, к которой программа **dta** производит подключение, используя элемент **DatabaseToConnect** , расположенный в элементе **TuningOptions** . Дополнительные сведения см. в разделе [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md).  
  
 При настройке только одной базы данных аргумент **-d** предоставляет функциональность, аналогичную аргументу **-d** в программе **sqlcmd** , но не выполняет инструкцию USE *database_name* . Дополнительные сведения см. в статье [sqlcmd Utility](../../tools/sqlcmd-utility.md).  
  
 **-E**  
 Использует доверительное соединение вместо запроса пароля. Необходимо указать либо аргумент **-E** , либо аргумент **-U** , задающий идентификатор входа.  
  
 **-e** _tuning_log_name_  
 Задает имя таблицы или файла, куда программа **dta** записывает события, которые ей не удалось настроить. Таблица создается на сервере, где производится настройка.  
  
 В случае использования таблицы задавайте ее имя в следующем формате: *[database_name].[owner_name].table_name*. Следующая таблица показывает значения по умолчанию для каждого параметра.  
  
|Параметр|Значение по умолчанию|Сведения|  
|---------------|-------------------|-------------|  
|*database_name*|*database_name*, заданное с параметром **–D**||  
|*owner_name*|**dbo**|*owner_name* должно быть **dbo**. Если указано любое другое значение, выполнение программы **dta** прервется и она вернет ошибку.|  
|*table_name*|None||  
  
 В случае использования файла следует указать XML в качестве его расширения. Например TuningLog.xml.  
  
> [!NOTE]  
>  Программа **dta** не производит удаление содержимого пользовательских таблиц журнала настройки в случае удаления сеанса. При настройке очень больших рабочих нагрузок рекомендуется задать таблицу для журнала настройки. Поскольку настройка больших рабочих нагрузок может привести к созданию больших журналов настройки, сеансы при использовании таблицы могут быть удалены гораздо быстрее.  
  
 **-F**  
 Разрешает программе **dta** перезапись существующего выходного файла. Если выходной файл с тем же именем уже существует, а параметр **-F** не указан, программа **dta** возвращает ошибку. Можно использовать параметр **-F** с параметром **-of**, **-or** или **-ox**.  
  
 **-fa** _physical_design_structures_to_add_  
 Задает типы структур физического проектирования, которые программа **dta** должна включать в рекомендацию. В следующей таблице перечислены и описаны все значения, которые могут быть указаны для данного аргумента. Если значение не указано, программа **dta** использует значение по умолчанию **-fa IDX**.  
  
|Значение|Описание|  
|-----------|-----------------|  
|IDX_IV|Индексы и индексированные представления.|  
|IDX|Только индексы.|  
|IV|Только индексированные представления.|  
|NCL_IDX|Только некластеризованные индексы.|  
  
 **-fi**  
 Указывает, что необходимо рассмотреть новые рекомендации для отфильтрованных индексах. Дополнительные сведения см. в разделе [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
**-fc**  
 Указывает, что индексы columnstore следует учитывать для новых рекомендаций. DTA поддерживает как кластеризованные, так и некластеризованные индексы columnstore. Дополнительные сведения см. в разделе    
[Рекомендации по индексам сolumnstore в помощнике по настройке ядра СУБД (DTA)](../../relational-databases/performance/columnstore-index-recommendations-in-database-engine-tuning-advisor-dta.md).

> **Область применения**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] и более поздних версий.  

  
 **-fk** _keep_existing_option_  
 Указывает, какие существующие структуры физического проектирования программа **dta** должна сохранить при формировании рекомендации. В следующей таблице перечислены и описаны все значения, которые могут быть указаны для данного аргумента.  
  
|Значение|Описание|  
|-----------|-----------------|  
|None|Никакие из существующих структур|  
|ALL|Все существующие структуры|  
|ALIGNED|Все структуры, выровненные по секциям.|  
|CL_IDX|Все кластеризованные индексы в таблицах|  
|IDX|Все кластеризованные и некластеризованные индексы в таблицах|  
  
 **-fp** _partitioning_strategy_  
 Определяет, будут ли секционироваться новые структуры физического проектирования (индексы и индексированные представления), предлагаемые программой **dta** , а также указывает способ их секционирования. В следующей таблице перечислены и описаны все значения, которые могут быть указаны для данного аргумента.  
  
|Значение|Описание|  
|-----------|-----------------|  
|None|Без секционирования|  
|FULL|Полное секционирование (выберите для улучшения производительности)|  
|ALIGNED|Только выровненное секционирование (выберите для улучшения возможностей управления)|  
  
 Значение ALIGNED означает, что в рекомендации, созданной программой **dta** , каждый предлагаемый индекс секционируется точно таким же образом, что и базовая таблица, для которой определяется индекс. Некластеризованные индексы в индексированном представлении выравниваются по индексированному представлению. Для этого аргумента может быть задано только одно значение. Значение по умолчанию — **-fp NONE**.  
  
 **-fx** _drop_only_mode_  
 Указывает, что программа **dta** рассматривает только удаление существующих структур физического проектирования. Новые структуры физического проектирования не рассматриваются. При указании этого параметра программа **dta** производит оценку полезности существующих структур физического проектирования и рекомендует удалить редко используемые структуры. Этот аргумент не принимает значений. Он не может быть использован с аргументами **-fa**, **-fp** или **-fk ALL**  
  
 **-ID** _session_ID_  
 Задает числовой идентификатор сеанса настройки. В случае его отсутствия программа **dta** формирует идентификатор. Можно использовать этот идентификатор для просмотра сведений о существующих сеансах настройки. Если значение для **-ID** не указано, то имя сеанса должно быть задано с помощью ключа **-s**.  
  
 **-ip**  
 Указывает, что в качестве рабочей нагрузки нужно использовать кэш планов. Анализируется верхняя тысяча событий кэша планов для явно выбранных баз данных. Это значение можно изменить с помощью параметра **–n**.  

**-iq**  
 Указывает, что в качестве рабочей нагрузки нужно использовать хранилище запросов. Анализируется первая тысяча событий из хранилища запросов для явно выбранных баз данных. Это значение можно изменить с помощью параметра **–n**.  Дополнительные сведения см. в статье [Хранилище запросов](../../relational-databases/performance/how-query-store-collects-data.md)[Настройка базы данных с помощью рабочей нагрузки из хранилища запросов](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md).
  
> **Область применения**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] и более поздних версий.
     
 **-if** _workload_file_  
 Задает путь и имя файла рабочей нагрузки, используемого в качестве файла исходных данных для настройки. Файл должен быть в одном из следующих форматов: TRC (трассировочный файл приложения SQL Server Profiler), SQL (файл SQL) или LOG (файл трассировки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Требуется указание либо одного файла рабочей нагрузки, либо одной таблицы рабочей нагрузки.  
  
 **-it** _workload_trace_table_name_  
 Задает имя таблицы, содержащей трассировку рабочей нагрузки для настройки. Имя задается в следующем формате: [*database_name*] **.** [*owner_name*] **.** _table_name_.  
  
 Следующая таблица показывает значения по умолчанию для каждого параметра.  
  
|Параметр|Значение по умолчанию|  
|---------------|-------------------|  
|*database_name*|*database_name*, заданное с параметром **–D**.|  
|*owner_name*|**dbo**.|  
|*table_name*|Нет.|  
  
> [!NOTE]  
>  *owner_name* должно быть **dbo**. Если указано любое другое значение, выполнение программы **dta** прервется и программа вернет ошибку. Также обратите внимание на то, что требуется указать либо один файл рабочей нагрузки, либо одну таблицу рабочей нагрузки.  
  
 **-ix** _input_XML_file_name_  
 Задает имя XML-файла, содержащего входные данные для программы **dta** . Файл должен представлять собой допустимый XML-документ, соответствующий DTASchema.xsd. Конфликтующие аргументы, указанные в командной строке для параметров настройки, переопределяют соответствующие значения в XML-файле. Единственное исключение имеет место в том случае, когда пользовательская конфигурация вводится во входном XML-файле в режиме оценки. Например, если конфигурация задана в элементе **Configuration** входного XML-файла и задан элемент **EvaluateConfiguration** в качестве одного из параметров настройки, то параметры настройки, заданные во входном XML-файле, переопределят любые параметры настройки, введенные в командной строке.  

 **-k** _maxtotalindexes_  
 Максимальное число индексов в рекомендации.  

 **-K** _maxtotalindexes_  
 Максимальное число индексов на одну таблицу.

 **-m** _minimum_improvement_  
 Задает минимальный процент улучшения, которому должна удовлетворять рекомендуемая конфигурация.  
  
 **-N** _online_option_  
 Указывает, создаются ли структуры физического проектирования при работе в режиме в сети. В следующей таблице перечислены и описаны все значения, которые могут быть указаны для данного аргумента.  
  
|Значение|Описание|  
|-----------|-----------------|  
|OFF|Рекомендованные структуры физического проектирования не могут быть созданы в оперативном режиме.|  
|ON|Все рекомендованные структуры физического проектирования могут быть созданы в режиме в сети.|  
|MIXED|Помощник по настройке ядра СУБД пытается рекомендовать структуры физического проектирования, которые могут быть созданы, по возможности, в режиме в сети.|  
  
 Если индексы создаются в режиме в сети, то к определению объекта добавляется ONLINE = ON.  
  
 **-n** _number_of_events_  
 Задает количество событий в рабочей нагрузке, которые должны быть настроены программой **dta** . Если этот аргумент указан, а рабочей нагрузкой является файл трассировки, содержащий сведения о продолжительности, то программа **dta** производит настройку событий в порядке убывания продолжительности. Этот аргумент полезен для сравнения двух конфигураций структур физического проектирования. Для сравнения двух конфигураций укажите одинаковое количество событий для настройки обеих конфигураций, а затем задайте неограниченное время настройки для обеих конфигураций, как указано далее:  
  
```  
dta -n number_of_events -A 0  
```  
  
 В этом случае важно указать неограниченное время настройки (`-A 0`). В противном случае помощник по настройке ядра СУБД примет для времени настройки значение по умолчанию, равное 8 часам.
 
 **-l** _time_window_in_hours_   
   Указывает интервал (в часах), запросы из которого помощник по настройке ядра СУБД будет рассматривать для настройки при использовании параметра **-iq** (рабочая нагрузка из хранилища запросов).
   
```  
dta -iq -l 48  
```  

В этом примере помощник по настройке будет использовать хранилище запросов в качестве источника рабочей нагрузки и учитывать только те запросы, которые были выполнены за последние 48 часов.  

> **Область применения**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] и более поздних версий.  

 
 **-of** _output_script_file_name_  
 Указывает, что программа **dta** производит запись рекомендаций в виде скрипта [!INCLUDE[tsql](../../includes/tsql-md.md)] в файл с указанным именем и расположением.  
  
 С этим параметром можно использовать параметр **-F** . Убедитесь в уникальности имени файла, особенно если используются параметры **-or** и **-ox**.  
  
 **-or** _output_xml_report_file_name_  
 Указывает, что программа **dta** производит запись рекомендаций в выходной отчет в формате XML. Если указано имя файла, то запись рекомендаций происходит в указанное назначение. В противном случае программа **dta** использует имя сеанса для создания имени файла и записывает его в текущий каталог.  
  
 С этим параметром можно использовать параметр **-F** . Убедитесь в уникальности имени файла, особенно если используются параметры **-of** и **-ox**.  
  
 **-ox** _output_XML_file_name_  
 Указывает, что программа **dta** производит запись рекомендаций в виде XML-файла с указанным именем и расположением. Убедитесь, что помощник по настройке ядра СУБД имеет разрешение на запись в целевой каталог.  
  
 С этим параметром можно использовать параметр **-F** . Убедитесь в уникальности имени файла, особенно если используются параметры **-of** и **-or**.  
  
 **-P** _password_  
 Указывает пароль для идентификатора имени входа. Если данный параметр не указан, программа **dta** запрашивает пароль.  
  
 **-q**  
 Устанавливает тихий режим. Никакие сведения не выводятся на консоль, включая сведения о ходе выполнения и сведения в заголовке.  
  
 **-rl** _analysis_report_list_  
 Указывает создаваемый список аналитических отчетов. В следующей таблице перечислены значения, которые могут быть указаны для этого аргумента.  
  
|Значение|Report|  
|-----------|------------|  
|ALL|Все аналитические отчеты|  
|STMT_COST|Отчет о стоимости инструкции|  
|EVT_FREQ|Отчет о повторяемости событий|  
|STMT_DET|Подробный отчет об инструкциях|  
|CUR_STMT_IDX|Отчет о связях «инструкция-индекс» (текущая конфигурация)|  
|REC_STMT_IDX|Отчет о связях «инструкция-индекс» (рекомендуемая конфигурация)|  
|STMT_COSTRANGE|Отчет о диапазоне стоимости инструкции|  
|CUR_IDX_USAGE|Отчет об использовании индексов (текущая конфигурация)|  
|REC_IDX_USAGE|Отчет об использовании индексов (рекомендуемая конфигурация)|  
|CUR_IDX_DET|Подробный отчет об индексах (текущая конфигурация)|  
|REC_IDX_DET|Подробный отчет об индексах (рекомендуемая конфигурация)|  
|VIW_TAB|Отчет о связях «представление-таблица»|  
|WKLD_ANL|Отчет об анализе рабочей нагрузки|  
|DB_ACCESS|Отчет о доступе к базе данных|  
|TAB_ACCESS|Отчет о доступе к таблицам|  
|COL_ACCESS|Отчет о доступе к столбцам|  
  
 Указывайте несколько отчетов, разделяя значения запятыми, например:  
  
```  
... -rl EVT_FREQ, VIW_TAB, WKLD_ANL ...  
```  
  
 **-S** _server_name_[ *\instance*]  
 Задает имя компьютера и экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для подключения. Если аргумент *server_name* не указан, **dta** подключается к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию на локальном компьютере. Этот параметр требуется при соединении с именованным экземпляром или при запуске программы **dta** с удаленного компьютера в сети.  
  
 **-s** _session_name_  
 Задает имя сеанса настройки. Этот параметр обязателен в случае отсутствия параметра **-ID** .  
  
 **-Tf** _table_list_file_  
 Задает имя файла, содержащего список настраиваемых таблиц. Каждая таблица, перечисленная в файле, должна начинаться на новой строке. Имена таблиц должны быть трехкомпонентными, например **AdventureWorks2012.HumanResources.Department**. При необходимости для вызова функции масштабирования таблицы за именем существующей таблицы может следовать число, указывающее предполагаемое количество строк в таблице. Помощник по настройке ядра СУБД учитывает количество предполагаемых строк при настройке или оценке инструкций в рабочей нагрузке, которая ссылается на эти таблицы. Обратите внимание, что между числом строк *number_of_rows* и *table_name* может быть один или более пробелов.  
  
 Формат файла для *table_list_file*:  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 Этот аргумент является альтернативой вводу списка таблиц в командной строке ( **-Tl**). Не указывайте файл со списком таблиц ( **-Tf**) в случае использования параметра **-Tl**. В случае указания обоих аргументов программа **dta** прерывает работу и возвращает ошибку.  
  
 В случае отсутствия аргументов **-Tf** и **-Tl** все пользовательские таблицы указанной базы данных рассматриваются как подлежащие настройке.  
  
 **-Tl** _table_list_  
 Задает список настраиваемых таблиц в командной строке. Используйте запятую в качестве разделителя имен таблиц. Если с аргументом **-D** указана только одна база данных, указывать имя базы данных в именах таблиц не обязательно. В противном случае полное имя в формате *database_name.schema_name.table_name* требуется для каждой таблицы.  
  
 Этот аргумент является альтернативой использованию файла со списком таблиц ( **-Tf**). В случае указания обоих аргументов, **-Tl** и **-Tf** , программа **dta** прерывает работу и возвращает ошибку.  
  
 **-U** _login_id_  
 Указывает идентификатор входа, используемый для соединения с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **-u**  
 Запускает графический интерфейс помощника по настройке ядра СУБД. Все параметры рассматриваются в качестве начальных установок пользовательского интерфейса.  
  
 **-x**  
 Производит запуск сеанса настройки и завершает работу.  
  
## <a name="remarks"></a>Remarks  
 Нажмите один раз сочетание клавиш CTRL + C для прерывания сеанса настройки и создания рекомендаций на основе анализа, выполненного программой **dta** до момента остановки. Будет выведен запрос о создании рекомендаций. Чтобы прервать сеанс настройки без формирования рекомендаций, еще раз нажмите сочетание клавиш CTRL+C.  
  
## <a name="examples"></a>Примеры  
 **А. Настройка рабочей нагрузки, которая содержит в рекомендации индексы и индексированные представления**  
  
 В этом примере используется безопасное соединение (`-E`) для соединения с базой данных **tpcd1G** на сервере MyServer для проведения анализа рабочей нагрузки и формирования рекомендаций. Выполняется запись выхода в файл скрипта с именем script.sql. Если файл script.sql уже существует, программа **dta** перезапишет этот файл, поскольку указан аргумент `-F` . Сеанс настройки выполняется неограниченное время в целях обеспечения полноты анализа рабочей нагрузки (`-A 0`). Рекомендация должна обеспечивать минимум 5% улучшения (`-m 5`). Программа **dta** должна содержать в своей заключительной рекомендации (`-fa IDX_IV`) индексы и индексированные представления.  
  
```  
dta -S MyServer -E -D tpcd1G -if tpcd_22.sql -F -of script.sql -A 0 -m 5 -fa IDX_IV  
```  
  
 **Б. Ограничение использования диска**  
  
 В этом примере общий размер базы данных, которая содержит необработанные данные и дополнительные индексы, ограничивается размером в 3 гигабайта (ГБ) (`-B 3000`). Выходные данные направляются в файл «d:\result_dir\script1.sql». Время выполнения примера — не более 1 часа (`-A 60`).  
  
```  
dta -D tpcd1G -if tpcd_22.sql -B 3000 -of "d:\result_dir\script1.sql" -A 60  
```  
  
 **В. Ограничение количества настраиваемых запросов**  
  
 В этом примере количество запросов, читаемых из файла orders_wkld.sql, ограничивается десятью запросами (`-n 10`). Он выполняется в течение 15 минут (`-A 15`) либо до завершения работы. Чтобы убедиться в том, что все 10 запросов настроены, задайте неограниченное время настройки с помощью `-A 0`. Если важен фактор времени, то укажите соответствующий предел времени путем задания числа минут, доступных для настройки, с помощью аргумента `-A` , как показано в этом примере.  
  
```  
dta -D orders -if orders_wkld.sql -of script.sql -A 15 -n 10  
```  
  
 **Г. Настройка отдельных таблиц, перечисленных в файле**  

 Этот пример демонстрирует использование параметра *table_list_file* (аргумент **-Tf** ). Файл таблицы table_list.txt имеет следующее содержимое:  

```
AdventureWorks2012.Sales.Customer  100000  
AdventureWorks2012.Sales.Store  
AdventureWorks2012.Production.Product  2000000  
```

Содержимое файла table_list.txt указывает, что:  

- Настройке подлежат только таблицы **Customer**, **Store** и **Product** в базе данных.  
  
- Число строк в таблицах **Customer** и **Product** принимается равным 100 000 и 2 000 000 соответственно.  
  
- Число строк в таблице **Store** принимается равным текущему числу строк в таблице.  

    Обратите внимание, что между числом строк и предшествующим ему именем таблицы в *table_list_file* может быть один или более пробелов.  
    
    Время настройки равно двум часам (`-A 120`), а результаты записываются в XML-файл (`-ox XMLTune.xml`).  

``` 
dta -D pubs -if pubs_wkld.sql -ox XMLTune.xml -A 120 -Tf table_list.txt  
``` 

## <a name="see-also"></a>См. также:

- [Справочник по программе командной строки (Database Engine)](../../tools/command-prompt-utility-reference-database-engine.md)
- [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)

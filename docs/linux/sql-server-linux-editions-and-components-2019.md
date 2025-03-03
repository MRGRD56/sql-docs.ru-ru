---
description: Выпуски и поддерживаемые функции SQL Server 2019 на Linux
title: Выпуски и поддерживаемые функции SQL Server 2019 на Linux
ms.date: 01/08/2020
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- default components
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: 5211d0b1ce2a20643c2256e5e86f97eeec214171
ms.sourcegitcommit: 2db7412d30722f198cbafcd683bd4da206b33996
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106100007"
---
# <a name="editions-and-supported-features-of-sql-server-2019-on-linux"></a>Выпуски и поддерживаемые функции SQL Server 2019 на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

В этой статье подробно описаны функции, поддерживаемые различными выпусками SQL Server 2019 на Linux. Описание выпусков и поддерживаемых функций SQL Server в Windows см. в статье [SQL Server 2019 — Windows](../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
Требования для установки сильно зависят от потребностей приложения. Различные выпуски [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] удовлетворяют индивидуальным требованиям каждой организации или отдельного лица к производительности, среде выполнения и цене. Набор устанавливаемых компонентов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] зависит от потребностей конкретного пользователя. В следующих разделах содержатся сведения, на основе которых из множества выпусков и компонентов, доступных в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], можно сделать наилучший выбор.  

Актуальные заметки о выпуске и сведения о новых возможностях содержатся в следующих разделах:
- [Заметки о выпуске SQL Server 2019 на Linux](sql-server-linux-release-notes-2019.md)
- [Новые возможности SQL Server 2019 на Linux](sql-server-linux-whats-new-2019.md)

Список функций SQL Server, которые недоступны в Linux, см. в статье [Неподдерживаемые функции и службы](#Unsupported).

### <a name="try-sql-server"></a>Попробуйте SQL Server!    
    
[Загрузить SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)

## <a name="ssnoversion-editions"></a>[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] , выпуски  
 Эти выпуски [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]описаны в следующей таблице. 
  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] edition|Определение|  
|---------------------------------------|----------------|  
|Enterprise|Выпуск [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition является предложением класса "Премиум", обеспечивающим полный набор возможностей ЦОД с исключительно высокой производительностью, что позволяет добиться высокого уровня обслуживания важнейших рабочих нагрузок.|  
|Standard|Выпуск [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard обеспечивает основные функции управления данными для приложений, работающих в отделах и небольших организациях. Поддерживаются распространенные средства разработки в локальных системах и вычислительных облаках, что делает возможным эффективное управление базами данных с минимальными затратами ИТ-ресурсов.|  
|Интернет|Выпуск[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition — это вариант с низкой совокупной стоимостью владения, предназначенный для размещения веб-сайтов и дополнительных веб-услуг, который по доступной цене обеспечивает масштабируемость и функции управления для небольших и крупномасштабных веб-проектов.|  
|Разработчик|Выпуск[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition позволяет разработчикам создавать приложения любого типа на базе [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Он включает все функциональные возможности выпуска Enterprise Edition, однако лицензируется как система для разработки и тестирования, а не для применения в качестве рабочего сервера. Выпуск [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer Edition является идеальным выбором для тех, кто создает и тестирует приложения.|  
|Express|Выпуск Express является бесплатной базой данных начального уровня и идеально подходит для обучения, а также для создания управляемых данными приложений, работающих на рабочих станциях и небольших серверах. Этот выпуск — лучший выбор для независимых поставщиков программного обеспечения, непрофессиональных разработчиков и любителей, создающих клиентские приложения. Если необходимы дополнительные функции базы данных, выпуск [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express можно легко обновить до версий [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]более высокого класса.|  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Использование [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с клиентскими и серверными приложениями  

На компьютер, где работают клиент-серверные приложения, которые подключаются непосредственно к экземпляру [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , можно установить только клиентские компоненты [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Установка клиентских компонентов будет хорошим выбором также и в том случае, если администрируется экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на сервере базы данных или планируется разработка приложений [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="ssnoversion-components"></a>составные части компонента [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

SQL Server 2019 на Linux поддерживает ядро СУБД SQL Server. В приведенной ниже таблице описаны функции ядра СУБД.   
  
|Компоненты сервера|Описание|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] включает компонент [!INCLUDE[ssDE](../includes/ssde-md.md)], основную службу для хранения, обработки и обеспечения безопасности данных, репликации, полнотекстового поиска, а также средства управления реляционными и XML-данными и возможности интеграции с системами аналитики данных.|  

**Выпуски Developer, Enterprise Core и Evaluation**  
Поддерживаемые компоненты для выпусков Developer, Enterprise Core и Evaluation указаны в списке возможностей SQL Server Enterprise в приведенных ниже таблицах.

Выпуск Developer по-прежнему поддерживает только один клиент для [распределенного воспроизведения SQL Server](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Ограничения масштабирования  
  
|Компонент|Enterprise|Standard|Интернет|Express| 
|-------------|----------------|--------------|---------|------------------------|
|Максимальная вычислительная мощность, используемая одним экземпляром, — [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Максимальное значение, поддерживаемое операционной системой|Ограничение: меньшее из 4 процессоров и 24 ядер|Ограничение: меньшее из 4 процессоров и 16 ядер|Ограничение: меньшее из 1 процессора и 4 ядер| 
|Максимальная вычислительная мощность, используемая одним экземпляром, — [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] или [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Максимальное значение, поддерживаемое операционной системой|Ограничение: меньшее из 4 процессоров и 24 ядер|Ограничение: меньшее из 4 процессоров и 16 ядер|Ограничение: меньшее из 1 процессора и 4 ядер|
|Максимальный объем памяти для буферного пула на экземпляр [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Максимум, поддерживаемый операционной системой|128 ГБ|64 ГБ|1410 МБ|
|Максимальный объем памяти для кэша сегмента Columnstore на экземпляр [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Неограниченная память| 32 ГБ| 16 ГБ| 352 МБ|  
|Максимальный размер данных, оптимизированных для памяти, на базу данных в [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Неограниченная память| 32 ГБ| 16 ГБ| 352 МБ|
|Максимальный размер реляционной базы данных|524 ПБ|524 ПБ|524 ПБ|10 ГБ|  
  
<sup>1</sup> Использование выпуска Enterprise Edition с лицензированием по принципу "лицензия на сервер и клиентские лицензии (Server+CAL)" (недоступно для новых соглашений) ограничено максимум 20 ядрами в расчете на экземпляр SQL Server. В модели лицензирования по числу ядер никаких ограничений нет. Дополнительные сведения см. в статье [Вычисление производительности выпуска SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
 
##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> Высокий уровень доступности реляционной СУБД  
  
|Компонент|Enterprise|Standard|Интернет|Express|  
|-------------|----------------|--------------|---------|------------------------|  
|доставка журналов;|Да|Да|Да|нет|  
|Сжатие резервных копий|Да|Да|Нет|нет| 
|Моментальный снимок базы данных|Да|Да|Нет|нет|
|Экземпляр отказоустойчивого кластера Always On <sup>1</sup>|Да|Да|Нет|нет| 
|Группы доступности Always On <sup>2</sup>|Да|Нет|Нет|нет|
|Базовые группы доступности <sup>3</sup>|нет|Да|Нет|нет|
|Группа доступности с минимальным числом реплик для фиксации|Да|Да|Нет|нет|
|Группа доступности без кластеров|Да|Да|Нет|нет|
|Восстановление страниц и файлов в режиме «в сети»|Да|Нет|Нет|нет|
|Индексирование в сети|Да|Нет|Нет|нет|
|Возобновляемая перестройка индексов в подключенном режиме|Да|Нет|Нет|нет|
|Изменение схемы в режиме «в сети»|Да|Нет|Нет|нет|
|Быстрое восстановление|Да|Нет|Нет|нет|
|Зеркальные резервные копии|Да|Нет|Нет|нет|
|Поддержка памяти и ЦП с "горячей" заменой|Да|Нет|Нет|нет|
|Зашифрованная резервная копия|Да|Да|Нет|нет|
|Гибридное резервное копирование в Azure (резервное копирование по URL-адресу)|Да|Да|Нет|нет|
  
<sup>1</sup> В выпуске Enterprise количество узлов равно максимуму, поддерживаемому операционной системой. В выпуске Standard поддерживается два узла. 

<sup>2</sup> В выпуске Enterprise поддерживается до 8 вторичных реплик, включая 2 синхронные вторичные реплики. 

<sup>3</sup> В выпуске Standard поддерживаются базовые группы доступности. Базовая группа доступности поддерживает две реплики с одной базой данных. Дополнительные сведения о базовых группах доступности см. в разделе [Базовые группы доступности](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).    

##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> Масштабируемость и производительность реляционных СУБД  
  
|Компонент|Enterprise|Standard|Интернет|Express|  
|-------------|----------------|--------------|---------|------------------------| 
|Columnstore <sup>1</sup>|Да|Да|Да|Да|  
|Большие двоичные объекты в кластеризованных индексах columnstore|Да|Да|Да|Да|  
|Перестройка некластеризованных индексов columnstore в подключенном режиме|Да|Нет|Нет|нет|
|Выполняющаяся в памяти OLTP <sup>1</sup>|Да|Да|Да|Да|
|Постоянная основная память|Да|Да|Да|Да|
|Секционирование таблиц и индексов|Да|Да|Да|Да|  
|Сжатие данных|Да|Да|Да|Да|
|Resource Governor|Да|Нет|Нет|нет|  
|Параллелизм секционированных таблиц|Да|Нет|Нет|нет|
|Поддержка NUMA, выделение памяти больших страниц и массива буфера|Да|Нет|Нет|нет|
|Управление ресурсами ввода-вывода|Да|Нет|Нет|нет|  
|Отложенная устойчивость|Да|Да|Да|Да|
|Автоматическая настройка|Да|Нет|Нет|нет|
|Адаптивные соединения в пакетном режиме|Да|Нет|Нет|нет|
|Обратная связь по временно предоставляемому буферу памяти в пакетном режиме|Да|Нет|Нет|нет|
|Выполнение с чередованием для функций с табличным значением с несколькими инструкциями|Да|Да|Да|Да|
|Улучшения массовой вставки|Да|Да|Да|Да|


<sup>1</sup> Размер данных выполняющейся в памяти OLTP и кэша сегмента Columnstore ограничены объемом памяти, указанным в выпуске в разделе "Ограничения масштабирования". Максимальная степень параллелизма ограничена. Степень параллелизма процесса (DOP) для построения индекса ограничена значением 2 для выпуска Standard и 1 для выпусков Express и Web. Это относится к индексам columnstore, созданным на основе таблиц на диске и оптимизированных для памяти таблиц.

##  <a name="rdbms-security"></a><a name="RDBMSS"></a> Безопасность реляционных СУБД  
  
|Компонент|Enterprise|Standard|Интернет|Express|
|-------------|----------------|--------------|---------|------------------------------------| 
|Безопасность на уровне строк|Да|Да|Да|Да|  
|Always Encrypted|Да|Да|Да|Да| 
|Динамическое маскирование данных|Да|Да|Да|Да|   
|Основные возможности аудита|Да|Да|Да|Да| 
|Аудит мелких фрагментов данных|Да|Да|Да|Да| 
|Прозрачное шифрование в базе данных|Да|Да|Нет|нет|   
|Определяемые пользователем роли|Да|Да|Да|Да| 
|Автономные базы данных|Да|Да|Да|Да| 
|Шифрование для резервного копирования|Да|Да|Нет|нет|  

##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> Использование реляционных СУБД  
  
|Компонент|Enterprise|Standard|Интернет|Express|   
|-------------|----------------|--------------|---------|------------------------|  
|Выделенное административное соединение|Да|Да|Да|Да, с помощью флага трассировки|   
|Поддержка скриптов PowerShell|Да|Да|Да|Да| 
|Поддержка операций с компонентами приложения уровня данных — извлечение, развертывание, обновление, удаление|Да|Да|Да|Да| 
|Автоматизация политики (проверка по расписанию и изменение)|Да|Да|Да|нет|  
|Сборщик данных производительности|Да|Да|Да|нет|
|Стандартный производительности отчет|Да|Да|Да|нет|
|Структуры планов и закрепление плана для структур планов|Да|Да|Да|нет| 
|Прямой запрос индексированных представлений (с использованием указания NOEXPAND)|Да|Да|Да|Да| 
|Автоматическое сопровождение индексированного представления|Да|Да|Да|нет|
|Распределенные секционированные представления|Да|Нет|Нет|нет| 
|Параллельные операции с индексами|Да|Нет|Нет|нет|  
|Автоматическое использование индексированного представления оптимизатором запросов|Да|Нет|Нет|нет| 
|Проверка согласованности параллелизма|Да|Нет|Нет|нет| 
|Точка управления служебной программой SQL Server|Да|Нет|Нет|нет|    

##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Компонент|Enterprise|Standard|Интернет|Express 
|-------------|----------------|--------------|---------|------------------------|  
|JSON|Да|Да|Да|Да|   
|Хранилище запросов|Да|Да|Да|Да|   
|Temporal|Да|Да|Да|Да|   
|Собственная поддержка XML|Да|Да|Да|Да| 
|Индексирование XML|Да|Да|Да|Да| 
|Возможности MERGE & UPSERT|Да|Да|Да|Да|   
|Типы данных даты и времени|Да|Да|Да|Да|  
|Поддержка международного использования|Да|Да|Да|Да| 
|Семантический поиск и полнотекстовый поиск|Да|Да|Да|Да|
|Определение языка в запросе|Да|Да|Да|Да|
|Компонент Service Broker (сообщения)|Да|Да|Нет (только клиент)|Нет (только клиент)|
|конечные точки в языке Transact-SQL|Да|Да|Да|нет|
|График|Да|Да|Да|Да|  


<sup>1</sup> Для горизонтального увеличения масштаба с несколькими вычислительными узлами требуется головной узел.

## <a name="integration-services"></a><a name="IS"></a> Службы Integration Services

Сведения о функциях служб Integration Services (SSIS), поддерживаемых различными выпусками [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], см. в статье о [функциях служб Integration Services, поддерживаемых разными выпусками SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Пространственные службы и службы расположения  
  
|Имя функции|Enterprise|Standard|Интернет|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Пространственные индексы|Да|Да|Да|Да|   
|Плоский и геодезический типы данных|Да|Да|Да|Да| 
|Дополнительные пространственные библиотеки|Да|Да|Да|Да|   
|Импорт-экспорт стандартных форматов пространственных данных|Да|Да|Да|Да|   

## <a name="unsupported-features--services"></a><a name="Unsupported"></a> Неподдерживаемые функции и службы

Следующие функции и службы недоступны для SQL Server 2019 на Linux. Поддержка этих функций будет постепенно реализовываться с течением времени.

| Область | Неподдерживаемая функция или служба |
|-----|-----|
| **Ядро СУБД** | Репликация слиянием |
| &nbsp; | База данных Stretch |
| &nbsp; | Распределенный запрос с сторонними подключениями |
| &nbsp; | Связанные серверы для источников данных, отличных от [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  |
| &nbsp; | Системные расширенные хранимые процедуры (XP_CMDSHELL и т. п.) |
| &nbsp; | Filetable, FILESTREAM |
| &nbsp; | Сборки CLR с набором разрешений EXTERNAL_ACCESS или UNSAFE |
| &nbsp; | Buffer Pool Extension |
| &nbsp; | Резервное копирование по URL-адресу: страничный BLOB-объект<sup>2</sup> |
| **Агент SQL Server** |  Подсистемы: CmdExec, PowerShell, средство чтения очереди, SSIS, SSAS, SSRS |
| &nbsp; | видны узлы |
| &nbsp; | Управляемое резервное копирование |
| **Обеспечение высокого уровня доступности** | Зеркальное отображение базы данных  |
| **Безопасность** | расширенное управление ключами |
| &nbsp; | Проверка подлинности AD для связанных серверов |
| &nbsp; | Аутентификация AD для конечных точек групп доступности |
| **Службы** | Обозреватель SQL Server |
| &nbsp; | Службы R <sup>1</sup> SQL Server |
| &nbsp; | StreamInsight |
| &nbsp; | Службы Analysis Services |
| &nbsp; | Службы Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Службы Master Data Services |

<sup>1</sup> R SQL Server поддерживается в SQL Server, однако службы R SQL Server в виде отдельного пакета нет.

<sup>2</sup> Резервное копирование по URL-адресу поддерживается для блочных BLOB-объектов с использованием [подписанного URL-адреса](../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS).

## <a name="next-steps"></a>Дальнейшие действия
 [Выпуски и поддерживаемые функции для SQL Server 2017 на Linux](sql-server-linux-editions-and-components-2017.md)  
 [Возможности, поддерживаемые различными выпусками SQL Server 2019 — Windows](../sql-server/editions-and-components-of-sql-server-version-15.md)  
 [Возможности, поддерживаемые различными выпусками SQL Server 2017 — Windows](../sql-server/editions-and-components-of-sql-server-2017.md)  
 [Возможности, поддерживаемые различными выпусками SQL Server 2016 — Windows](../sql-server/editions-and-components-of-sql-server-2016.md)  
 [Установка SQL Server 2016](../database-engine/install-windows/install-sql-server.md)  
 [Спецификации SQL Server](../sql-server/index.yml)

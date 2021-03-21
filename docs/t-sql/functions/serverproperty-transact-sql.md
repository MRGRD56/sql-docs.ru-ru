---
description: SERVERPROPERTY (Transact-SQL)
title: SERVERPROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SERVERPROPERTY_TSQL
- SERVERPROPERTY
- sql13.swb.serverpropeties.connections.f1
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- SERVERPROPERTY function
- server instance property information [SQL Server]
- IsHadrEnabled server property
- instances of SQL Server, property information
- server properties [SQL Server]
ms.assetid: 11e166fa-3dd2-42d8-ac4b-04f18c612c4a
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c5572f1d3e85ed5e9efc2006dabbddddfc0c3b20
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747944"
---
# <a name="serverproperty-transact-sql"></a>SERVERPROPERTY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает сведения о свойстве экземпляра сервера.  

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
SERVERPROPERTY ( 'propertyname' )  
```  

> [!IMPORTANT]
> Номера версий [!INCLUDE[ssde_md](../../includes/ssde_md.md)] для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не сравнимы друг с другом. Они являются внутренними номерами сборок этих отдельных продуктов. База кода [!INCLUDE[ssde_md](../../includes/ssde_md.md)] для [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] такая же, что и для [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Важнее всего то, что [!INCLUDE[ssde_md](../../includes/ssde_md.md)] в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] всегда имеет новейшие биты [!INCLUDE[ssde_md](../../includes/ssde_md.md)] SQL. Версия 12 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] новее, чем версия 15 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*propertyname*  
Выражение, содержащее сведения о свойстве, которые необходимо вернуть для сервера. *propertyname* может иметь одно из указанных ниже значений.  
  
|Свойство|Возвращаемые значения|  
|--------------|---------------------|  
|BuildClrVersion|Версия среды CLR [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], которая использовалась при сборке экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|Параметры сортировки|Имя параметров сортировки для сервера, установленного по умолчанию.<br /><br /> NULL = недопустимый ввод или произошла ошибка.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|CollationID|Идентификатор параметров сортировки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Базовый тип данных: **int**|  
|ComparisonStyle|Стиль сравнения Windows для параметров сортировки.<br /><br /> Базовый тип данных: **int**|  
|ComputerNamePhysicalNetBIOS|Имя NetBIOS для локального компьютера, на котором работает экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Для кластеризованного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на отказоустойчивом кластере это значение изменяется, когда экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] переключается на другие узлы в отказоустойчивом кластере.<br /><br /> Для изолированного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это значение остается постоянным и совпадает со значением, возвращаемым свойством MachineName.<br /><br /> **Примечание.** Если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] находится в отказоустойчивом кластере и требуется получить имя экземпляра отказоустойчивого кластера, воспользуйтесь свойством MachineName.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|Выпуск|Установленный выпуск экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используйте значение этого свойства для определения функций и ограничений, таких как [ограничения вычислительной емкости для разных выпусков SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md). В 64-разрядных версиях компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] к обозначению версии добавляется «(64-разрядная версия)».<br /><br /> Возвращает:<br /><br /> выпуск «Enterprise Edition»;<br /><br /> выпуск "Enterprise Edition: лицензирование по числу ядер";<br /><br /> выпуск «Enterprise Evaluation Edition»;<br /><br /> выпуск Business Intelligence Edition;<br /><br /> выпуск «Developer Edition»;<br /><br /> выпуск «Express Edition»;<br /><br /> экспресс-выпуск с дополнительными службами;<br /><br /> выпуск «Standard Edition»;<br /><br /> «Web Edition».<br /><br /> "SQL Azure" означает [!INCLUDE[ssSDS](../../includes/sssds-md.md)] или [!INCLUDE[ssDW](../../includes/ssdw-md.md)].<br /><br /> "Azure SQL Edge Developer" обозначает выпуск SQL Azure для пограничных вычислений, предназначенный только для разработчиков.<br /><br /> "Azure SQL Edge" указывает платный выпуск для SQL Azure для пограничных вычислений.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|EditionID|EditionID представляет установленный выпуск продукта для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используйте значение этого свойства для определения функций и ограничений, таких как [ограничения вычислительной емкости для разных выпусков SQL Server](../../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).<br /><br /> 1804890536 = Enterprise<br /><br /> 1872460670 = Enterprise Edition: лицензирование на ядро<br /><br /> 610778273 = Enterprise Evaluation<br /><br /> 284895786 = Business Intelligence<br /><br /> -2117995310 = Developer<br /><br /> -1592396055 = Express<br /><br /> -133711905= Express with Advanced Services<br /><br /> –1534726760 = Standard<br /><br /> 1293598313 = Web<br /><br /> 1674378470 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)] или [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]<br /><br /> -1461570097 = Azure SQL Edge Developer <br /><br /> 1994083197 = Azure SQL Edge <br /><br />Базовый тип данных: **bigint**|  
|EngineEdition|Выпуск компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], установленного на сервере.<br /><br /> 1 = Personal или Desktop Engine (недоступен для [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и более поздних версий).<br /><br /> 2 = Standard (возвращается для выпусков Standard, Web и Business Intelligence).<br /><br /> 3 = Enterprise (это значение возвращается для выпусков Evaluation, Developer и Enterprise).<br /><br /> 4 = Express (возвращается для выпусков Express, Express with Tools и Express with Advanced Services).<br /><br /> 5 = [!INCLUDE[ssSDS](../../includes/sssds-md.md)]<br /><br /> 6 = [!INCLUDE[ssDW](../../includes/ssdw-md.md)]<br /><br /> 8 = [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]<br /><br /> 9 = SQL Azure для пограничных вычислений (это значение возвращается для всех выпусков SQL Azure для пограничных вычислений).<br /><br /> 11 = бессерверный пул SQL в Azure Synapse<br /><br /> Базовый тип данных: **int**|  
|FilestreamConfiguredLevel|Настроенный уровень доступа FILESTREAM. Дополнительные сведения см. в разделе [Уровень доступа к файловому потоку](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).<br /><br /> Базовый тип данных: **int**|  
|FilestreamEffectiveLevel|Действующий уровень доступа FILESTREAM. Это значение может отличаться от значения FilestreamConfiguredLevel, если уровень был изменен и ожидается перезапуск экземпляра или перезагрузка компьютера. Дополнительные сведения см. в разделе [Уровень доступа к файловому потоку](../../database-engine/configure-windows/filestream-access-level-server-configuration-option.md).<br /><br /> Базовый тип данных: **int**|  
|FilestreamShareName|Имя общего ресурса, используемое FILESTREAM.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **nvarchar(128)**| 
|HadrManagerStatus|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Показывает, запущен ли диспетчер [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].<br /><br /> 0 = не запущен, ожидает связи.<br /><br /> 1 = запущен и выполняется.<br /><br /> 2 = не запущен и завершился неудачно.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|InstanceDefaultBackupPath|**Применимо к**: [!INCLUDE[ssSQL2019](../../includes/sssql19-md.md)] и более поздних версий.<br /><br /> Имя пути по умолчанию к файлам резервных копий экземпляра.|  
|InstanceDefaultDataPath|**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до текущей версии в обновлениях, выпущенных начиная с конца 2015 г.<br /><br /> Имя пути по умолчанию к файлам данных экземпляра.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|InstanceDefaultLogPath|**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до текущей версии в обновлениях, выпущенных начиная с конца 2015 г.<br /><br /> Имя пути по умолчанию к файлам журналов экземпляра.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|InstanceName|Имя экземпляра, к которому подключен пользователь.<br /><br /> Возвращает значение NULL в случае, если имя экземпляра установлено по умолчанию, при возникновении ошибки и в случае, если входные данные оказываются недопустимы.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|IsAdvancedAnalyticsInstalled|Возвращает значение 1, если компонент расширенной аналитики был установлен во время установки системы, или значение 0, если компонент расширенной аналитики не был установлен.<br /><br /> Базовый тип данных: **int**|  
|IsBigDataCluster| Появился в [!INCLUDE[ssSQL2019](../../includes/sssql19-md.md)], начиная с накопительного пакета обновления 4 (CU4).<br /><br />Возвращает значение 1, если экземпляр является кластером больших данных SQL Server, в противном случае — значение 0.<br /><br /> Базовый тип данных: **int**|  
|IsClustered|Экземпляр сервера настроен для работы в отказоустойчивом кластере.<br /><br /> 1 = в кластере.<br /><br /> 0 = не в кластере.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|IsFullTextInstalled|На текущем экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] установлены компоненты полнотекстового и семантического индексирования.<br /><br /> 1 = компоненты полнотекстового и семантического индексирования установлены.<br /><br /> 0 = компоненты полнотекстового и семантического индексирования не установлены.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|IsHadrEnabled|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Служба [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] включена на этом экземпляре сервера.<br /><br /> 0 = компонент [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] отключен.<br /><br /> 1 = компонент [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] включен.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**<br /><br /> Для реплик доступности, создаваемых и запускаемых на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], на экземпляре сервера должна быть включена служба [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Дополнительные сведения см. в статье [Включение и отключение групп доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md).<br /><br /> **Примечание.** Свойство IsHadrEnabled относится только к [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]. Другие возможности высокого уровня доступности или аварийного восстановления, такие как зеркальное отображение базы данных или доставка журналов, не затрагиваются этим свойством сервера.|  
|IsIntegratedSecurityOnly|Сервер запущен во встроенном режиме безопасности.<br /><br /> 1 = встроенная безопасность (проверка подлинности Windows)<br /><br /> 0 = без встроенного режима безопасности. (Как проверка подлинности Windows, так и проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].)<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|IsLocalDB|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Сервер является экземпляром [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|IsPolybaseInstalled|**Применимо к**: [!INCLUDE[ssSQL2016](../../includes/sssql16-md.md)].<br /><br /> Возвращает значение, показывающее, установлен ли компонент PolyBase в экземпляре сервера.<br /><br /> 0 = компонент PolyBase не установлен.<br /><br /> 1 = компонент PolyBase установлен.<br /><br /> Базовый тип данных: **int**|  
|IsSingleUser|Server запущен в однопользовательском режиме.<br /><br /> 1 = однопользовательский режим.<br /><br /> 0 = не однопользовательский режим.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|IsTempDbMetadataMemoryOptimized|**Область применения**: [!INCLUDE[ssSQL2019](../../includes/sssql19-md.md)] и более поздних версий.<br /><br />Возвращает значение 1, если в базе данных tempdb включено использование оптимизированных для памяти таблиц для метаданных, или значение 0, если база данных tempdb использует обычные дисковые таблицы для метаданных. Дополнительные сведения см. в статье [tempdb Database](../../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata).<br /><br /> Базовый тип данных: **int**|  
|IsXTPSupported|**Область применения**: SQL Server ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и выше), [!INCLUDE[ssSDS](../../includes/sssds-md.md)].<br /><br /> Сервер поддерживает компонент In-Memory OLTP.<br /><br /> 1 = сервер поддерживает компонент In-Memory OLTP.<br /><br /> 0= сервер не поддерживает компонент In-Memory OLTP.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|LCID|Код локали Windows для параметров сортировки.<br /><br /> Базовый тип данных: **int**|  
|LicenseType|Не используется. В продукте [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не сохраняются сведения о лицензии. Всегда возвращает DISABLED.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|MachineName|Имя компьютера Windows, на котором запущен экземпляр сервера.<br /><br /> Для кластеризованного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], работающего на виртуальном сервере службы кластеров (Майкрософт), возвращается имя виртуального сервера.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|NumLicenses|Не используется. В продукте [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не сохраняются сведения о лицензии. Всегда возвращает значение NULL.<br /><br /> Базовый тип данных: **int**|  
|ProcessID|Идентификатор процесса службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. С помощью свойства ProcessID удобно определять, какой файл Sqlservr.exe принадлежит этому экземпляру.<br /><br /> NULL = недопустимый ввод, ошибка или неприменимо.<br /><br /> Базовый тип данных: **int**|  
|ProductBuild|**Применимо к**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] начиная с октября 2015 г.<br /><br /> Номер сборки.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ProductBuildType|**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до текущей версии в обновлениях, выпущенных начиная с конца 2015 г.<br /><br /> Тип текущей сборки.<br /><br /> Возвращает одно из следующих значений.<br /><br /> OD = выпуск по запросу для определенного клиента.<br /><br /> GDR = выпуск для общего распространения посредством обновления Windows.<br /><br /> NULL = неприменимо.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ProductLevel|Уровень версии экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Возвращает одно из следующих значений.<br /><br /> 'RTM' = Исходная выпущенная версия<br /><br /> 'SP *n*' = версия пакета обновления<br /><br /> 'CTP *n*', = ознакомительная версия для сообщества<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ProductMajorVersion|**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до текущей версии в обновлениях, выпущенных начиная с конца 2015 г.<br /><br /> Основная версия.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ProductMinorVersion|**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до текущей версии в обновлениях, выпущенных начиная с конца 2015 г.<br /><br /> Дополнительная версия.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ProductUpdateLevel|**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до текущей версии в обновлениях, выпущенных начиная с конца 2015 г.<br /><br /> Уровень обновления текущей сборки. CU означает накопительный пакет обновления.<br /><br /> Возвращает одно из следующих значений.<br /><br /> CU *n* = накопительный пакет обновления<br /><br /> NULL = неприменимо.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ProductUpdateReference|**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до текущей версии в обновлениях, выпущенных начиная с конца 2015 г.<br /><br /> Статья базы знаний для этого выпуска.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ProductVersion|Версия экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в формате *основной_номер.дополнительный_номер.сборка.редакция*.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ResourceLastUpdateDateTime|Отображаются дата и время последнего изменения базы данных Resource.<br /><br /> Базовый тип данных: **datetime**|  
|ResourceVersion|Возвращает версию базы данных Resource.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|ServerName|Сведения об экземпляре и сервере Windows, связанные с определенным экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> NULL = недопустимый ввод или произошла ошибка.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|SqlCharSet|Идентификатор кодировки SQL из идентификатора параметров сортировки.<br /><br /> Базовый тип данных: **tinyint**|  
|SqlCharSetName|Имя кодировки SQL из параметров сортировки.<br /><br /> Базовый тип данных: **nvarchar(128)**|  
|SqlSortOrder|Идентификатор порядка сортировки SQL из параметров сортировки<br /><br /> Базовый тип данных: **tinyint**|  
|SqlSortOrderName|Имя порядка сортировки SQL из параметров сортировки<br /><br /> Базовый тип данных: **nvarchar(128)**|  
  
## <a name="return-types"></a>Типы возвращаемых данных  

**sql_variant**
  
## <a name="remarks"></a>Remarks  
  
### <a name="servername-property"></a>Свойство ServerName

Свойство `ServerName` функции `SERVERPROPERTY` и функция [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) возвращают аналогичные сведения. В свойстве `ServerName` задаются имена экземпляра и сервера Windows, которые вместе образуют уникальный экземпляр сервера. Функция [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) возвращает текущее имя локального сервера.  
  
Свойство `ServerName` и функция [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) возвращают одинаковые сведения, если установленное по умолчанию имя сервера не было изменено во время установки. Имя локального сервера можно настроить, выполнив следующие команды:  
  
```sql
EXEC sp_dropserver 'current_server_name';  
GO  
EXEC sp_addserver 'new_server_name', 'local';  
GO  
```  
  
Если имя локального сервера было изменено во время установки и отличается от заданного по умолчанию имени, то функция [@@SERVERNAME](../../t-sql/functions/servername-transact-sql.md) возвращает новое имя.  
  
### <a name="version-properties"></a>Свойства версии

Функция `SERVERPROPERTY` возвращает отдельные свойства, которые относятся к информации о версии, а функция [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md) объединяет выходные данные в одну строку. Если для конкретного приложения требуются отдельные строки свойств, можно использовать функцию `SERVERPROPERTY`, которая возвращает эти строки, а не заниматься синтаксическим анализом результатов функции [@@VERSION](../../t-sql/functions/version-transact-sql-configuration-functions.md).  

## <a name="permissions"></a>Разрешения

Все пользователи могут запрашивать свойства сервера.
  
## <a name="examples"></a>Примеры

В приведенном ниже примере используется функция `SERVERPROPERTY` в инструкции `SELECT` для получения сведений о текущем экземпляре [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].
  
```sql
SELECT  
  SERVERPROPERTY('MachineName') AS ComputerName,
  SERVERPROPERTY('ServerName') AS InstanceName,  
  SERVERPROPERTY('Edition') AS Edition,
  SERVERPROPERTY('ProductVersion') AS ProductVersion,  
  SERVERPROPERTY('ProductLevel') AS ProductLevel;  
GO  
```  
  
## <a name="see-also"></a>См. также:

[Выпуски и компоненты SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)  

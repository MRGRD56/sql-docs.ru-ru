---
description: Новые функции даты и времени с предыдущими версиями SQL Server (OLE DB)
title: Функции OLE DB даты и времени с предыдущими версиями SQL Server
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: 96976bac-018c-47cc-b1b2-fa9605eb55e5
author: markingmyname
ms.author: maghan
ms.custom: seo-dt-2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 55a7e519f22bb10a472a8c76bb88bb003ab7ad21
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755914"
---
# <a name="new-date-and-time-features-with-previous-sql-server-versions-ole-db"></a>Новые функции даты и времени с предыдущими версиями SQL Server (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В этом разделе описывается ожидаемое поведение, когда клиентское приложение, использующее улучшенные функции даты и времени, взаимодействует с версией, предшествующей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , и когда клиент компилируется с версией [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственного клиента раньше, чем [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Отправка команд на сервер, поддерживающий улучшенные функции даты и времени.  
  
## <a name="down-level-client-behavior"></a>Работа в клиентах низкого уровня  
 Клиентские приложения, использующие версию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственного клиента, более раннюю, чем [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] новые типы даты и времени как столбцы типа **nvarchar** . Содержимое столбца представлено литералом. Дополнительные сведения см. в разделе "форматы данных: строки и литералы" статьи [Поддержка типов данных для OLE DB улучшения даты и времени](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md). Размер столбца представляет максимальную длину литерала для точности, указанной для столбца.  
  
 API-интерфейсы каталога будут возвращать метаданные, соответствующие коду типа данных нижнего уровня, возвращаемому клиенту (например, **nvarchar**), и связанному представлению нижнего уровня (например, соответствующему литеральному формату). Тем не менее, будет возвращено реальное имя типа данных [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 Если клиентское приложение нижнего уровня выполняется [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] на сервере (или более поздней версии), на котором изменения схемы производятся в типах даты и времени, то ожидаемое поведение выглядит следующим образом:  
  
|Тип клиента OLE DB|Тип SQL Server 2005|Тип SQL Server 2008 (или более поздних версий)|Преобразование результата (сервер-клиент)|Преобразование параметра (клиент-сервер)|  
|------------------------|--------------------------|---------------------------------------|--------------------------------------------|-----------------------------------------------|  
|DBTYPE_DBDATE|Datetime|Дата|ОК|ОК|  
|DBTYPE_DBTIMESTAMP|||Поля времени устанавливаются в нули.|IRowsetChange завершится ошибкой из-за усечения строки, если поле времени не равно нулю.|  
|DBTYPE_DBTIME||Time(0)|ОК|ОК|  
|DBTYPE_DBTIMESTAMP|||Поля даты устанавливаются в текущую дату.|IRowsetChange завершится ошибкой из-за усечения строки, если доли секунды не равны нулю.<br /><br /> Дата не учитывается.|  
|DBTYPE_DBTIME||Time(7)|Сбой-недопустимый литерал времени.|ОК|  
|DBTYPE_DBTIMESTAMP|||Сбой-недопустимый литерал времени.|ОК|  
|DBTYPE_DBTIMESTAMP||Datetime2 (3)|ОК|ОК|  
|DBTYPE_DBTIMESTAMP||Datetime2 (7)|ОК|ОК|  
|DBTYPE_DBDATE|Smalldatetime|Дата|ОК|ОК|  
|DBTYPE_DBTIMESTAMP|||Поля времени устанавливаются в нули.|IRowsetChange завершится ошибкой из-за усечения строки, если поле времени не равно нулю.|  
|DBTYPE_DBTIME||Time(0)|ОК|ОК|  
|DBTYPE_DBTIMESTAMP|||Поля даты устанавливаются в текущую дату.|IRowsetChange завершится ошибкой из-за усечения строки, если доли секунды не равны нулю.<br /><br /> Дата не учитывается.|  
|DBTYPE_DBTIMESTAMP||Datetime2(0)|ОК|ОК|  
  
 Значение «ОК» означает, что если код работал в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], то он должен работать и в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (и более поздних версиях).  
  
 Учтены только следующие типичные изменения схемы.  
  
-   Использование нового типа, когда логически приложению необходимо только значение даты или времени. Однако приложение было вынуждено использовать **DateTime** или **smalldatetime** , так как разные типы даты и времени недоступны.  
  
-   Использование нового типа для получения долей секунд c более высокой точностью.  
  
-   Переключение на **datetime2** , так как это предпочтительный тип данных для даты и времени.  
  
 Приложения, использующие метаданные сервера, полученные с помощью ICommandWithParameters:: GetParameterInfo или наборов строк схемы для задания сведений о типе параметра через ICommandWithParameters:: SetParameterInfo, завершатся ошибкой во время преобразования клиента, где строковое представление исходного типа больше, чем строковое представление целевого типа. Например, если в клиентской привязке используется DBTYPE_DBTIMESTAMP и столбец Server имеет значение Date, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент преобразует его в «гггг-дд-мм чч: мм: СС. FFF», но метаданные сервера будут возвращены как **nvarchar (10)**. В результате переполнение приведет к ошибке DBSTATUS_E_CATCONVERTVALUE. Аналогичные проблемы возникают при преобразовании данных с помощью IRowsetChange, так как метаданные набора строк задаются из метаданных ResultSet.  
  
### <a name="parameter-and-rowset-metadata"></a>Метаданные параметров и наборов строк  
 В этом разделе описываются метаданные для параметров, столбцов результатов и наборов строк схемы для клиентов, компилируемых с версией [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, предшествующей версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .  
  
#### <a name="icommandwithparametersgetparameterinfo"></a>ICommandWithParameters::GetParameterInfo  
 Структура DBPARAMINFO возвращает следующие сведения с помощью параметра *пргпараминфо* :  
  
|Тип параметра|wType|ulParamSize|bPrecision|bScale|  
|--------------------|-----------|-----------------|----------------|------------|  
|Дата|DBTYPE_WSTR|10|~0|~0|  
|time|DBTYPE_WSTR|8, 10..16|~0|~0|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|DATETIME|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19, 21.27|~0|~0|  
|datetimeoffset|DBTYPE_WSTR|26, 28.34|~0|~0|  
  
 Обратите внимание, что некоторые из этих диапазонов значений не являются непрерывными, например в последовательности 8, 10… 16 отсутствует значение 9. Это следствие добавления десятичной запятой, когда точность в долях секунды выше нуля.  
  
#### <a name="icolumnsrowsetgetcolumnsrowset"></a>IColumnsRowset::GetColumnsRowset  
 Возвращаются следующие столбцы.  
  
|Тип столбца|DBCOLUMN_TYPE|DBCOLUMN_COLUMNSIZE|DBCOLUMN_PRECISION|DBCOLUMN_SCALE, DBCOLUMN_DATETIMEPRECISION|  
|-----------------|--------------------|--------------------------|-------------------------|--------------------------------------------------|  
|Дата|DBTYPE_WSTR|10|NULL|NULL|  
|time|DBTYPE_WSTR|8, 10..16|NULL|NULL|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|DATETIME|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19, 21.27|NULL|NULL|  
|datetimeoffset|DBTYPE_WSTR|26, 28.34|NULL|NULL|  
  
#### <a name="columnsinfogetcolumninfo"></a>ColumnsInfo::GetColumnInfo  
 Структура DBCOLUMNINFO возвращает следующие данные.  
  
|Тип параметра|wType|ulColumnSize|bPrecision|bScale|  
|--------------------|-----------|------------------|----------------|------------|  
|Дата|DBTYPE_WSTR|10|~0|~0|  
|time(1..7)|DBTYPE_WSTR|8, 10..16|~0|~0|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|DATETIME|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19, 21.27|~0|~0|  
|datetimeoffset|DBTYPE_WSTR|26, 28.34|~0|~0|  
  
### <a name="schema-rowsets"></a>Наборы строк схемы  
 В этом разделе рассматриваются метаданные для параметров, результирующих столбцов и наборов строк схемы для новых типов данных. Эта информация полезна при наличии поставщика клиента, разработанного с помощью средств, предшествующих [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
#### <a name="columns-rowset"></a>COLUMNS, набор строк  
 Для типов даты-времени возвращаются значения следующих столбцов.  
  
|Тип столбца|DATA_TYPE|CHARACTER_MAXIMUM_LENGTH|CHARACTER_OCTET_LENGTH|DATETIME_PRECISION|  
|-----------------|----------------|--------------------------------|------------------------------|-------------------------|  
|Дата|DBTYPE_WSTR|10|20|NULL|  
|time|DBTYPE_WSTR|8, 10..16|16, 20.. 32|NULL|  
|smalldatetime|DBTYPE_DBTIMESTAMP|NULL|NULL|0|  
|DATETIME|DBTYPE_DBTIMESTAMP|NULL|NULL|3|  
|datetime2|DBTYPE_WSTR|19, 21.27|38, 42.. 54|NULL|  
|datetimeoffset|DBTYPE_WSTR|26, 28.34|52, 56.. 68|NULL|  
  
#### <a name="procedure_parameters-rowset"></a>Набор строк PROCEDURE_PARAMETERS  
 Для типов даты-времени возвращаются значения следующих столбцов.  
  
|Тип столбца|DATA_TYPE|CHARACTER_MAXIMUM_LENGTH|CHARACTER_OCTET_LENGTH|TYPE_NAME<br /><br /> LOCAL_TYPE_NAME|  
|-----------------|----------------|--------------------------------|------------------------------|--------------------------------------|  
|Дата|DBTYPE_WSTR|10|20|Дата|  
|time|DBTYPE_WSTR|8, 10..16|16, 20.. 32|time|  
|smalldatetime|DBTYPE_DBTIMESTAMP|NULL|NULL|smalldatetime|  
|DATETIME|DBTYPE_DBTIMESTAMP|NULL|NULL|DATETIME|  
|datetime2|DBTYPE_WSTR|19, 21.27|38, 42.. 54|datetime2|  
|datetimeoffset|DBTYPE_WSTR|26, 28.34|52, 56.. 68|datetimeoffset|  
  
#### <a name="provider_types-rowset"></a>Набор строк PROVIDER_TYPES  
 Для типов даты-времени возвращаются следующие строки:  
  
|Тип -><br /><br /> Столбец|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|--------------------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|DATA_TYPE|DBTYPE_WSTR|DBTYPE_WSTR|DBTYPE_DBTIMESTAMP|DBTYPE_DBTIMESTAMP|DBTYPE_WSTR|DBTYPE_WSTR|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|'|'|'|'|'|'|  
|LITERAL_SUFFIX|'|'|'|'|'|'|  
|CREATE_PARAMS|NULL|NULL|NULL|NULL|NULL|NULL|  
|IS_NULLABLE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|  
|CASE_SENSITIVE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|NULL|NULL|NULL|NULL|NULL|NULL|  
|FIXED_PREC_SCALE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|AUTO_UNIQUE_VALUE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|LOCAL_TYPE_NAME|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|MINIMUM_SCALE|NULL|NULL|NULL|NULL|NULL|NULL|  
|MAXIMUM_SCALE|NULL|NULL|NULL|NULL|NULL|NULL|  
|GUID|NULL|NULL|NULL|NULL|NULL|NULL|  
|TYPELIB|NULL|NULL|NULL|NULL|NULL|NULL|  
|VERSION|NULL|NULL|NULL|NULL|NULL|NULL|  
|IS_LONG|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|BEST_MATCH|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_TRUE|VARIANT_FALSE|VARIANT_FALSE|  
|IS_FIXEDLENGTH|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
  
## <a name="down-level-server-behavior"></a>Работа сервера низкого уровня  
 При соединении с сервером более ранней версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] любая попытка использовать имена новых типов серверов (например, с ICommandWithParameters:: SetParameterInfo или ITableDefinition:: CreateTable) приведет к DB_E_BADTYPENAME.  
  
 Если новые типы привязываются к параметрам или результатам без указания имени типа и новый тип неявным образом указывает на серверный тип данных либо отсутствует допустимое преобразование из серверного типа данных в клиентский, то возвращается ошибка DB_E_ERRORSOCCURRED, а DBBINDSTATUS_UNSUPPORTED_CONVERSION устанавливается в состояние привязки для метода доступа, используемого Execute.  
  
 Если существует поддерживаемое клиентское преобразование из типа буфера в серверный тип данных для той версии сервера, с которым установлено соединение, то могут быть использованы все типы данных буфера клиента. В этом контексте *Тип сервера* означает тип, заданный параметром ICommandWithParameters:: SetParameterInfo или подразумеваемый типом буфера, если ICommandWithParameters:: SetParameterInfo не был вызван. Это означает, что DBTYPE_DBTIME2 и DBTYPE_DBTIMESTAMPOFFSET могут быть использованы с серверами низкого уровня или при DataTypeCompatibility=80, если клиентское преобразование к поддерживаемому серверному типу данных выполнено успешно. Конечно, если обнаружен неверный серверный тип данных, то сервер может сообщить об ошибке, если не удается выполнить неявное преобразование к действительному серверному типу данных.  
  
## <a name="ssprop_init_datatypecompatibility-behavior"></a>Поведение SSPROP_INIT_DATATYPECOMPATIBILITY  
 Если SSPROP_INIT_DATATYPECOMPATIBILITY имеет значение SSPROPVAL_DATATYPECOMPATIBILITY_SQL2000, новые типы даты и времени и связанные метаданные отображаются на клиентах в том виде, в котором они отображаются для клиентов нижнего уровня, как описано в статье об изменениях в разделе " [групповое копирование" для расширенных типов даты и времени &#40;OLE DB и ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  
  
## <a name="comparability-for-irowsetfind"></a>Сравнимость для IRowsetFind  
 Все операторы сравнения разрешены для новых типов данных даты-времени, так как они отображаются как строковые типы, а не типы даты-времени.  
  
## <a name="see-also"></a>См. также:  
 [Улучшения функций даты и времени &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  

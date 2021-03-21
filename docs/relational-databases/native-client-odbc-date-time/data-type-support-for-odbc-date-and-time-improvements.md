---
title: Поддержка типов, Дата и время ODBC
description: Сведения о типах ODBC, поддерживающих SQL Server типы данных даты и времени, включая сопоставление типов данных и форматы для строк, литералов и структур данных.
ms.custom: ''
ms.date: 12/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- date/time [ODBC], data type support
- ODBC, date/time improvements
ms.assetid: 8e0d9ba2-3ec1-4680-86e3-b2590ba8e2e9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8acb8060cb543d264f7237bb720d54ce5b9daedd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756124"
---
# <a name="data-type-support-for-odbc-date-and-time-improvements"></a>Поддержка типов данных для улучшений функций даты и времени ODBC
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Этот раздел содержит сведения о типах ODBC, поддерживающих типы данных даты и времени [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="data-type-mapping-in-parameters-and-resultsets"></a>Сопоставление типов данных в параметрах и результирующих наборах  
 Кроме типов данных ODBC (SQL_TYPE_TIMESTAMP и SQL_TIMESTAMP), в ODBC-драйвер собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] были добавлены два новых типа данных для представления новых типов серверов.  
  
-   SQL_SS_TIME2  
  
-   SQL_SS_TIMESTAMPOFFSET  
  
 В следующей таблице показано полное сопоставление серверных типов данных. Обратите внимание, что некоторые ячейки таблицы содержат по две записи. В таких случаях первая запись — это значение ODBC 3.0, а вторая — значение ODBC 2.0.  
  
|Тип данных SQL Server|Тип данных SQL|Значение|  
|--------------------------|-------------------|-----------|  
|Datetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
|Smalldatetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
|Дата|SQL_TYPE_DATE<br /><br /> SQL_DATE|91 (SQL. h)<br /><br /> 9 (sqlext. h)|  
|время;|SQL_SS_TIME2|-154 (SQLNCLI. h)|  
|DatetimeOFFSET|SQL_SS_TIMESTAMPOFFSET|-155 (SQLNCLI.h)|  
|Datetime2|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
||||

 В следующей таблице приведены соответствующие структуры и типы ODBC C. Поскольку ODBC не поддерживает типы C, определяемые драйвером, для представления данных time и datetimeoffset в виде двоичных структур используется тип SQL_C_BINARY.  
  
|Тип данных SQL|Организация памяти|Тип данных C по умолчанию|Значение (sqlext.h)|  
|-------------------|-------------------|-------------------------|------------------------|  
|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|SQL_TIMESTAMP_STRUCT<br /><br /> TIMESTAMP_STRUCT;|SQL_C_TYPE_TIMESTAMP<br /><br /> SQL_C_TIMESTAMP|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|  
|SQL_TYPE_DATE<br /><br /> SQL_DATE|SQL_DATE_STRUCT<br /><br /> DATE_STRUCT;|SQL_C_TYPE_DATE<br /><br /> SQL_C_DATE|SQL_TYPE_DATE<br /><br /> SQL_DATE|  
|SQL_SS_TIME2|SQL_SS_TIME2_STRUCT|SQL_C_SS_TIME2<br /><br /> SQL_C_BINARY (ODBC 3.5 и ранее)|0x4000 (sqlncli.h)<br /><br /> SQL_BINARY (-2)|  
|SQL_SS_TIMESTAMPOFFSET|SQL_SS_TIMESTAMPOFFSET_STRUCT|SQL_C_SS_TIMESTAMPOFFSET<br /><br /> SQL_C_BINARY (ODBC 3.5 и ранее)|0x4001 (sqlncli.h)<br /><br /> SQL_BINARY (-2)|  
|||||

 Если определена привязка SQL_C_BINARY, будет выполнена проверка выравнивания, и в случае неверного выравнивания будет выдана ошибка. Код SQLSTATE для этой ошибки равен IM016 с сообщением «Неверное выравнивание структуры».  
  
## <a name="data-formats-strings-and-literals"></a>Форматы данных: строки и литералы  
 В следующей таблице показано сопоставление типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и ODBC и строковых литералов ODBC.  
  
|Тип данных SQL Server|Тип данных ODBC|Формат строки для клиентских преобразований|  
|--------------------------|--------------------|------------------------------------------|  
|Datetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|'гггг-мм-дд чч:мм:сс:[.999]'<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для типа Datetime поддерживает значения долей секунды, состоящие из не более чем трех цифр.|  
|Smalldatetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|'yyyy-mm-dd hh:hh:ss'<br /><br /> Точность этого типа данных составляет одну минуту. При выводе данных секунды будут равны нулю, а при вводе данных они округляются сервером.|  
|Дата|SQL_TYPE_DATE<br /><br /> SQL_DATE|'гггг-мм-дд'|  
|время;|SQL_SS_TIME2|'чч:мм:сс[.9999999]'<br /><br /> Дополнительно можно указывать доли секунд до семи цифр.|  
|Datetime2|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|"гггг-мм-дд чч: мм: СС [. 9999999]"<br /><br /> Дополнительно можно указывать доли секунд до семи цифр.|  
|DatetimeOFFSET|SQL_SS_TIMESTAMPOFFSET|'yyyy-mm-dd hh:mm:ss[.9999999] +/- hh:mm'<br /><br /> Дополнительно можно указывать доли секунд до семи цифр.|  
||||

 В escape-последовательностях ODBC для литералов даты и времени изменений нет.  
  
 Для обозначения долей секунды в результатах всегда используется точка (.), а не двоеточие (:).  
  
 Возвращаемые приложениям строковые значения всегда имеют одинаковую длину для заданного столбца. Компонентам года, месяца, дня, часов, минут и секунд предшествуют ведущие нули для достижения максимальной длины, а в значениях datetime между датой и временем присутствует один пробел. Также один пробел имеется между временем и сдвигом часового пояса в значении datetimeoffset. Перед сдвигом часового пояса всегда стоит знак. Если сдвиг равен нулю, ставится плюс (+). Доли секунды при необходимости дополняются замыкающими нулями до заданной точности столбца. В столбцах datetime для долей секунды предусмотрены три цифры. В столбцах smalldatetime значение секунд всегда равно нулю, а цифр для долей секунд не предусмотрено.  
  
 Пустая строка не является допустимым литералом даты-времени, она не представляет значение NULL. Попытка преобразования пустой строки в значение даты-времени приведет к возникновению ошибки SQLState 22018 с сообщением «Недопустимое символьное значение для приведения».  
  
 При преобразовании из строки параметры ожидаются в том же формате. Однако, если часы и минуты, представляющие часовой пояс, равны нулю, то перед элементом может стоять как плюс, так и минус, а для долей секунд разрешено использовать замыкающие нули, дополняющие значение до 9 цифр. Время может завершаться десятичной запятой без указания цифр долей секунды.  
  
 В настоящее время драйвер допускает использование дополнительного пробела вокруг знаков препинания, а пробел между временем и часовым поясом является необязательным. Однако, в следующих выпусках это может быть изменено. Приложениям не следует полагаться на текущее поведение драйвера.  
  
## <a name="data-formats-data-structures"></a>Форматы данных: структуры данных  
 В описанных ниже структурах ODBC определяет следующие ограничения, накладываемые григорианским календарем.  
  
-   Диапазон месяцев — от 1 до 12 включительно.  
  
-   Диапазон поля даты — от 1 до количества дней в месяце включительно, он должен быть согласован с полями года и месяца с учетом високосного года.  
  
-   Диапазон часов — от 0 до 23 включительно.  
  
-   Диапазон минут — от 0 до 59 включительно.  
  
-   Диапазон секунд — от 0 до 61.9(...). Это позволяет использовать до двух корректировочных секунд для синхронизации со звездным временем.  
  
     Следует иметь в виду, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не допускает использования корректировочных секунд, поэтому значения секунд больше 59 приведут к серверной ошибке.  
  
 Реализация следующих существующих структур ODBC была изменена для поддержки новых типов данных времени и даты в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. При этом определения не изменились.  
  
-   DATE_STRUCT;  
  
-   TIME_STRUCT;  
  
-   TIMESTAMP_STRUCT;  
  
 Также добавлены две новых структуры.  
  
-   SQL_SS_TIME2_STRUCT  
  
-   SQL_SS_TIMESTAMPOFFSET_STRUCT  
  
### <a name="sql_ss_time2_struct"></a>SQL_SS_TIME2_STRUCT  
 Эта структура дополняется до 12 байт как в 32-разрядных, так и в 64-разрядных операционных системах.  
  
```cpp
typedef struct tagSS_TIME2_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;  
} SQL_SS_TIME2_STRUCT;  
```  
  
### <a name="sql_ss_timestampoffset_struct"></a>SQL_SS_TIMESTAMPOFFSET_STRUCT  
  
```cpp
typedef struct tagSS_TIMESTAMPOFFSET_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;  
   SQLSMALLINT timezone_hour;  
   SQLSMALLINT timezone_minute;  
} SQL_SS_TIMESTAMPOFFSET_STRUCT;  
```  
  
 Если **timezone_hour** является отрицательным, **timezone_minute** должны быть отрицательными или равными нулю. Если **timezone_hour** положительное, **timezone_minute** должен быть положительным или равным нулю. Если **timezone_hour** равен нулю, **timezone_minute** может иметь любое значение в диапазоне от-59 до + 59.  
  
## <a name="see-also"></a>См. также:  
 [Улучшения даты и времени &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  

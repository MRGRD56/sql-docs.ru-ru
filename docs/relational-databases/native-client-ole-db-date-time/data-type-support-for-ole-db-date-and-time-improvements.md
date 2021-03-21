---
description: Поддержка типов данных для улучшений даты и времени (поставщик собственного клиента OLE DB)
title: Поддержка типов данных для усовершенствований даты и времени (поставщик собственного клиента OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB], data type support
- OLE DB, date/time improvements
ms.assetid: d40e3fd6-9057-4371-8236-95cef300603e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7500e311e3c1694bcb958048c21ca12e154a8752
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748394"
---
# <a name="sql-server-native-client-data-type-support-for-ole-db-date-and-time-improvements"></a>SQL Server Native Client поддержка типов данных для улучшения OLE DB даты и времени
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В этом разделе содержатся сведения о типах OLE DB ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client), поддерживающих [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типы данных даты-времени.  
  
## <a name="data-type-mapping-in-rowsets-and-parameters"></a>Сопоставление типов данных в наборах строк и параметрах  
 Для поддержки серверов новых типов OLE DB предоставляет два новых типа данных: DBTYPE_DBTIME2 и DBTYPE_DBTIMESTAMPOFFSET. Следующая таблица отображает полное сопоставление типов серверов.  
  
|Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Тип данных OLE DB|Значение|  
|-----------------------------------------|----------------------|-----------|  
|DATETIME|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
|smalldatetime|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
|Дата|DBTYPE_DBDATE|133 (oledb.h)|  
|time|DBTYPE_DBTIME2|145 (sqlncli. h)|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|146 (sqlncli. h)|  
|datetime2|DBTYPE_DBTIMESTAMP|135 (oledb.h)|  
  
## <a name="data-formats-strings-and-literals"></a>Форматы данных: строки и литералы  
  
|Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Тип данных OLE DB|Формат строки для клиентских преобразований|  
|-----------------------------------------|----------------------|------------------------------------------|  
|DATETIME|DBTYPE_DBTIMESTAMP|'гггг-мм-дд чч:мм:сс:[.999]'<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для типа Datetime поддерживает значения долей секунды, состоящие из не более чем трех цифр.|  
|smalldatetime|DBTYPE_DBTIMESTAMP|'гггг-мм-дд чч:мм:сс'<br /><br /> Точность этого типа данных составляет одну минуту. При выводе данных секунды будут равны нулю, а при вводе данных они округляются сервером.|  
|Дата|DBTYPE_DBDATE|'гггг-мм-дд'|  
|time|DBTYPE_DBTIME2|'чч:мм:сс[.9999999]'<br /><br /> Дополнительно можно указывать доли секунд до семи цифр.|  
|datetime2|DBTYPE_DBTIMESTAMP|'гггг-мм-дд чч:мм:сс[.еееееее]'<br /><br /> Дополнительно можно указывать доли секунд до семи цифр.|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|'гггг-мм-дд чч:мм:сс[.еееееее] +/-чч:мм'<br /><br /> Дополнительно можно указывать доли секунд до семи цифр.|  
  
 В escape-последовательностях для литералов даты и времени изменений нет.  
  
 Для долей секунды в результатах используется точка (.), а не двоеточие (:).  
  
 Строковые значения, возвращаемые в приложения, всегда будут иметь одинаковую длину для данного столбца. Компоненты года, месяца, дня, часа, минуты и секунды дополняются ведущими нулями до максимальной длины. Между значениями даты и времени имеется точно один пробел, а также предусмотрен точно один пробел между значением времени и смещением часового пояса. Перед смещением часового пояса всегда должен стоять знак. Это знак «плюс» (+), если смещение равен нулю. Пробелы между знаком и значением смещения отсутствуют. Доли секунды дополняются замыкающими нулями при необходимости до заданной точности столбца, но не более. Для столбцов datetime количество цифр с обозначением долей секунды равно трем. Для столбцов smalldatetime цифры с обозначением долей секунды отсутствуют, а секунды всегда равны нулю.  
  
 Приложения позволяют применять более гибкие преобразования из строковых значений, а также обеспечивают возможность использования значений компонентов с меньшей шириной по сравнению с максимальной. Годы могут быть представлены цифрами в количестве от 1 до 4. Месяцы, дни, часы, минуты и секунды могут быть представлены 1 или 2 цифрами. Между значениями даты и времени, а также значением времени и смещением часового пояса может находиться произвольное число пробелов. Смещение, равное нулю часов и нулю минут, может иметь знак плюс или минус. Допускается использование замыкающих нулей для долей секунд вплоть до максимального количества цифр, равного 9. Время может завершаться десятичной запятой без указания цифр долей секунды.  
  
 Пустая строка не является допустимым литералом даты-времени, она не представляет значение NULL. Попытка преобразовать пустую строку в значение даты-времени приведет к ошибкам со значением SQLState, равным 22018, и сообщением «Недопустимое символьное значение для спецификации приведения».  
  
## <a name="data-formats-data-structures"></a>Форматы данных: структуры данных  
 В структурах, зависящих от поставщика OLE DB, на OLE DB налагаются те же ограничения, что и на ODBC. Следующие определения взяты из описания григорианского календаря.  
  
-   Диапазон месяцев — от 1 до 12 включительно.  
  
-   Диапазон поля даты — от 1 до количества дней в месяце включительно, он должен быть согласован с полями года и месяца с учетом високосного года.  
  
-   Диапазон часов — от 0 до 23 включительно.  
  
-   Диапазон минут — от 0 до 59 включительно.  
  
-   Диапазон секунд — от 0 до 59. Это позволяет использовать до двух корректировочных секунд для синхронизации со звездным временем.  
  
 Реализации следующих существующих структур OLE DB были изменены в целях совместимости с новыми типами данных даты и времени [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. При этом определения не изменились.  
  
-   DBTYPE_DATE (Тип автоматизации DATE. Имеет внутреннее представление **double**. Целая часть числа равна числу дней, прошедшему с 30 декабря 1899 г., а десятичная часть равна части дня. Точность этого типа составляет 1 секунду, поэтому значение масштаба фактически равно 0.)  
  
-   DBTYPE_DBDATE  
  
-   DBTYPE_DBTIME  
  
-   DBTYPE_DBTIMESTAMP (Поле дробной части определяется в OLE DB как число миллиардных долей секунды (наносекунд) и имеет диапазон от 0 до 999 999 999.)  
  
-   DBTYPE_FILETIME  
  
### <a name="dbtype_dbtime2"></a>DBTYPE_DBTIME2  
 Эта структура дополняется до 12 байт как в 32-разрядных, так и в 64-разрядных операционных системах.  
  
```  
typedef struct tagDBTIME2 {  
    USHORT hour;  
    USHORT minute;  
    USHORT second;  
    ULONG fraction;  
    } DBTIME2;  
```  
  
### <a name="dbtype_-dbtimestampoffset"></a>DBTYPE_ DBTIMESTAMPOFFSET  
  
```  
typedef struct tagDBTIMESTAMPOFFSET {  
    SHORT year;  
    USHORT month;  
    USHORT day;  
    USHORT hour;  
    USHORT minute;  
    USHORT second;  
    ULONG fraction;  
    SHORT timezone_hour;  
    SHORT timezone_minute;  
    } DBTIMESTAMPOFFSET;  
```  
  
 Если значение `timezone_hour` отрицательно, значение `timezone_minute` должно быть отрицательным или нулевым. Если значение `timezone_hour` положительно, значение `timezone minute` должно быть положительным или нулевым. Если значение `timezone_hour` равно нулю, `timezone minute` может содержать значение от -59 до +59.  
  
### <a name="ssvariant"></a>SSVARIANT  
 Эта структура изменена и теперь содержит новые типы DBTYPE_DBTIME2 и DBTYPE_DBTIMESTAMPOFFSET, а также шкалу дробных долей секунды для соответствующих типов.  
  
```  
struct SSVARIANT {  
   SSVARTYPE vt;  
   DWORD dwReserved1;  
   DWORD dwReserved2;  
   union {  
// ...  
      DBTIMESTAMP tsDateTimeVal;  
      DBDATE dDateVal;  
      struct _Time2Val {  
         DBTIME2 tTime2Val;  
         BYTE bScale;  
      } Time2Val;  
      struct _DateTimeVal {  
         DBTIMESTAMP tsDateTimeVal;  
         BYTE bScale;  
      } DateTimeVal;  
      struct _DateTimeOffsetVal {   
         DBTIMESTAMPOFFSET tsoDateTimeOffsetVal;  
         BYTE bScale;  
      } DateTimeOffsetVal;  
// ...  
   };  
};  
```  
  
 Кроме того, перечисление, связанное с типом шифрования SSVARIANT, который определяет тип перечисления, будет расширено следующим образом:  
  
```  
enum SQLVARENUM {  
// ...  
   // Datetime  
   VT_SS_DATETIME      = DBTYPE_DBTIMESTAMP,  
   VT_SS_SMALLDATETIME = 206,  
  
   VT_SS_DATE = DBTYPE_DBDATE,  
   VT_SS_TIME2 = DBTYPE_DBTIME2,  
   VT_SS_DATETIME2 = 212  
   VT_SS_DATETIMEOFFSET = DBTYPE_DBTIMESTAMPOFFSET  
};  
```  
  
 Приложения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполняющие миграцию на собственный клиент, который использует **sql_variant** и зависят от ограниченной точности **даты и времени** , должны быть обновлены, если базовая схема обновлена для использования **datetime2** , а не **DateTime**.  
  
 Макрос доступа SSVARIANT также расширен с помощью следующего дополнения:  
  
```  
#define V_SS_DATETIME2(X)       V_SS_UNION(X, DateTimeVal)  
#define V_SS_TIME2(X)           V_SS_UNION(X, Time2Val)  
#define V_SS_DATE(X)            V_SS_UNION(X, dDateVal)  
#define V_SS_DATETIMEOFFSET(X)  V_SS_UNION(X, DateTimeOffsetVal)  
```  
  
## <a name="data-type-mapping-in-itabledefinitioncreatetable"></a>Сопоставление типов данных в методе ITableDefinition::CreateTable  
 Следующее сопоставление типов используется со структурами DBCOLUMNDESC в ITableDefinition::CreateTable:  
  
|тип данных OLE DB (*wType*)|Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Примечания|  
|----------------------------------|-----------------------------------------|-----------|  
|DBTYPE_DBDATE|Дата||  
|DBTYPE_DBTIMESTAMP|**datetime2**(p)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента проверяет элемент *bScale* дбколумдеск, чтобы определить точность доли секунды.|  
|DBTYPE_DBTIME2|**time**(p)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента проверяет элемент *bScale* дбколумдеск, чтобы определить точность доли секунды.|  
|DBTYPE_DBTIMESTAMPOFFSET|**datetimeoffset**(p)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента проверяет элемент *bScale* дбколумдеск, чтобы определить точность доли секунды.|  
  
 Если приложение задает DBTYPE_DBTIMESTAMP в *wType*, оно может заменить сопоставление на **datetime2**, предоставив имя типа в *pwszTypeName*. Если указано **datetime**, *bScale* должен быть равен 3. Если указано **smalldatetime**, *bScale* должен быть равен 0. Если *bScale* не согласуется с *wType* и *pwszTypeName*, возвращается DB_E_BADSCALE.  
  
## <a name="see-also"></a>См. также:  
 [Улучшения функций даты и времени &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  

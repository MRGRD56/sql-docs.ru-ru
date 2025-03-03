---
description: time (Transact-SQL)
title: time (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- time_TSQL
- time
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- time [SQL Server]
- date and time [SQL Server], time
- data types [SQL Server], date and time
- time data type [SQL Server]
ms.assetid: 30a6c681-8190-48e4-94d0-78182290a402
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e51ebb8bb2a55551ca7a9be918e3d30f58a263a7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744614"
---
# <a name="time-transact-sql"></a>time (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Определяет время дня. Время без учета часового пояса в 24-часовом формате.  
  
  > [!NOTE]  
  > Сведения об Informatica предоставляются для клиентов PDW, использующих соединитель Informatica. 
  
## <a name="time-description"></a>Описание типа данных time  
  
|Property (Свойство)|Значение|  
|--------------|-----------|  
|Синтаксис|**time** [ (*fractional second scale*) ]|  
|Использование|DECLARE \@MyTime **time(7)**<br /><br /> CREATE TABLE Таблица1 ( Столбец1 **time(7)** )|  
|*fractional seconds scale*|Задает число знаков для долей секунды.<br /><br /> Может быть целым числом от 0 до 7. Для Informatica может быть целым числом от 0 до 3.<br /><br /> Длина дробной части по умолчанию равна 7 (100 нс).|  
|Формат строковых литералов по умолчанию<br /><br /> (используется для клиента нижнего уровня)|чч:мм:сс[.ннннннн] для Informatica)<br /><br /> Дополнительные сведения см. в разделе [Обратная совместимость для клиентов нижнего уровня](#BackwardCompatibilityforDownlevelClients).|  
|Диапазон|От 00:00:00.0000000 до 23:59:59.9999999 (от 00:00:00.000 до 23:59:59.999 для Informatica)|  
|Диапазоны элементов|Обозначение чч состоит из двух цифр, представляющих час, и принимает значения от 0 до 23.<br /><br /> Обозначение мм состоит из двух цифр, представляющих минуты, и принимает значения от 0 до 59.<br /><br /> Обозначение сс состоит из двух цифр, представляющих секунды, и принимает значения от 0 до 59.<br /><br /> Обозначение n\* может содержать от нуля до семи цифр, представляющих доли секунды, и принимает значения от 0 до 9999999. Для Informatica обозначение n\* может содержать от нуля до трех цифр и принимает значения от 0 до 999.|  
|Длина в символах|От 8 позиций (чч:мм:сс) до 16 позиций (чч:мм:сс.ннннннн). Для Informatica максимальная длина равна 12 (чч:мм:сс.ннн).|  
|Точность, масштаб<br /><br /> (только пользовательский масштаб)|См. таблицу ниже.|  
|Объем памяти|5 байт, по умолчанию используется фиксированная точность 100 нс. В Informatica размер по умолчанию — 4 байта; по умолчанию используется фиксированная точность 1 мс.|  
|Точность|100 наносекунд (1 миллисекунда в Informatica)|  
|Значение по умолчанию|00:00:00<br /><br /> Это значение используется как присоединяемая часть даты при неявном преобразовании данных типа **date** в значение типа **datetime2** или **datetimeoffset**.|  
|Определяемая пользователем точность в долях секунды|Да|  
|Учет и сохранение смещения часового пояса|Нет|  
|Учет перехода на летнее время|Нет|  
  
|Указанный масштаб|Результат (точность, масштаб)|Длина столбца (в байтах)|Дробная часть<br /><br /> секунд<br /><br /> точность|  
|---------------------|---------------------------------|-----------------------------|------------------------------------------|  
|**time**|(16,7) [(12,3) в Informatica]|5 (4 в Informatica)|7 (3 в Informatica)|  
|**time(0)**|(8,0)|3|0–2|  
|**time(1)**|(10,1)|3|0–2|  
|**time(2)**|(11,2)|3|0–2|  
|**time(3)**|(12,3)|4|3–4|  
|**time(4)**<br /><br /> Не поддерживается в Informatica.|(13,4)|4|3–4|  
|**time(5)**<br /><br /> Не поддерживается в Informatica.|(14,5)|5|5–7|  
|**time(6)**<br /><br /> Не поддерживается в Informatica.|(15,6)|5|5–7|  
|**time(7)**<br /><br /> Не поддерживается в Informatica.|(16,7)|5|5–7|  
  
## <a name="supported-string-literal-formats-for-time"></a>Поддерживаемые форматы строковых литералов для типа данных time  
 В таблице ниже приводятся допустимые форматы строковых литералов для типа данных **time**.  
  
|SQL Server|Описание|  
|----------------|-----------------|  
|чч:мм[сс][:доли секунд][AM][PM]<br /><br /> чч:мм[сс][.доли секунд][AM][PM]<br /><br /> ччAM[PM]<br /><br /> чч AM[PM]|Значение часа 0 означает час после полуночи (AM), независимо от указания литерала «AM». Если час равен 0, «PM» указывать нельзя.<br /><br /> Значения часа от 01 до 11 представляют часы до полудня, если не задан параметр AM или PM. Если задан параметр «AM», то эти значения так же представляют часы до полудня. Если указано «PM», то эти значения указывают на часы после полудня.<br /><br /> Значение 12 представляет час, начавшийся в полдень, если не указано «PM» или «AM». Если указано «AM», это значение представляет час, начавшийся в полночь. Если указано «PM», то это значение представляет час, начавшийся в полдень. Например, 12:01 — это 1 минута после полудня, так же как и 12:01 PM, тогда как 12:01 AM — это 1 минута после полуночи. 12:01 АМ аналогично указанию 00:01 или 00:01 AM.<br /><br /> Значения часов от 13 до 23 представляют часы после полудня, если не указано «AM» или «PM». Если задан параметр «PM», то эти значения также представляют часы после полудня. Если час принимает значение от 13 до 23, то указывать «AM» нельзя.<br /><br /> Значение часа, равное 24, недопустимо. Для представления полночи используется 12:00 AM или 00:00.<br /><br /> Миллисекундам может предшествовать либо двоеточие (:), либо точка (.). Число после двоеточия обозначает тысячные доли секунды. При использовании точки однозначное число обозначает десятые доли секунды, двузначное число — сотые, а трехзначное — тысячные доли секунды. Например: 12:30:20:1 означает 20 секунд и одну тысячную долю секунды после 12:30, 12:30:20.1 означает 20 секунд и одну десятую секунды после 12:30.|  
  
|ISO 8601|Примечания|  
|--------------|-----------|  
|чч:мм:сс<br /><br /> чч:мм[:сс][.доли секунды]|чч — двузначное число от 0 до 23, представляющее количество часов в смещении часового пояса.<br /><br /> обозначение мм состоит из двух цифр, представляющих дополнительное смещение часового пояса в минутах, и принимает значения от 0 до 59;|  
  
|ODBC|Примечания|  
|----------|-----------|  
|{ t 'чч-мм-сс[.доли секунды]'}|Зависит от API-интерфейса ODBC.|  
  
## <a name="compliance-with-ansi-and-iso-8601-standards"></a>Соответствие стандартам ANSI и ISO 8601  
 Использование 24-часового формата и секунды координации свыше 59 согласно стандарту ISO 8601 (5.3.2 и 5.3) не поддерживается с целью обратной совместимости и соответствия существующим форматам даты и времени.  
  
 Формат строковых литералов по умолчанию (который используется для клиента нижнего уровня) соответствует стандарту языка SQL, определенному в форме чч.мм:сс[.ннннннн]. Такой формат напоминает определение стандарта ISO 8601 для типа TIME, за исключением долей секунд.  
  
##  <a name="backward-compatibility-for-down-level-clients"></a><a name="BackwardCompatibilityforDownlevelClients"></a> Обратная совместимость для клиентов нижнего уровня  
 Некоторые клиенты нижнего уровня не поддерживают типы данных **time**, **date**, **datetime2** и **datetimeoffset**. В следующей таблице показано сопоставление типов экземпляра более высокого уровня [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и клиентов низкого уровня.  
  
|Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Формат строкового литерала по умолчанию, передаваемый клиенту низкого уровня|ODBC низкого уровня|OLEDB низкого уровня|JDBC низкого уровня|SQLCLIENT низкого уровня|  
|-----------------------------------------|----------------------------------------------------------------|----------------------|-----------------------|----------------------|---------------------------|  
|**time**|чч:мм:сс[.ннннннн]|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
|**date**|ГГГГ-ММ-ДД|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
|**datetime2**|ГГГГ-ММ-ДД чч:мм:сс[.ннннннн]|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
|**datetimeoffset**|ГГГГ-ММ-ДД чч:мм:сс[.ннннннн] [+&#124;-]чч:мм|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
  
## <a name="converting-date-and-time-data"></a>Преобразование данных типа Date и Time  
 При преобразовании в типы данных даты и времени [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отвергает все значения, которые он не распознает как значения даты или времени. Сведения об использовании функций CAST и CONVERT c данными типов даты и времени см. в статье [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
### <a name="converting-timen-data-type-to-other-date-and-time-types"></a>Преобразование типа данных time(n) в другие типы данных даты и времени  
 В этом разделе описывается, что происходит при преобразовании типа данных **time** в другие типы даты и времени.  
  
 При преобразовании в тип **time(n)** копируются часы, минуты и секунды. Если целевая точность меньше исходной точности, доли секунд будут округлены в сторону увеличения, чтобы соответствовать целевой точности. Следующий пример показывает результаты преобразования значения `time(4)` в значение `time(3)`.  
  
```sql
DECLARE @timeFrom time(4) = '12:34:54.1237';  
DECLARE @timeTo time(3) = @timeFrom;  
  
SELECT @timeTo AS 'time(3)', @timeFrom AS 'time(4)';  
  
--Results  
--time(3)      time(4)  
-------------- -------------  
--12:34:54.124 12:34:54.1237  
--  
--(1 row(s) affected)  
```  
  
 Преобразование в **date** завершается сбоем, и появляется сообщение об ошибке 206: "Конфликт типов операндов: date несовместим с time".  
  
 При преобразовании в тип **datetime** значения часов, минут и секунд копируются; для компонента даты устанавливается значение "1900-01-01". Если точность в долях секунды значения типа **time(n)** больше трех цифр, результат типа **datetime** будет усечен. Следующий код демонстрирует результаты преобразования значения `time(4)` в значение `datetime`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime datetime= @time;  
SELECT @time AS '@time', @datetime AS '@datetime';  
  
--Result  
--@time         @datetime  
--------------- -----------------------  
--12:15:04.1237 1900-01-01 12:15:04.123  
--  
--(1 row(s) affected)  
  
```  
  
 При преобразовании в тип **smalldatetime** значения часов и минут округляются в сторону увеличения, и для даты устанавливается значение "1900-01-01". Секунды и доли секунд устанавливаются в значение 0. Следующий код демонстрирует результаты преобразования значения `time(4)` в значение `smalldatetime`.  
  
```sql
-- Shows rounding up of the minute value.  
DECLARE @time time(4) = '12:15:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';   
  
--Result  
@time            @smalldatetime  
---------------- -----------------------  
12:15:59.9999    1900-01-01 12:16:00--  
--(1 row(s) affected)  
  
-- Shows rounding up of the hour value.  
DECLARE @time time(4) = '12:59:59.9999';   
DECLARE @smalldatetime smalldatetime= @time;    
  
SELECT @time AS '@time', @smalldatetime AS '@smalldatetime';  
@time            @smalldatetime  
---------------- -----------------------  
12:59:59.9999    1900-01-01 13:00:00  
  
(1 row(s) affected)  
  
```  
  
 При преобразовании в **datetimeoffset(n)** время копируется, а для даты устанавливается значение "1900-01-01". Для смещения часового пояса устанавливается значение +00:00. Если точность в долях секунды значения типа **time(n)** больше, чем точность значения типа **datetimeoffset(n)**, значение округляется в сторону увеличения. В следующем примере демонстрируются результаты преобразования значения типа `time(4)` в тип `datetimeoffset(3)`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetimeoffset datetimeoffset(3) = @time;  
  
SELECT @time AS '@time', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@time         @datetimeoffset  
--------------- ------------------------------  
--12:15:04.1237 1900-01-01 12:15:04.124 +00:00  
--  
--(1 row(s) affected)  
  
```  
  
 При преобразовании в тип **datetime2(n)** для даты устанавливается значение 1900-01-01, компонент времени копируется, а для смещения часового пояса устанавливается значение 00:00. Если точность в долях секунды значения типа **datetime2(n)** больше точности значения типа **time(n)**, доли секунды округляются в сторону увеличения.  Следующий пример показывает результаты преобразования значения `time(4)` в значение `datetime2(2)`.  
  
```sql
DECLARE @time time(4) = '12:15:04.1237';  
DECLARE @datetime2 datetime2(3) = @time;  
  
SELECT @datetime2 AS '@datetime2', @time AS '@time';  
  
--Result  
--@datetime2              @time  
------------------------- -------------  
--1900-01-01 12:15:04.124 12:15:04.1237  
--  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-timen"></a>Преобразование строковых литералов в тип time(n)  
 Преобразование строковых литералов в типы данных даты и времени разрешается, если все части строк записаны в допустимом формате. Иначе возникает ошибка времени выполнения. Явные или скрытые преобразования, в которых не задан стиль преобразования типов данных даты и времени в строковые литералы, будут проведены в формате по умолчанию для текущего сеанса. В таблице ниже приводятся правила преобразования строковых литералов в тип данных **time**.  
  
|Строковый литерал входа|Правило преобразования|  
|--------------------------|---------------------|  
|ODBC DATE|Строковые литералы ODBC сопоставляются с типом данных **datetime**. Любая операция присваивания литералов ODBC DATETIME типу данных **time** вызывает неявное преобразование между данным типом и типом **datetime** согласно правилам преобразования.|  
|ODBC TIME|См. правило ODBC DATE выше.|  
|ODBC DATETIME|См. правило ODBC DATE выше.|  
|только DATE|Указаны значения по умолчанию.|  
|только TIME|Простейший.|  
|только TIMEZONE|Указаны значения по умолчанию.|  
|DATE + TIME|Используется компонент TIME входной строки.|  
|DATE + TIMEZONE|Не допускается.|  
|TIME + TIMEZONE|Используется компонент TIME входной строки.|  
|DATE + TIME + TIMEZONE|Используется компонент TIME локального значения DATETIME.|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-comparing-date-and-time-data-types"></a>A. Сравнение типов данных даты и времени  
 В приведенном ниже примере сравниваются результаты приведения строкового типа к каждому из типов данных **date** и **time**.  
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
|Тип данных|Выходные данные|  
|---------------|------------|  
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
###  <a name="b-inserting-valid-time-string-literals-into-a-time7-column"></a><a name="ExampleB"></a> Б. Вставка допустимых строковых литералов времени в столбец time(7)  
 В таблице ниже приводится список строковых литералов, которые можно вставлять в столбец с типом данных **time(7)** вместе со значениями, хранящимися в этом столбце.  
  
|Формат строковых литералов|Вставляемый строковый литерал|Хранящееся значение time(7)|Описание|  
|--------------------------------|-----------------------------|------------------------------------|-----------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01:123AM'|01:01:01.1230000|Если перед долями секунд стоит двоеточие (:), масштаб не может превышать трех позиций, иначе возникает ошибка.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 AM'|01:01:01.1234567|Если указан параметр «AM» или «PM», время сохраняется в 24-часовом формате без литерала «AM» или «PM».|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567 AM'|13:01:01.1234567|Если указан параметр «AM» или «PM», время сохраняется в 24-часовом формате без литерала «AM» или «PM».|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01.1234567PM'|13:01:01.1234567|Пробел перед литералом «AM» или «PM» является необязательным.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01AM'|01:00:00.0000000|Если задан только час, все остальные значения равны 0.|  
|SQL Server|'01 AM'|01:00:00.0000000|Пробел перед литералом «AM» или «PM» является необязательным.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|'01:01:01'|01:01:01.0000000|Если не заданы доли секунды, каждая позиция, определяемая этим типом данных, равна 0.|  
|ISO 8601|'01:01:01.1234567'|01:01:01.1234567|Для соответствия стандарту ISO 8601 используется 24-часовой формат, а не литералы «AM» и «PM».|  
|ISO 8601|'01:01:01.1234567 +01:01'|01:01:01.1234567|Необязательная разница во времени (TZD) во входных данных разрешается, но не сохраняется.|  
  
### <a name="c-inserting-time-string-literal-into-columns-of-each-date-and-time-date-type"></a>В. Вставка строкового литерала времени в столбцы каждого типа данных date и time  
 В приведенной ниже таблице в первом столбце показан строковый литерал времени, который должен вставляться в столбец базы данных с типом данных date или time, представленный во втором столбце. В третьем столбце показано значение, которое будет сохранено в базе данных.  
  
|Вставляемый строковый литерал|Тип данных столбца|Значение, хранящееся в столбце|Описание|  
|-----------------------------|----------------------|------------------------------------|-----------------|  
|'12:12:12.1234567'|**time(7)**|12:12:12.1234567|Если точность в долях секунды превышает значение, указанное для столбца, строка будет усечена без сообщения об ошибке.|  
|'2007-05-07'|**date**|NULL|Любое значение времени вызовет ошибку инструкции INSERT.|  
|'12:12:12'|**smalldatetime**|1900-01-01 12:12:00|Любое значение долей секунды вызовет ошибку инструкции INSERT.|  
|'12:12:12.123'|**datetime**|1900-01-01 12:12:12.123|Любое значение долей секунды длиннее трех позиций вызовет ошибку инструкции INSERT.|  
|'12:12:12.1234567'|**datetime2(7)**|1900-01-01 12:12:12.1234567|Если точность в долях секунды превышает значение, указанное для столбца, строка будет усечена без сообщения об ошибке.|  
|'12:12:12.1234567'|**datetimeoffset(7)**|1900-01-01 12:12:12.1234567 +00:00|Если точность в долях секунды превышает значение, указанное для столбца, строка будет усечена без сообщения об ошибке.|  
  
## <a name="see-also"></a>См. также:  
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
  

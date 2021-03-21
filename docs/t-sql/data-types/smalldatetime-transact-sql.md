---
description: smalldatetime (Transact-SQL)
title: smalldatetime (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- smalldatetime_TSQL
- smalldatetime
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- smalldatetime data type [SQL Server]
- dates [SQL Server], data types
- date and time [SQL Server], smalldatetime
- data types [SQL Server], date and time
ms.assetid: 68b74610-d54c-4c8e-b4b2-7e3747546ee0
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24de52b89c597167c8ba6b9f7448701b637bba87
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104736854"
---
# <a name="smalldatetime-transact-sql"></a>smalldatetime (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Определяет дату, сочетающуюся с временем дня. Время представлено в 24-часовом формате с секундами, всегда равными нулю (:00), без долей секунд.
  
> [!NOTE]  
>  Используйте для новых проектов типы данных **time**, **date**, **datetime2** и **datetimeoffset**. Эти типы соответствуют стандарту языка SQL. Их проще переносить на другие платформы. Типы **time**, **datetime2** и **datetimeoffset** обеспечивают большую точность секунд. **datetimeoffset** обеспечивает поддержку часовых поясов для приложений, развертываемых по всему миру.  
  
## <a name="smalldatetime-description"></a>Описание типа данных smalldatetime
  
|Свойство|Значение|
|--------|-----|
|Синтаксис|**smalldatetime**|  
|Использование|DECLARE \@MySmalldatetime **smalldatetime**<br /><br /> CREATE TABLE Таблица1 ( Столбец1 **smalldatetime** )|  
|Форматы строковых литералов по умолчанию<br /><br /> (используется для клиента нижнего уровня)|Неприменимо|  
|Диапазон даты|От 01.01.1900 до 06.06.2079<br /><br /> 1 января 1900 года — 6 июня 2079 года|  
|Диапазон времени|От 00:00:00 до 23:59:59<br /><br /> 2007-05-09 23:59:59 округляется до<br /><br /> 2007-05-10 00:00:00|  
|Диапазоны элементов|ГГГГ — четырехзначное число от 1900 до 2079, представляющее год.<br /><br /> ММ обозначает 2 цифры, которые представляют месяц и принимают значения от 01 до 12.<br /><br /> Обозначение ДД состоит из двух цифр, представляющих день указанного месяца, и принимает значения от 01 до 31 в зависимости от месяца.<br /><br /> Обозначение чч состоит из двух цифр, представляющих час, и принимает значения от 00 до 23.<br /><br /> Обозначение мм состоит из двух цифр, представляющих минуту, и принимает значения от 00 до 59.<br /><br /> Обозначение сс состоит из двух цифр, представляющих секунду, и принимает значения от 00 до 59. Значения 29,998 с или меньше округляются до ближайшей минуты. Значения 29,999 с или больше округляются до ближайшей минуты.|  
|Длина в символах|Максимально 19 позиций|  
|Объем памяти|4 байта, фиксированный.|  
|Точность|Одна минута|  
|Значение по умолчанию|1900-01-01 00:00:00|  
|Календарь|Григорианский<br /><br /> (Не включает полный диапазон лет.)|  
|Определяемая пользователем точность в долях секунды|Нет|  
|Учет и сохранение смещения часового пояса|Нет|  
|Учет перехода на летнее время|Нет|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Соответствие стандартам ANSI и ISO 8601  
**smalldatetime** не удовлетворяет стандартам ANSI и ISO 8601.
  
## <a name="converting-date-and-time-data"></a>Преобразование данных типа Date и Time
При преобразовании в типы данных даты и времени [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отбрасывает все значения, которые не распознаются как значения даты или времени. Сведения об использовании функций CAST и CONVERT c данными типов даты и времени см. в статье [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-smalldatetime-to-other-date-and-time-types"></a>Преобразование типа smalldatetime в другие типы данных даты и времени
В этом разделе описывается, что происходит при преобразовании типа данных **smalldatetime** в другие типы даты и времени.
  
Для преобразования в тип **date** копируются год, месяц и день. Следующий код демонстрирует результаты преобразования значения `smalldatetime` в значение `date`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @date date = @smalldatetime  
  
SELECT @smalldatetime AS '@smalldatetime', @date AS 'date';  
  
--Result  
--@smalldatetime          date  
------------------------- ----------  
--1955-12-13 12:43:00     1955-12-13  
--  
--(1 row(s) affected)  
```  
  
При преобразовании в тип **time(n)** копируются часы, минуты и секунды. Доли секунды устанавливаются в значение 0. Следующий код демонстрирует результаты преобразования значения `smalldatetime` в значение `time(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @time time(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @time AS 'time';  
  
--Result  
--@smalldatetime          time  
------------------------- -------------  
--1955-12-13 12:43:00     12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
При преобразовании в тип **datetime** значение **smalldatetime** копируется в значение **datetime**. Доли секунды устанавливаются в значение 0. Следующий код демонстрирует результаты преобразования значения `smalldatetime` в значение `datetime`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime datetime = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime AS 'datetime';  
  
--Result  
--@smalldatetime          datetime  
------------------------- -----------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.000  
--  
--(1 row(s) affected)  
```  
  
Для преобразования в тип **datetimeoffset(n)** значение **smalldatetime** копируется в значение **datetimeoffset(n)** . Для долей секунды устанавливается значение 0; для смещения часового пояса устанавливается значение +00:0. Следующий код демонстрирует результаты преобразования значения `smalldatetime` в значение `datetimeoffset(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetimeoffset datetimeoffset(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetimeoffset AS 'datetimeoffset(4)';  
  
--Result  
--@smalldatetime          datetimeoffset(4)  
------------------------- ------------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000 +00:0  
--  
--(1 row(s) affected)  
```  
  
При преобразовании в тип **datetime2(n)** значение **smalldatetime** копируется в значение **datetime2(n)** . Доли секунды устанавливаются в значение 0. Следующий код демонстрирует результаты преобразования значения `smalldatetime` в значение `datetime2(4)`.
  
```sql
DECLARE @smalldatetime smalldatetime = '1955-12-13 12:43:10';  
DECLARE @datetime2 datetime2(4) = @smalldatetime;  
  
SELECT @smalldatetime AS '@smalldatetime', @datetime2 AS ' datetime2(4)';  
  
--Result  
--@smalldatetime           datetime2(4)  
------------------------- ------------------------  
--1955-12-13 12:43:00     1955-12-13 12:43:00.0000  
--  
--(1 row(s) affected)  
```  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-casting-string-literals-with-seconds-to-smalldatetime"></a>A. Приведение строковых литералов с секундами к типу smalldatetime  
В следующем примере сравнивается преобразование секунд в строковых литералах в тип данных `smalldatetime`.
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29'     AS smalldatetime)  
    ,CAST('2007-05-08 12:35:30'     AS smalldatetime)  
    ,CAST('2007-05-08 12:59:59.998' AS smalldatetime);  
```  
  
|Входные данные|Вывод|  
|---|---|
|2007-05-08 12:35:29|2007-05-08 12:35:00|  
|2007-05-08 12:35:30|2007-05-08 12:36:00|  
|2007-05-08 12:59:59.998|2007-05-08 13:00:00|  
  
### <a name="b-comparing-date-and-time-data-types"></a>Б. Сравнение типов данных даты и времени  
В следующем примере сравниваются результаты приведения строкового типа к каждому из типов данных date и time.
  
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
|---|---|
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  

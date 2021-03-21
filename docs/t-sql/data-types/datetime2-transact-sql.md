---
description: datetime2 (Transact-SQL)
title: datetime2 (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- datetime2
- datetime2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- time [SQL Server], data types
- dates [SQL Server], data types
- date and time [SQL Server], datetime2
- data types [SQL Server], date and time
- datetime2 data type [SQL Server]
ms.assetid: 868017f3-214f-43ef-8536-cc1632a2288f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ad0fb7d21621f83370dfcef29ea57b8b1cf4680b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747714"
---
# <a name="datetime2-transact-sql"></a>datetime2 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Определяет дату, включающую время суток в 24-часовом формате. Тип данных **datetime2** может рассматриваться как расширение существующего типа **datetime**, имеющее более широкий диапазон дат, большую точность в долях секунды по умолчанию и дополнительную точность, определяемую пользователем.
  
## <a name="datetime2-description"></a>Описание типа данных datetime2
  
|Property (Свойство)|Значение|  
|--------------|-----------|  
|Синтаксис|**datetime2** [ (*fractional seconds precision*) ]|  
|Использование|DECLARE \@MyDatetime2 **datetime2(7)**<br /><br /> CREATE TABLE Таблица1 ( Столбец1 **datetime2(7)** )|  
|Формат строковых литералов по умолчанию<br /><br /> (используется для клиента нижнего уровня)|ГГГГ-ММ-ДД чч:мм:сс[.доли секунды]<br /><br /> Дополнительные сведения см. в подразделе «Обратная совместимость для клиентов низкого уровня» следующего раздела.|  
|Диапазон даты|От 0001-01-01 до 31.12.99<br /><br /> С 1 января 1 года нашей эры до 31 декабря 9999 года нашей эры|  
|Диапазон времени|от 00:00:00 до 23:59:59.9999999|  
|Диапазон смещения часового пояса|None|  
|Диапазоны элементов|ГГГГ представляет собой четырехзначное число от 0001 до 9999, определяющее год.<br /><br /> ММ — двузначное число от 01 до 12, представляющее месяц указанного года.<br /><br /> Обозначение ДД состоит из двух цифр, представляющих день указанного месяца, и принимает значения от 01 до 31 в зависимости от месяца.<br /><br /> Обозначение «чч» состоит из двух цифр, представляющих час, и принимает значения от 00 до 23.<br /><br /> Обозначение «мм» состоит из двух цифр, представляющих минуту, и принимает значения от 00 до 59.<br /><br /> Обозначение «сс» состоит из двух цифр, представляющих секунду, и принимает значения от 00 до 59.<br /><br /> Обозначение n* может содержать от нуля до семи цифр, представляющих доли секунды, и принимает значения от 0 до 9999999. В Informatica доли секунды усекаются при n > 3.|  
|Длина в символах|Минимальная — 19 позиций (ГГГГ-ММ-ДД чч:мм:сс), максимальная — 27 позиций ((ГГГГ-ММ-ДД чч:мм:сс.0000000)|  
|Точность, масштаб|От 0 до 7 цифр, с точностью 100 нс. Точность по умолчанию составляет 7 цифр.|  
|Объем памяти <sup>1</sup>|6 байтов для представления точности меньше 3 цифр.<br/>7 байтов — для точности в 3 или 4 цифры.<br/>Для представления любых других значений точности требуется 8 байт <sup>2</sup>.|  
|Точность|100 наносекунд|  
|Значение по умолчанию|1900-01-01 00:00:00|  
|Календарь|Григорианский|  
|Определяемая пользователем точность в долях секунды|Да|  
|Учет и сохранение смещения часового пояса|Нет|  
|Учет перехода на летнее время|Нет|  

<sup>1</sup> Указанные значения относятся к несжатым rowstore. Использование [сжатия данных](../../relational-databases/data-compression/data-compression.md) или [columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md) может изменить размер хранилища для каждого уровня точности. Кроме того, размер хранилища на диске и в памяти может различаться. Например, значения **datetime2** при использовании пакетного режима всегда требует 8 байт в памяти.

<sup>2</sup> При приведении значения **datetime2** к значению **varbinary** к значению **varbinary** добавляется дополнительный байт для сохранения точности.

Сведения о метаданных типа данных см. в статье [sys.systypes (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md) или [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md). В некоторых типах данных дат и времени точность и масштаб разные. Сведения о получении точности и масштаба для столбца см. в статье [COLUMNPROPERTY (Transact-SQL)](../../t-sql/functions/columnproperty-transact-sql.md), [COL_LENGTH (Transact-SQL)](../../t-sql/functions/col-length-transact-sql.md) или [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).
  
## <a name="supported-string-literal-formats-for-datetime2"></a>Поддерживаемые форматы строковых литералов для типа данных datetime2
В таблицах ниже приводятся поддерживаемые форматы строковых литералов ISO 8601 и ODBC для типа данных **datetime2**. Сведения об алфавитных и числовых форматах, форматах строки без разделителей и форматах времени для частей даты и времени типа **datetime2** см. в статьях [date (Transact-SQL)](../../t-sql/data-types/date-transact-sql.md) и [time (Transact-SQL)](../../t-sql/data-types/time-transact-sql.md).
  
|ISO 8601|Описания|  
|---|---|
|ГГГГ-ММ-ДДТчч:мм:сс[.nnnnnnn]<br /><br /> ГГГГ-ММ-ДДТчч:мм:сс[.nnnnnnn]|На этот формат не влияют настройки локали сеанса инструкций SET LANGUAGE и SET DATEFORMAT. Символы **T**, двоеточие (:) и точка (.) включаются в строковый литерал, например "2007-05-02T19:58:47.1234567".|  
  
|ODBC|Описание|  
|---|---|
|{ ts 'гггг-мм-дд чч:мм:сс[.доли секунды]' }|Зависит от API-интерфейса ODBC.<br /><br /> Можно указать от 0 до 7 знаков (100 наносекунд) справа от десятичной запятой, представляющих доли секунды.|  
  
## <a name="ansi-and-iso-8601-compliance"></a>Соответствие стандартам ANSI и ISO 8601  
Соглашения стандартов ANSI и ISO 8601 для типов данных [date](../../t-sql/data-types/date-transact-sql.md) и [time](../../t-sql/data-types/time-transact-sql.md) применимы к типу данных **datetime2**.
  
##  <a name="backward-compatibility-for-down-level-clients"></a>Обратная совместимость для клиентов нижнего уровня  
Некоторые клиенты нижнего уровня не поддерживают типы данных **time**, **date**, **datetime2** и **datetimeoffset**. В следующей таблице показано сопоставление типов экземпляра более высокого уровня [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и клиентов низкого уровня.
  
|Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Формат строкового литерала по умолчанию, передаваемый клиенту низкого уровня|ODBC низкого уровня|OLEDB низкого уровня|JDBC низкого уровня|SQLCLIENT низкого уровня|  
| --- | --- | --- | --- | --- | --- |
|**time**|чч:мм:сс[.ннннннн]|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
|**date**|ГГГГ-ММ-ДД|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
|**datetime2**|ГГГГ-ММ-ДД чч:мм:сс[.ннннннн]|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
|**datetimeoffset**|ГГГГ-ММ-ДД чч:мм:сс[.ннннннн] [+&#124;-]чч:мм|SQL_WVARCHAR или SQL_VARCHAR|DBTYPE_WSTR или DBTYPE_STR|Java.sql.String|String или SqString|  
  
## <a name="converting-date-and-time-data"></a>Преобразование данных типа Date и Time
При преобразовании в типы данных даты и времени [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отвергает все значения, которые он не распознает как значения даты или времени. Сведения об использовании функций CAST и CONVERT c данными типов даты и времени см. в статье [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md).
  
### <a name="converting-other-date-and-time-types-to-the-datetime2-data-type"></a>Преобразование других типов даты и времени в тип данных datetime2
В этом разделе описывается, что происходит при преобразовании других типов даты и времени в тип данных **datetime2**.  
  
При преобразовании из типа **date** копируются год, месяц и день.  Для компонента времени устанавливается значение 00:00:00.0000000.  Следующий код демонстрирует результаты преобразования значения `date` в значение `datetime2`.  
  
```sql
DECLARE @date date = '12-21-16';
DECLARE @datetime2 datetime2 = @date;

SELECT @datetime2 AS '@datetime2', @date AS '@date';
  
--Result  
--@datetime2                  @date
----------------------------- ----------
--2016-12-21 00:00:00.0000000 2016-12-21
```  
  
При преобразовании из **time(n)** компонент времени копируется, а для компонента даты устанавливается значение 1900-01-01. Следующий пример показывает результаты преобразования значения `time(7)` в значение `datetime2`.  
  
```sql
DECLARE @time time(7) = '12:10:16.1234567';
DECLARE @datetime2 datetime2 = @time;

SELECT @datetime2 AS '@datetime2', @time AS '@time';
  
--Result  
--@datetime2                  @time
----------------------------- ----------------
--1900-01-01 12:10:16.1234567 12:10:16.1234567
```  
  
При преобразовании из типа **smalldatetime** копируются часы и минуты. Секунды и доли секунд устанавливаются в значение 0. Следующий код демонстрирует результаты преобразования значения `smalldatetime` в значение `datetime2`.  
  
```sql
DECLARE @smalldatetime smalldatetime = '12-01-16 12:32';
DECLARE @datetime2 datetime2 = @smalldatetime;

SELECT @datetime2 AS '@datetime2', @smalldatetime AS '@smalldatetime'; 
  
--Result  
--@datetime2                  @smalldatetime
----------------------------- -----------------------
--2016-12-01 12:32:00.0000000 2016-12-01 12:32:00 
```  
  
При преобразовании из типа **datetimeoffset(n)** копируются компоненты даты и времени. Часовой пояс усекается. Следующий пример показывает результаты преобразования значения `datetimeoffset(7)` в значение `datetime2`.  
  
```sql
DECLARE @datetimeoffset datetimeoffset(7) = '2016-10-23 12:45:37.1234567 +10:0';
DECLARE @datetime2 datetime2 = @datetimeoffset;

SELECT @datetime2 AS '@datetime2', @datetimeoffset AS '@datetimeoffset'; 
  
--Result  
--@datetime2                  @datetimeoffset
----------------------------- ----------------------------------
--2016-10-23 12:45:37.1234567 2016-10-23 12:45:37.1234567 +10:00
```  

При преобразовании из типа **datetime** копируются дата и время. Точность в долях увеличивается до 7 цифр. Следующий пример показывает результаты преобразования значения `datetime` в значение `datetime2`.

```sql
DECLARE @datetime datetime = '2016-10-23 12:45:37.333';
DECLARE @datetime2 datetime2 = @datetime;

SELECT @datetime2 AS '@datetime2', @datetime AS '@datetime';
   
--Result  
--@datetime2                  @datetime
------------------------- ---------------------------
--2016-10-23 12:45:37.3333333 2016-10-23 12:45:37.333
```  

> [!NOTE]
> При уровне совместимости базы данных 130 неявные преобразования типов данных из datetime в datetime2 демонстрируют повышенную точность благодаря учету долей миллисекунд. В результате преобразования дают иные значения, как показано в примере выше. Всегда используйте явное приведение к типу данных datetime2, когда имеется сценарий смешанного сравнения типов данных datetime и datetime2. Дополнительные сведения см. в [этой статье](https://support.microsoft.com/help/4010261) на сайте службы поддержки Майкрософт.

### <a name="converting-string-literals-to-datetime2"></a>Преобразование строковых литералов в datetime2  
Преобразование строковых литералов в типы данных даты и времени разрешается, если все части строк записаны в допустимом формате. Иначе возникает ошибка времени выполнения. Явные или скрытые преобразования, в которых не задан стиль преобразования типов данных даты и времени в строковые литералы, будут проведены в формате по умолчанию для текущего сеанса. В таблице ниже приводятся правила преобразования строковых литералов в тип данных **datetime2**.
  
|Строковый литерал входа|**datetime2(n)**|  
|---|---|
|ODBC DATE|Строковые литералы ODBC сопоставляются с типом данных **datetime**. Любая операция присваивания литералов ODBC DATETIME типам данных **datetime2** вызывает неявное преобразование между данным типом и типом **datetime** согласно правилам преобразования.|  
|ODBC TIME|См. предыдущее правило ODBC DATE.|  
|ODBC DATETIME|См. предыдущее правило ODBC DATE.|  
|только DATE|Компонент TIME по умолчанию имеет значение 00:00:00.|  
|только TIME|Компонент DATE по умолчанию имеет значение 1900-1-1.|  
|только TIMEZONE|Указаны значения по умолчанию.|  
|DATE + TIME|Простейший.|  
|DATE + TIMEZONE|Не допускается.|  
|TIME + TIMEZONE|Компонент DATE по умолчанию имеет значение 1900-1-1. Входное значение TIMEZONE не учитывается.|  
|DATE + TIME + TIMEZONE|Используется локальный компонент DATETIME.|  
  
## <a name="examples"></a>Примеры  
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
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|Тип данных|Выходные данные|  
|---|---|
|**time**|12:35:29.1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29.1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  

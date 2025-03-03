---
title: Изменения массового копирования для расширенных типов даты и времени (OLE DB) | Документы Майкрософт
description: Узнайте о новых возможностях для работы с датами и временем для поддержки операций массового копирования в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, bulk copy operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4ceeea79de149408cb57a92c340de97ca22125f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748804"
---
# <a name="bulk-copy-changes-for-enhanced-date-and-time-types-ole-db"></a>Изменения массового копирования для расширенных типов даты и времени (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В статье описываются новые возможности даты-времени для поддержки операций массового копирования в OLE DB Driver for SQL Server.  
  
## <a name="format-files"></a>Файлы форматирования  
 При интерактивном построении файлов форматирования следующая таблица описывает вводные данные для задания типов даты-времени и соответствующих имен типов данных из файлов размещения.  
  
|Тип файла хранилища|Тип данных файла|Ответ на запрос: "Введите тип хранения файлов для поля <field_name> [\<default>]:"|  
|-----------------------|-------------------------|-----------------------------------------------------------------------------------------------------|  
|Datetime|SQLDATETIME|d|  
|Smalldatetime|SQLDATETIM4|D|  
|Дата|SQLDATE|de|  
|Time|SQLTIME|te|  
|Datetime2|SQLDATETIME2|d2|  
|Datetimeoffset|SQLDATETIMEOFFSET|do|  
  
 XSD для XML-файла форматирования будет содержать следующие дополнительные данные:  
  
```  
<xs:complexType name="SQLDATETIME2">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATETIMEOFFSET">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATE">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLTIME">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
```  
  
## <a name="character-data-files"></a>Файлы символьных данных  
 В файлах символьных данных значения даты и времени представлены в соответствии с документом "Форматы данных: строки и литералы" статьи [Data Type Support for OLE DB Date and Time Improvements](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md) (Улучшения поддержки типов данных даты и времени OLE DB).  
  
 В файлах данных в собственном формате значения даты и времени четырех новых типов представлены в виде потока табличных данных с масштабом 7 (поскольку это максимальное значение, поддерживаемое в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], а файлы данных bcp не хранят масштаб для этих столбцов). В хранении существующих типов **datetime** и **smalldatetime**, а также их представлений в виде потоков табличных данных (TDS) изменений нет.  
  
 Размеры типов OLE DB для различных типов хранения следующие:  
  
|Тип файла хранилища|Объем памяти в байтах|  
|-----------------------|---------------------------|  
|DATETIME|8|  
|smalldatetime|4|  
|Дата|3|  
|time|6|  
|datetime2|9|  
|datetimeoffset|11|  
 
  
## <a name="bcp-types-in-msoledbsqlh"></a>Типы BCP в msoledbsql.h  
 В msoledbsql.h определены следующие типы. Эти типы передаются с помощью параметра *eUserDataType* IBCPSession::BCPColFmt в OLE DB.  
  
|Тип файла хранилища|Тип данных файла|Введите msoledbsql.h для использования с IBCPSession::BCPColFmt|Значение|  
|-----------------------|-------------------------|-----------------------------------------------------------|-----------|  
|Datetime|SQLDATETIME|BCP_TYPE_SQLDATETIME|0x3d|  
|Smalldatetime|SQLDATETIM4|BCP_TYPE_SQLDATETIM4|0x3a|  
|Дата|SQLDATE|BCP_TYPE_SQLDATE|0x28|  
|Time|SQLTIME|BCP_TYPE_SQLTIME|0x29|  
|Datetime2|SQLDATETIME2|BCP_TYPE_SQLDATETIME2|0x2a|  
|Datetimeoffset|SQLDATETIMEOFFSET|BCP_TYPE_SQLDATETIMEOFFSET|0x2b|  
  
## <a name="bcp-data-type-conversions"></a>Поддерживаемые преобразования типов данных BCP  
 Сведения о преобразованиях приведены в следующих таблицах.  
  
 **Примечание для OLE DB**. Следующие преобразования выполняются через интерфейс IBCPSession. IRowsetFastLoad использует преобразования OLE DB, как определено в статье [Conversions Performed from Client to Server](../../oledb/ole-db-date-time/conversions-performed-from-client-to-server.md) (Преобразования, выполняемые при передаче от клиента к серверу). Следует заметить, что значения даты-времени округляются до 1/300 секунды, а в значениях типа smalldatetime после выполнения клиентских преобразований, описанных ниже, значение секунд становится равным нулю. Округление даты-времени распространяется на часы и минуты, но не на дату.  
  
|Кому --><br /><br /> От|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|char|wchar|  
|------------------------|----------|----------|-------------------|--------------|---------------|--------------------|----------|-----------|  
|Дата|1|-|1, 6|1, 6|1, 6|1, 5, 6|1, 3|1, 3|  
|Time|Недоступно|1, 10|1, 7, 10|1, 7, 10|1, 7, 10|1, 5, 7, 10|1, 3|1, 3|  
|Smalldatetime|1, 2|1, 4, 10|1|1|1, 10|1, 5, 10|1, 11|1, 11|  
|Datetime|1, 2|1, 4, 10|1, 12|1|1, 10|1, 5, 10|1, 11|1, 11|  
|Datetime2|1, 2|1, 4, 10|1, 12|1, 10|1, 10|1, 5, 10|1, 3|1, 3|  
|Datetimeoffset|1, 2, 8|1, 4, 8, 10|1, 8, 10|1, 8, 10|1, 8, 10|1, 10|1, 3|1, 3|  
|Char/wchar (date)|9|-|9, 6, 12|9, 6, 12|9, 6|9, 5, 6|Недоступно|Недоступно|  
|Char/wchar (time)|-|9, 10|9, 7, 10, 12|9, 7, 10, 12|9, 7, 10|9, 5, 7, 10|Недоступно|Недоступно|  
|Char/wchar (datetime)|9, 2|9, 4, 10|9, 10, 12|9, 10, 12|9, 10|9, 5, 10|Недоступно|Недоступно|  
|Char/wchar (datetimeoffset)|9, 2, 8|9, 4, 8, 10|9, 8, 10, 12|9, 8, 10, 12|9, 8, 10|9, 10|Недоступно|Недоступно|  
  
#### <a name="key-to-symbols"></a>Расшифровка символов  
  
|Символ|Значение|  
|------------|-------------|  
|-|Преобразование не поддерживается.<br />|  
|1|Если предоставленные данные недействительны, происходит ошибка. Для значений типа datetimeoffset после преобразования в формате UTC временная часть должна находиться в пределах диапазона, даже если преобразование в формате UTC не требуется. Это требование вызвано тем, что поток табличных данных и сервер всегда нормализуют время в значениях типа datetimeoffset для времени в формате UTC. Поэтому клиент должен проверить, попадают ли в поддерживаемый диапазон компоненты времени после преобразования в UTC.|  
|2|Компонент времени не учитывается.|  
|3|Если происходит усечение с потерей данных, выводится сообщение об ошибке. Для типа datetime2 число разрядов для дробной секунды (масштаб) определяется размером целевого столбца согласно следующей таблице. Для размеров столбцов, превышающих диапазон таблицы, подразумевается масштаб 9. Это преобразование позволяет передавать доли секунд с точностью до девяти значащих цифр — максимум, поддерживаемый OLE DB.<br /><br /> **Тип:** DBTIME2<br /><br /> **Подразумеваемый масштаб 0** 8<br /><br /> **Подразумеваемый масштаб 1..9** 1..9<br /><br /> <br /><br /> **Тип:** DBTIMESTAMP<br /><br /> **Подразумеваемый масштаб 0:** 19<br /><br /> **Подразумеваемый масштаб 1..9:** 21..29<br /><br /> <br /><br /> **Тип:** DBTIMESTAMPOFFSET<br /><br /> **Подразумеваемый масштаб 0:** 26<br /><br /> **Подразумеваемый масштаб 1..9:** 28..36|  
|4|Компонент даты не учитывается.|  
|5|Часовой пояс устанавливается в формате UTC (например, 00:00).|  
|6|Время установлено в нуль.|  
|7|Для даты задается значение 1900-01-01.|  
|8|Сдвиг часового пояса не учитывается.|  
|9|Строка проходит синтаксический анализ и преобразуется в значение типа date, datetime, datetimeoffset или time в зависимости от первого встреченного знака препинания и наличия остальных компонентов. Затем строка преобразуется в целевой тип согласно правилам, описанным в таблице, приведенной в конце статьи, для типа исходных данных, который выясняется в процессе анализа. Если при синтаксическом анализе данных неизбежно возникает ошибка, любой из компонентов вышел за пределы допустимого диапазона или не существует преобразования из литерального типа в целевой тип, возникает ошибка. Для параметров datetime и smalldatetime публикуется ошибка, если год находится за пределами диапазона, поддерживаемого этими типами.<br /><br /> Значение datetimeoffset после преобразования во времени в формате UTC должно находиться в пределах диапазона, даже если преобразование во времени в формате UTC не требуется. Причина этого заключается в том, что поток табличных данных и сервер всегда нормализуют время в значениях datetimeoffset для времени в формате UTC, поэтому клиент должен проверять, что значение времени после преобразования во времени в формате UTC находится в пределах поддерживаемого диапазона. Если значение не находится в поддерживаемом диапазоне UTC, возникает ошибка.|  
|10|Для преобразований, выполняемых при передаче от клиента серверу, сообщение об ошибке выдается, если происходит усечение с потерей данных. Эта ошибка также возникает в том случае, если значение выходит за пределы диапазона, который может быть представлен диапазоном времени в формате UTC, используемым сервером. Если в преобразовании с сервера на клиент происходит усечение секунд или долей секунд, выдается только предупреждение.|  
|11|Для преобразований, выполняемых при передаче от клиента серверу, сообщение об ошибке выдается, если происходит усечение с потерей данных.|
|12|Для секунд устанавливается значение 0, а доли секунды отбрасываются. Ошибка усечения невозможна.|  
|Недоступно|Существующий способ работы [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] и более ранних версий сохранен.|  
  
## <a name="see-also"></a>См. также:     
 [Улучшения функций даты и времени &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  

---
title: Типы данных PHP по умолчанию
description: В этом разделе перечислены все типы данных PHP по умолчанию с соответствующими типами данных SQL Server, которые применяются при использовании драйвера Microsoft SQLSRV для PHP для SQL Server
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- default data types
- converting data types
ms.assetid: b66c301d-3d20-45b8-a112-225d8f01c0bd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c1e1cf91baf80fd6298eaaca9c9e12a0b5858d9f
ms.sourcegitcommit: d1051f05a7db81ec62d9785bb6af572408f3d4e0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88680796"
---
# <a name="default-php-data-types"></a>типы данных PHP по умолчанию;
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

При извлечении данных с сервера [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] преобразует данные в тип данных PHP по умолчанию, если пользователь не указал тип данных PHP.  
  
При возврате данных с помощью драйвера PDO_SQLSRV типом данных является int или string.  
  
В оставшейся части этой статьи рассматриваются типы данных по умолчанию при работе с драйвером SQLSRV.  
  
Следующая таблица содержит тип данных SQL Server (тип данных, извлекаемых с сервера), тип данных PHP по умолчанию (тип данных, в который преобразуются данные) и кодировку по умолчанию для потоков и строк. Дополнительные сведения об указании типов данных при извлечении данных с сервера см. в статье [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md).  
  
|Тип SQL Server|Тип PHP по умолчанию|Кодировка по умолчанию|  
|-------------------|--------------------|--------------------|  
|BIGINT|Строка|8-битовый символ<sup>1</sup>|  
|binary|Поток<sup>2</sup>|Двоичная<sup>3</sup>|  
|bit|Целое число|8-битовый символ<sup>1</sup>|  
|char|Строка|8-битовый символ<sup>1</sup>|  
|date<sup>4</sup>|Datetime|Неприменимо|  
|datetime<sup>4</sup>|Datetime|Неприменимо|  
|datetime2<sup>4</sup>|Datetime|Неприменимо|  
|datetimeoffset<sup>4</sup>|Datetime|Неприменимо|  
|Decimal|Строка|8-битовый символ<sup>1</sup>|  
|FLOAT|Float|8-битовый символ<sup>1</sup>|  
|geography|Поток|Двоичная<sup>3</sup>|  
|geometry|Поток|Двоичная<sup>3</sup>|  
|image<sup>5</sup>|Поток<sup>2</sup>|Двоичная<sup>3</sup>|  
|INT|Целое число|8-битовый символ<sup>1</sup>|  
|money|Строка|8-битовый символ<sup>1</sup>|  
|nchar|Строка|8-битовый символ<sup>1</sup>|  
|NUMERIC|Строка|8-битовый символ<sup>1</sup>|  
|nvarchar|Строка|8-битовый символ<sup>1</sup>|  
|nvarchar(MAX)|Поток<sup>2</sup>|8-битовый символ<sup>1</sup>|  
|ntext<sup>6</sup>|Поток<sup>2</sup>|8-битовый символ<sup>1</sup>|  
|real|Float|8-битовый символ<sup>1</sup>|  
|smalldatetime|Datetime|8-битовый символ<sup>1</sup>|  
|smallint|Целое число|8-битовый символ<sup>1</sup>|  
|smallmoney|Строка|8-битовый символ<sup>1</sup>|  
|sql_variant<sup>7</sup>|Строка|8-битовый символ<sup>1</sup>|  
|text<sup>8</sup>|Поток<sup>2</sup>|8-битовый символ<sup>1</sup>|  
|time<sup>4</sup>|Datetime|Неприменимо|  
|TIMESTAMP|Строка|8-битовый символ<sup>1</sup>|  
|tinyint|Целое число|8-битовый символ<sup>1</sup>|  
|(UDT)|Поток<sup>2</sup>|Двоичная<sup>3</sup>|  
|UNIQUEIDENTIFIER|String<sup>9</sup>|8-битовый символ<sup>1</sup>|  
|varbinary|Поток<sup>2</sup>|Двоичная<sup>3</sup>|  
|varbinary(MAX)|Поток<sup>2</sup>|Двоичная<sup>3</sup>|  
|varchar|Строка|8-битовый символ<sup>1</sup>|  
|varchar(MAX)|Поток<sup>2</sup>|8-битовый символ<sup>1</sup>|
|Xml|Поток<sup>2</sup>|8-битовый символ<sup>1</sup>|  
  

1.  Данные возвращаются в виде 8-битовых символов, как указано в кодовой странице языкового стандарта Windows, установленного в системе. Для всех многобайтовых символов или символов, не соответствующих этой кодовой странице, подставляется однобайтовый символ вопросительного знака (?).  
  
2.  Если для извлечения данных, имеющих тип потока PHP по умолчанию, используется [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md) или [sqlsrv_fetch_object](../../connect/php/sqlsrv-fetch-object.md), данные возвращаются в виде строки с кодировкой потока. Например, при извлечении данных двоичного типа SQL Server с использованием **sqlsrv_fetch_array** типом возвращаемого значения по умолчанию будет двоичная строка.  
  
3.  Данные возвращаются в виде потока необработанных байтов с сервера без применения кодировки или преобразования.  

4.  Данные типов даты и времени можно извлекать в виде строк. Дополнительные сведения см. в статье [Практическое руководство. Получение типа даты и времени в виде строк с помощью драйвера SQLSRV](../../connect/php/how-to-retrieve-date-and-time-type-as-strings-using-the-sqlsrv-driver.md).  

5.  Это устаревший тип, соответствующий типу varbinary(max).

6. Это устаревший тип, соответствующий типу nvarchar(max).

7.  sql_variant не поддерживается для двунаправленных или выходных параметров.

8.  Это устаревший тип, соответствующий типу varchar(max).  
  
9.  Идентификаторы UNIQUEIDENTIFIER — это идентификаторы GUID, представленные следующим регулярным выражением:  
  
    [0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-f]{4}-[0-9a-fA-f]{4}-[0-9a-fA-F]{12}  
 
 
## <a name="other-new-sql-server-2008-data-types-and-features"></a>Другие новые типы данных и функции SQL Server 2008  
Типы данных, впервые появившиеся в SQL Server 2008 и существующие за пределами столбцов (например, параметры с табличным значением), в [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] не поддерживаются. В следующей таблице приведены сводные данные о поддержке PHP в новых функциях SQL Server 2008.  
  
|Функция|Поддержка PHP|  
|-----------|---------------|  
|Возвращающий табличное значение параметр|нет|  
|Разреженные столбцы|Partial|  
|Сжатие NULL-битов|Да|  
|Определяемые пользователем типы данных больших значений CLR (UDT)|Да|  
|Имя субъекта-службы|нет|  
|MERGE|Да|  
|FILESTREAM|Partial|  
  
Частичная поддержка типа означает, что вы не можете программно запросить тип столбца.  
  
## <a name="see-also"></a>См. также:  
[Константы (драйверы Майкрософт для PHP для SQL Server)](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)

[Преобразование типов данных](../../connect/php/converting-data-types.md)

[Типы PHP](https://php.net/manual/en/language.types.php)

[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)

[sqlsrv_field_metadata](../../connect/php/sqlsrv-field-metadata.md)  
  

---
title: Сопоставление типов с помощью PolyBase | Документация Майкрософт
description: В этой таблице приводится сопоставление между внешними источниками данных PolyBase и SQL Server. Чтобы определить внешние таблицы, выполните команду Transact-SQL CREATE EXTERNAL TABLE.
ms.date: 09/24/2018
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
ms.openlocfilehash: 2cbdcfca40f1fa2fd51e669aeefcf317a958c687
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467435"
---
# <a name="type-mapping-with-polybase"></a>Сопоставление типов с помощью PolyBase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

В этой статье описывается сопоставление между внешними источниками данных PolyBase и SQL Server. Эти сведения можно использовать, чтобы правильно определить внешние таблицы с помощью команды Transact-SQL [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).

## <a name="overview"></a>Обзор

При создании внешней таблицы с помощью PolyBase определения столбцов, включая типы данных и количество столбцов, должны соответствовать данным во внешних файлах. В случае несоответствия при запросе данных строки файла отклоняются.  
  
Во внешних таблицах, которые ссылаются на файлы во внешних источниках данных, определения столбцов и типов должны точно соответствовать схеме внешнего файла. При определении типов данных, которые ссылаются на данные, хранящиеся в Hadoop или Hive, используйте следующие сопоставления типов данных SQL и Hive и приведите тип к типу данных SQL при выборе. Типы включают все версии Hive, если не указано иное.

> [!NOTE]  
> SQL Server не поддерживает тип данных Hive *infinity* в любых преобразованиях. PolyBase будет завершаться ошибкой преобразования типов данных.

## <a name="hadoop-type-mapping-reference"></a>Сопоставление типов Hadoop

| Тип данных SQL | Тип данных .NET            | Тип данных Hive | Тип данных Hadoop/Java | Комментарии                       |
| ------------- | ------------------------- | -------------- | --------------------- | ------------------------------ |
| tinyint       | Byte                      | tinyint        | ByteWritable          | Только для чисел без знака.     |
| smallint      | Int16                     | smallint       | ShortWritable         |
| INT           | Int32                     | INT            | IntWritable           |
| BIGINT        | Int64                     | BIGINT         | LongWritable          |
| bit           | Логическое                   | Логическое        | BooleanWritable       |
| FLOAT         | Double                    | double         | DoubleWritable        |
| real          | Один                    | FLOAT          | FloatWritable         |
| money         | Decimal                   | double         | DoubleWritable        |
| smallmoney    | Decimal                   | double         | DoubleWritable        |
| nchar         | Строка<br /><br /> Char[] | строка         | Varchar               |
| nvarchar      | Строка<br /><br /> Char[] | строка         | Varchar               |
| char          | Строка<br /><br /> Char[] | строка         | Varchar               |
| varchar       | Строка<br /><br /> Char[] | строка         | Varchar               |
| binary        | Byte[]                    | binary         | BytesWritable         | Применяется к Hive 0.8 и более поздней версии. |
| varbinary     | Byte[]                    | binary         | BytesWritable         | Применяется к Hive 0.8 и более поздней версии. |
| Дата          | Дата и время                  | TIMESTAMP      | TimestampWritable     |
| smalldatetime | Дата и время                  | TIMESTAMP      | TimestampWritable     |
| datetime2     | Дата и время                  | TIMESTAMP      | TimestampWritable     |
| DATETIME      | Дата и время                  | TIMESTAMP      | TimestampWritable     |
| time          | TimeSpan                  | TIMESTAMP      | TimestampWritable     |
| Decimal       | Decimal                   | Decimal        | BigDecimalWritable    | Применяется к Hive 0.11 и более поздней версии. |

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

## <a name="oracle-type-mapping-reference"></a>Сопоставление типов Oracle

| Тип данных Oracle | Тип SQL Server | 
| -------------    | --------------- |
|Float             |Float            |
|NUMBER            |Float            |
|NUMBER (p,s)      |Decimal (p, s)   |
|LONG              |nvarchar         |
|BINARY_FLOAT      |Real             | 
|BINARY_DOUBLE     |Float            | 
|CHAR              |Char             |
|VARCHAR2          |Varchar          | 
|NVARCHAR2         |nvarchar         | 
|RAW               |Varbinary        |
|LONG RAW          |Varbinary        | 
|BLOB              |Varbinary        |
|CLOB              |Varchar          |
|NCLOB             | nvarchar        | 
|ROWID             |Varchar          |
|UROWID            |Varchar          | 
|DATE              |Datetime2        |
|timestamp         |Datetime2        | 

**Несоответствие типов** 

**Число с плавающей запятой**: Oracle поддерживает числа с плавающей запятой с точностью до 126. SQL Server поддерживает лучшую точность — 53. Таким образом **Float (1–53)** можно сопоставить напрямую, но при этом существует потеря данных из-за усечения.

**Метка времени:** Timestamp и Timestamp with local timezone в Oracle поддерживают точность секунды до 9 десятичных цифр после запятой, тогда как DateTime2 в SQL Server поддерживает точность только до 7. 




## <a name="mongodb-type-mapping"></a>Сопоставление типов MongoDB

| Тип данных BSON     | Тип SQL Server |
| ------------------ | --------------- |
| Double             | Float           |
| Строка             | nvarchar        |
| Двоичные данные        | nvarchar        |
| Идентификатор объекта.          | nvarchar        |
| Логическое значение            | bit             |
| Дата               | Datetime2       |
| 32-разрядное целое число     | Int             |
| Отметка времени          | nvarchar        |
| 64-разрядное целое число     | BigInt          |
|Decimal 128         | Decimal         | 
| DBPointer          | nvarchar        |
| JavaScript         | nvarchar        |
| Максимальный ключ            | nvarchar        |
| Минимальный ключ            | nvarchar        |
| Символ             | nvarchar        |
| Регулярное выражение | nvarchar        |
| Не определено/NULL     | nvarchar        |


MongoDB использует документы BSON для хранения записей данных. В отличие от предыдущих сценариев, в BSON нет схемы и поддерживается внедрение документов и массивов в другие документы. Это обеспечивает гибкость для пользователя. 


## <a name="teradata-type-mapping-reference"></a>Сопоставление типов Teradata

| Тип данных Teradata | Тип SQL Server | 
| -------------      | -------------   |
|ЦЕЛОЕ ЧИСЛО             |Int              |
|SMALLINT            |SmallInt         |
|bigint              |BigInt           |
|BYTEINT             |SmallInt         |
|DECIMAL             |Decimal          |
|FLOAT               |Decimal          |
|BYTE                |Двоичные данные           |
|VARBYTE             |Varbinary        |
|BLOB                |varbinary        |
|CHAR                |Nchar            |
|CLOB                |nvarchar         |
|VARCHAR             |nvarchar         |
|GRAPHIC             |Nchar            |
|JSON                |nvarchar         |
|VARGRAPHIC          |nvarchar         |
|DATE                |Дата             |
|timestamp           |Datetime2        |
|TIME                |Time             |
|TIME WITH TIME ZONE |Time             |
|TIMESTAMP WITH TIME ZONE|Время         |

::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об использовании сопоставления см. в справочнике по Transact-SQL для [CREATE EXTERNAL TABLE (Transact-SQL)](../../t-sql/statements/create-external-table-transact-sql.md).

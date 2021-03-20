---
title: Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных
description: В SQL Server файлы форматирования в формате XML и отличном от него могут использовать файл данных, поля которого расположены в ином порядке, нежели столбцы таблицы.
ms.date: 09/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- mapping columns to fields during import [SQL Server]
- format files [SQL Server], mapping columns to fields
ms.assetid: e7ee4f7e-24c4-4eb7-84d2-41e57ccc1ef1
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 6b2a501eafa04901aa72e6adedbdcd99854377db
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745784"
---
# <a name="use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server"></a>Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Файл данных может содержать поля, выстроенные в порядке, отличном от порядка соответствующих столбцов в таблице. В этом подразделе описывается изменение файлов форматирования в формате XML и в формате, отличном от XML для импорта файла данных, поля которого выстроены в ином порядке, нежели столбцы таблицы. Измененный файл форматирования сопоставляет поля данных соответствующим столбцам таблицы.  Ознакомьтесь с разделом [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) для получения дополнительных сведений.

|Контур|
|---|
|[Пример условий теста](#etc)<br />&emsp;&#9679;&emsp;[Образец таблицы](#sample_table)<br />&emsp;&#9679;&emsp;[Образец файла данных](#sample_data_file)<br />[Создание файлов форматирования](#create_format_file)<br />&emsp;&#9679;&emsp;[Создание файла форматирования в формате, отличном от XML](#nonxml_format_file)<br />&emsp;&#9679;&emsp;[Изменение файла форматирования в формате, отличном от XML](#modify_nonxml_format_file)<br />&emsp;&#9679;&emsp;[Создание XML-файла форматирования](#xml_format_file)<br />&emsp;&#9679;&emsp;[Изменение XML-файла форматирования](#modify_xml_format_file)<br />[Импорт данных с использованием файла форматирования для сопоставления столбцов таблицы полям файла данных](#import_data)<br />&emsp;&#9679;&emsp;[Использование bcp и файла форматирования в формате, отличном от XML](#bcp_nonxml)<br />&emsp;&#9679;&emsp;[Использование bcp и XML-файла форматирования](#bcp_xml)<br />&emsp;&#9679;&emsp;[Использование BULK INSERT и файла форматирования в формате, отличном от XML](#bulk_nonxml)<br />&emsp;&#9679;&emsp;[Использование BULK INSERT и XML-файла форматирования](#bulk_xml)<br />&emsp;&#9679;&emsp;[Использование OPENROWSET(BULK…) и файла форматирования в формате, отличном от XML](#openrowset_nonxml)<br />&emsp;&#9679;&emsp;[Использование OPENROWSET(BULK…) и XML-файла форматирования](#openrowset_xml)|

> [!NOTE]  
>  И файлы форматирования, отличные от XML, и XML-файлы форматирования можно использовать для массового импорта файла данных в таблицу с помощью команды служебной программы [bcp](../../tools/bcp-utility.md), а также инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) или INSERT... SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Дополнительные сведения см. в разделе [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).  

## <a name="example-test-conditions"></a>Пример условий теста<a name="etc"></a>  
Примеры измененных файлов форматирования базируются на таблице и файле данных, которые определены ниже.

### <a name="sample-table"></a>Образец таблицы<a name="sample_table"></a>
Приведенный ниже сценарий создает тестовую базу данных и таблицу с именем `myRemap`.  Выполните следующий код Transact-SQL в Microsoft SQL Server Management Studio (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE myRemap
   (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   Gender char(1)
   );
```

### <a name="sample-data-file"></a>Образец файла данных<a name="sample_data_file"></a>
В представленных ниже данных `FirstName` и `LastName` приведены в обратном порядке, как показано в таблице `myRemap`.  С помощью Блокнота создайте пустой файл `D:\BCP\myRemap.bcp` и вставьте следующие данные:
```
1,Grosse,Anthony,M
2,Fatnowna,Alica,F
3,Rosenhain,Stella,F
```

## <a name="creating-the-format-files"></a>Создание файлов форматирования<a name="create_format_file"></a>
Для массового импорта из файла `myRemap.bcp` в таблицу `myRemap` файл форматирования должен сделать следующий код:
* сопоставить первое поле данных с первым столбцом, `PersonID`;
* сопоставить второе поле данных с третьим столбцом, `LastName`;
* сопоставить третье поле данных со вторым столбцом, `FirstName`;
* сопоставить четвертое поле данных с четвертым столбцом, `Gender`.

Простейший способ создать файл форматирования заключается в использовании [служебной программы bcp](../../tools/bcp-utility.md).  Во-первых, создайте базовый файл форматирования из существующей таблицы.  Во-вторых, измените его в соответствии с фактическим файлом данных.

### <a name="creating-a-non-xml-format-file"></a>Создание файла форматирования в формате, отличном от XML<a name="nonxml_format_file"></a>
Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) . Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myRemap.fmt`в формате, отличном от XML, на основе схемы `myRemap`.  Кроме того, квалификатор `c` используется для указания символьных данных, `t,` используется для указания запятую в качестве признака конца поля, а `T` используется для указания доверенного соединения с использованием встроенной системы безопасности.  В командной строке введите следующую команду:
```
bcp TestDatabase.dbo.myRemap format nul -c -f D:\BCP\myRemap.fmt -t, -T
```
### <a name="modifying-the-non-xml-format-file"></a>Изменение файла форматирования в формате, отличном от XML <a name="modify_nonxml_format_file"></a>
Терминологию см. в разделе [Структура файлов форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md#Structure) .  Откройте `D:\BCP\myRemap.fmt` в Блокноте и внесите следующие изменения:
1.  Переупорядочите строки файла форматирования, чтобы они отображались в том же порядке, что и данные в `myRemap.bcp`.
2.  Убедитесь, что значения порядка полей в файле узлов идут последовательно.
3.  Убедитесь, что после последней строки файла форматирования расположен символ возврата каретки.

Сравните изменения:     
**До:**
```
13.0
4
1       SQLCHAR    0       7       ","      1     PersonID               ""
2       SQLCHAR    0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR    0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR    0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
**После:**
```
13.0
4
1       SQLCHAR    0       7       ","      1     PersonID               ""
2       SQLCHAR    0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR    0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR    0       1       "\r\n"   4     Gender                 SQL_Latin1_General_CP1_CI_AS

```
Теперь измененный файл форматирования отображает:
* Первое поле данных в `myRemap.bcp` сопоставляется с первым столбцом. `myRemap.. PersonID`
* Второе поле данных в `myRemap.bcp` сопоставляется с третьим столбцом. `myRemap.. LastName`
* Третье поле данных в `myRemap.bcp` сопоставляется со вторым столбцом. `myRemap.. FirstName`
* Четвертое поле данных в `myRemap.bcp` сопоставляется с четвертым столбцом. `myRemap.. Gender`

### <a name="creating-an-xml-format-file"></a>Создание XML-файла форматирования <a name="xml_format_file"></a>  
Дополнительные сведения см. в разделе [XML-файлы форматирования (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания XML-файла форматирования `myRemap.xml`на основе схемы `myRemap`.  Кроме того, квалификатор `c` используется для указания символьных данных, `t,` используется для указания запятую в качестве признака конца поля, а `T` используется для указания доверенного соединения с использованием встроенной системы безопасности.  Для создания XML-файла форматирования должен использоваться квалификатор `x` .  В командной строке введите следующую команду:
```
bcp TestDatabase.dbo.myRemap format nul -c -x -f D:\BCP\myRemap.xml -t, -T
```
### <a name="modifying-the-xml-format-file"></a>Изменение XML-файла форматирования <a name="modify_xml_format_file"></a>
Терминологию см. в разделе [Синтаксис схемы для XML-файлов форматирования](../../relational-databases/import-export/xml-format-files-sql-server.md#StructureOfXmlFFs) .  Откройте `D:\BCP\myRemap.xml` в Блокноте и внесите следующие изменения:
1. Порядок, в котором объявлены элементы \<FIELD> в файле форматирования, является порядком, в котором эти поля будут расположены в файле данных, поэтому обратите порядок элементов \<FIELD> с атрибутами ID, равными 2 и 3.
2. Убедитесь, что значения атрибутов ID \<FIELD> идут последовательно.
3. Порядок элементов \<COLUMN> в элементе \<ROW> задает порядок, в котором они будут возвращены в целевой объект массовой операцией.  XML-файл форматирования назначает каждому элементу \<COLUMN> локальное имя, не имеющее отношения к столбцу целевой таблицы операции массового импорта.  Порядок элементов \<COLUMN> не зависит от порядка элементов \<FIELD> в определении \<RECORD>.  Каждый элемент \<COLUMN> соответствует элементу \<FIELD> (чей идентификатор задан в атрибуте SOURCE элемента \<COLUMN>).  Таким образом, единственными атрибутами, требующими пересмотра, являются значения \<COLUMN> SOURCE.  Обратите порядок атрибутов 2 и 3 \<COLUMN> SOURCE.

Сравните изменения:  
**До:**
```
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
**После:**
```
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="1" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="3" NAME="FirstName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="2" NAME="LastName" xsi:type="SQLVARYCHAR"/>
  <COLUMN SOURCE="4" NAME="Gender" xsi:type="SQLCHAR"/>
 </ROW>
</BCPFORMAT>
```
Теперь измененный файл форматирования отображает:
* Поле 1, соответствующее столбцу 1, пересопоставляется с первым столбцом таблицы. `myRemap.. PersonID`
* Поле 2, соответствующее столбцу 2, пересопоставляется с третьим столбцом таблицы. `myRemap.. LastName`
* Поле 3, соответствующее столбцу 3, пересопоставляется со вторым столбцом таблицы. `myRemap.. FirstName`
* Поле 4, соответствующее столбцу 4, пересопоставляется с четвертым столбцом таблицы. `myRemap.. Gender`


## <a name="importing-data-with-a-format-file-to-map-table-columns-to-data-file-field"></a>Импорт данных с использованием файла форматирования для сопоставления столбцов таблицы полям файла данных<a name="import_data"></a>
В приведенных ниже примерах используется база данных, файл данных и файлы форматирования, созданные ранее.

### <a name="using-bcp-and-non-xml-format-file"></a>Использование [bcp](../../tools/bcp-utility.md) и [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bcp_nonxml"></a>
В командной строке введите следующую команду:
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.fmt -T
```

### <a name="using-bcp-and-xml-format-file"></a>Использование [bcp](../../tools/bcp-utility.md) и [XML-файла форматирования](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bcp_xml"></a>
В командной строке введите следующую команду:
```
bcp TestDatabase.dbo.myRemap IN D:\BCP\myRemap.bcp -f D:\BCP\myRemap.xml -T
```

### <a name="using-bulk-insert-and-non-xml-format-file"></a>Использование [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="bulk_nonxml"></a>
Выполните следующий код Transact-SQL в Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>Использование [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и [XML-файла форматирования](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="bulk_xml"></a>
Выполните следующий код Transact-SQL в Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
BULK INSERT dbo.myRemap   
   FROM 'D:\BCP\myRemap.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myRemap.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>Использование [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)<a name="openrowset_nonxml"></a>    
Выполните следующий код Transact-SQL в Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>Использование [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и [XML-файла форматирования](../../relational-databases/import-export/xml-format-files-sql-server.md)<a name="openrowset_xml"></a>
Выполните следующий код Transact-SQL в Microsoft SQL Server Management Studio (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myRemap;
INSERT INTO dbo.myRemap 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myRemap.bcp',
        FORMATFILE = 'D:\BCP\myRemap.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myRemap;
```


  
## <a name="see-also"></a>См. также:  
[bcp Utility](../../tools/bcp-utility.md)   
 [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
  

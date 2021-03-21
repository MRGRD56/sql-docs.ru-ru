---
title: Использование файла форматирования для массового импорта данных
description: В SQL Server можно использовать файл форматирования в операциях массового импорта. Файл форматирования сопоставляет поля в файле данных столбцам таблицы.
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], format files
- format files [SQL Server], importing data using
ms.assetid: 2956df78-833f-45fa-8a10-41d6522562b9
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 124b87eca694ef809eff7feffe37e6ae389cdd2e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745664"
---
# <a name="use-a-format-file-to-bulk-import-data-sql-server"></a>Использование файла форматирования для массового импорта данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта тема посвящена использованию файла форматирования в операциях массового импорта.  Файл форматирования сопоставляет поля в файле данных столбцам таблицы.  Ознакомьтесь с разделом [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) для получения дополнительных сведений.
  
## <a name="before-you-begin"></a>Перед началом
* Чтобы файл форматирования мог работать с файлом символьных данных в Юникоде, все поля ввода должны представлять собой текстовые строки Юникода (то есть строки Юникода фиксированной длины или заканчивающиеся символом конца строки).
* Чтобы массово экспортировать или импортировать [SQLXML](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md) -данные, используйте один из следующих типов данных в файле форматирования:
  * SQLCHAR или SQLVARCHAR (данные отправляются в кодовой странице клиента или кодовой странице, определенной параметрами сортировки);
  * SQLNCHAR или SQLNVARCHAR (данные отправляются в Юникоде);
  * SQLBINARY или SQLVARBIN (данные отправляются без преобразования).
* База данных SQL Azure и Azure Synapse Analytics поддерживают только [bcp](../../tools/bcp-utility.md).  Дополнительные сведения см. в следующих источниках:
  * [Загрузка данных в Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [Загрузка данных из SQL Server в Azure Synapse Analytics (неструктурированные файлы)](/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading)
  * [Перенос данных](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-develop)

## <a name="example-test-conditions"></a>Пример условий теста
Примеры файлов форматирования в этой статье основаны на таблице и файле данных, которые определены ниже.

### <a name="sample-table"></a>Пример таблицы
Приведенный ниже сценарий создает тестовую базу данных и таблицу с именем `myFirstImport`.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.MyFirstImport (
   PersonID smallint,
   FirstName varchar(25),
   LastName varchar(30),
   BirthDate Date
   );
```

### <a name="sample-data-file"></a>Пример файла данных
С помощью Блокнота создайте пустой файл `D:\BCP\myFirstImport.bcp` и вставьте следующие данные:
```
1,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
3,Stella,Rosenhain,1992-03-02
```

Кроме того, можно выполнить следующий сценарий PowerShell для создания и заполнения файла данных:
```powershell
Clear-Host
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = Join-Path -Path $dir -ChildPath 'MyFirstImport.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Notepad.exe $bcpfile;
```

## <a name="creating-the-format-files"></a>Создание файлов форматирования
SQL Server поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования.  Файл форматирования не в формате XML поддерживается более ранними версиями SQL Server.

### <a name="creating-a-non-xml-format-file"></a>Создание файла форматирования в формате, отличном от XML
Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myFirstImport.fmt`в формате, отличном от XML, на основе схемы `myFirstImport`.  Чтобы создать файл форматирования при помощи служебной программы bcp, укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format также требует наличия параметра **-f** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, **t,** используется для указания запятой в качестве признака [конца поля](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), а **T** используется для указания доверенного подключения с использованием встроенной системы безопасности.  В командной строке введите следующую команду:

```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -f D:\BCP\myFirstImport.fmt -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.fmt
```

Файл форматирования не в формате XML, `D:\BCP\myFirstImport.fmt` , должен выглядеть следующим образом:
```
13.0
4
1       SQLCHAR             0       7       ","      1     PersonID               ""
2       SQLCHAR             0       25      ","      2     FirstName              SQL_Latin1_General_CP1_CI_AS
3       SQLCHAR             0       30      ","      3     LastName               SQL_Latin1_General_CP1_CI_AS
4       SQLCHAR             0       11      "\r\n"   4     BirthDate              ""
```

> [!IMPORTANT]
> Убедитесь, что файл форматирования не в формате XML заканчивается символом перевода строки или возврата каретки.  В противном случае, скорее всего, появится следующее сообщение об ошибке:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

### <a name="creating-an-xml-format-file"></a>Создание XML-файла форматирования
Дополнительные сведения см. в разделе [XML-файлы форматирования (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания XML-файла форматирования `myFirstImport.xml`на основе схемы `myFirstImport`. Чтобы создать файл форматирования при помощи служебной программы bcp, укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format всегда требует наличия параметра **-f** , а чтобы создать XML-файл форматирования, необходимо также задать параметр **-x** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, **t,** используется для указания запятой в качестве признака [конца поля](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), а **T** используется для указания доверенного подключения с использованием встроенной системы безопасности.  В командной строке введите следующую команду:
```cmd
bcp TestDatabase.dbo.myFirstImport format nul -c -x -f D:\BCP\myFirstImport.xml -t, -T

REM Review file
Notepad D:\BCP\myFirstImport.xml
```

Файл форматирования в формате XML, `D:\BCP\myFirstImport.xml` , должен выглядеть следующим образом:
```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="http://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
 <RECORD>
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="25" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="11"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="PersonID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="FirstName" xsi:type="SQLVARCHAR"/>
  <COLUMN SOURCE="3" NAME="LastName" xsi:type="SQLVARCHAR"/>
  <COLUMN SOURCE="4" NAME="BirthDate" xsi:type="SQLDATE"/>
 </ROW>
</BCPFORMAT>
```

## <a name="using-a-format-file-to-bulk-import-data"></a>Использование файла форматирования для массового импорта данных
В приведенных ниже примерах используется база данных, файл данных и файлы форматирования, созданные ранее.

### <a name="using-bcp-and-non-xml-format-file"></a>Использование [bcp](../../tools/bcp-utility.md) и [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
В командной строке введите следующую команду:
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport"
```


### <a name="using-bcp-and-xml-format-file"></a>Использование [bcp](../../tools/bcp-utility.md) и [XML-файла форматирования](../../relational-databases/import-export/xml-format-files-sql-server.md)
В командной строке введите следующую команду:
```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.MyFirstImport;"

REM Import data
bcp TestDatabase.dbo.myFirstImport IN D:\BCP\myFirstImport.bcp -f D:\BCP\myFirstImport.xml -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.MyFirstImport;"
```


### <a name="using-bulk-insert-and-non-xml-format-file"></a>Использование [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.fmt');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-bulk-insert-and-xml-format-file"></a>Использование [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и [XML-файла форматирования](../../relational-databases/import-export/xml-format-files-sql-server.md)
Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
BULK INSERT dbo.myFirstImport   
   FROM 'D:\BCP\myFirstImport.bcp'   
   WITH (FORMATFILE = 'D:\BCP\myFirstImport.xml');  
GO  

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-non-xml-format-file"></a>Использование [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)    
Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.fmt'
        ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```

### <a name="using-openrowsetbulk-and-xml-format-file"></a>Использование [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и [XML-файла форматирования](../../relational-databases/import-export/xml-format-files-sql-server.md)
Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;  
GO

TRUNCATE TABLE myFirstImport; -- (for testing)
INSERT INTO dbo.myFirstImport 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myFirstImport.bcp',
        FORMATFILE = 'D:\BCP\myFirstImport.xml'  
       ) AS t1;
GO

-- review results
SELECT * FROM TestDatabase.dbo.myFirstImport;
```
  
## <a name="more-examples"></a>Другие примеры
 [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
 [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
 [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
 [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>См. также раздел  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [XML-файлы форматирования (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)

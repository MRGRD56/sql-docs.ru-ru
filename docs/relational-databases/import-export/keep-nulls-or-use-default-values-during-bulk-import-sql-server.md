---
title: Сохранение значений NULL или значений по умолчанию при массовом импорте данных
description: При массовом импорте в SQL Server как bcp, так и BULK INSERT загружают значения по умолчанию для замены значений NULL. Для обоих вариантов можно выбрать сохранение значений NULL.
ms.date: 09/20/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], null values
- bulk importing [SQL Server], default values
- data formats [SQL Server], null values
- bulk rowset providers [SQL Server]
- bcp utility [SQL Server], null values
- BULK INSERT statement
- default values
- OPENROWSET function, bulk importing
- data formats [SQL Server], default values
ms.assetid: 6b91d762-337b-4345-a159-88abb3e64a81
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 724b8c0df07cac555158661db6d00ccf98e7134c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752544"
---
# <a name="keep-nulls-or-default-values-during-bulk-import-sql-server"></a>Сохранение значений NULL или значений по умолчанию при массовом импорте данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

При импорте данных в таблицу команда [bcp](../../tools/bcp-utility.md) и инструкция [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) используют значения по умолчанию, которые определены для столбцов таблицы.  Например, если поле в файле данных имеет значение NULL, вместо него загружается значение по умолчанию соответствующего столбца.  И команда [bcp](../../tools/bcp-utility.md) , и инструкция [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) позволяют пользователю указать, следует ли оставлять значения NULL.

Обычная инструкция INSERT, напротив, оставляет значение NULL и не использует значения по умолчанию. Инструкция INSERT ... Инструкция SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) ведет себя так же, как обычная инструкция INSERT, но поддерживает [табличное указание](../../t-sql/queries/hints-transact-sql-table.md) для загрузки значений по умолчанию.

|Контур|
|---|
|[Сохранение значений NULL](#keep_nulls)<br />[Использование значений по умолчанию при выполнении инструкции INSERT ... SELECT * FROM OPENROWSET(BULK...)](#keep_default)<br />[Пример условий теста](#etc)<br />&emsp;&#9679;&emsp;[Образец таблицы](#sample_table)<br />&emsp;&#9679;&emsp;[Образец файла данных](#sample_data_file)<br />&emsp;&#9679;&emsp;[Образец файла форматирования в формате, отличном от XML](#nonxml_format_file)<br />[Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных](#import_data)<br />&emsp;&#9679;&emsp;[Использование команды bcp и сохранение значений NULL без файла форматирования](#bcp_null)<br />&emsp;&#9679;&emsp;[Использование команды bcp и сохранение значений NULL с помощью файла форматирования в формате, отличном от XML](#bcp_null_fmt)<br />&emsp;&#9679;&emsp;[Использование команды bcp и значений по умолчанию без файла форматирования](#bcp_default)<br />&emsp;&#9679;&emsp;[Использование команды bcp и значений по умолчанию с помощью файла форматирования в формате, отличном от XML](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и сохранение значений NULL без файла форматирования](#bulk_null)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и сохранение значений NULL с помощью файла форматирования в формате, отличном от XML](#bulk_null_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и значений по умолчанию без файла форматирования](#bulk_default)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и значений по умолчанию с помощью файла форматирования в формате, отличном от XML](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции OPENROWSET(BULK...) и сохранение значений NULL с помощью файла форматирования в формате, отличном от XML](#openrowset__null_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции OPENROWSET(BULK...) и значений по умолчанию с помощью файла форматирования в формате, отличном от XML](#openrowset__default_fmt)

## <a name="keeping-null-values"></a>Сохранение значений NULL<a name="keep_nulls"></a>  
Следующие квалификаторы указывают, что вместо пустого поля в файле данных необходимо вставить не значение по умолчанию, а значение NULL.  Для инструкции [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md)по умолчанию любым столбцам, не участвующим в операции массовой загрузки, присваивается значение NULL.
  
|Get-Help|Квалификатор|Тип квалификатора|  
|-------------|---------------|--------------------|  
|bcp|-k|Параметр|  
|BULK INSERT|KEEPNULLS\*|Аргумент|  
|Инструкции INSERT ... SELECT * FROM OPENROWSET(BULK...).|Недоступно|Недоступно|  
  
\* Для инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), если значения по умолчанию недоступны, для столбца таблицы должны быть допустимы значения NULL. 
  
> [!NOTE]
> Эти квалификаторы отключают проверку определений DEFAULT для таблицы командами массового импорта.  Однако для одновременно выполняемых инструкций INSERT определения DEFAULT требуются.
 
## <a name="using-default-values-with-insert--select--from-openrowsetbulk"></a>Использование значений по умолчанию с помощью инструкции INSERT ... SELECT * FROM [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md)<a name="keep_default"></a>  
Можно указать, что вместо пустых полей в файле данных необходимо вставить значения по умолчанию соответствующих столбцов (если они заданы).  Чтобы использовать значения по умолчанию, используйте табличную подсказку [KEEPDEFAULTS](../../t-sql/queries/hints-transact-sql-table.md).
 
> [!NOTE]
>  Дополнительные сведения см. в статьях [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md), [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md), [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md) и [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md).

## <a name="example-test-conditions"></a>Пример условий теста<a name="etc"></a>  
Примеры в этом разделе основаны на таблице, файле данных и файле форматирования, которые определены ниже.

### <a name="sample-table"></a>**Образец таблицы**<a name="sample_table"></a>
Приведенный ниже сценарий создает тестовую базу данных и таблицу с именем `myNulls`.  Заметьте, что в четвертом столбце таблицы ( `Kids`) хранится значение по умолчанию.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNulls ( 
   PersonID smallint not null,
   FirstName varchar(25),
   LastName varchar(30),
   Kids varchar(13) DEFAULT 'Default Value',
   BirthDate date
   );
```

### <a name="sample-data-file"></a>**Образец файла данных**<a name="sample_data_file"></a>
С помощью Блокнота создайте пустой файл `D:\BCP\myNulls.bcp` и вставьте данные, приведенные ниже.  Обратите внимание, что в третьей записи в четвертом столбце значение отсутствует.

```
1,Anthony,Grosse,Yes,1980-02-23
2,Alica,Fatnowna,No,1963-11-14
3,Stella,Rosenhain,,1992-03-02
```

Кроме того, можно выполнить следующий сценарий PowerShell для создания и заполнения файла данных:

```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'MyNulls.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '1,Anthony,Grosse,Yes,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,No,1963-11-14';
Add-Content -Path $bcpFile -Value '3,Stella,Rosenhain,,1992-03-02';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```
  
### <a name="sample-non-xml-format-file"></a>**Образец файла форматирования в формате, отличном от XML**<a name="nonxml_format_file"></a>
SQL Server поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования.  Файл форматирования не в формате XML поддерживается более ранними версиями SQL Server.  Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myNulls.fmt`в формате, отличном от XML, на основе схемы `myNulls`.  Чтобы создать файл форматирования с помощью служебной программы [bcp](../../tools/bcp-utility.md) , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format также требует наличия параметра **-f** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, **t,** используется для указания запятой в качестве признака [конца поля](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), а **T** используется для указания доверенного подключения с использованием встроенной системы безопасности.  В командной строке введите следующую команду:

```cmd
bcp TestDatabase.dbo.myNulls format nul -c -f D:\BCP\myNulls.fmt -t, -T

REM Review file
Notepad D:\BCP\myNulls.fmt
```

> [!IMPORTANT]
> Убедитесь, что файл форматирования не в формате XML заканчивается символом перевода строки или возврата каретки.  В противном случае, скорее всего, появится следующее сообщение об ошибке:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

 Дополнительные сведения о создании файла форматирования см. в статье [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
## <a name="keep-nulls-or-use-default-values-during-bulk-import"></a>Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных<a name="import_data"></a>
В приведенных ниже примерах используется база данных, файл данных и файлы форматирования, созданные ранее.

### <a name="using-bcp-and-keeping-null-values-without-a-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и сохранение значений NULL без файла форматирования**<a name="bcp_null"></a>

Параметр **-k** .  В командной строке введите следующую команду:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### <a name="using-bcp-and-keeping-null-values-with-a-non-xml-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и сохранение значений NULL с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_null_fmt"></a>
Параметры **-k** и **-f** . В командной строке введите следующую команду:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T -k

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### <a name="using-bcp-and-using-default-values-without-a-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и значений по умолчанию без файла форматирования**<a name="bcp_default"></a>
В командной строке введите следующую команду:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -c -t, -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```
  
### <a name="using-bcp-and-using-default-values-with-a-non-xml-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и значений по умолчанию с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_default_fmt"></a>
Параметр **-f** .  В командной строке введите следующую команду:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNulls;"

REM Import data
bcp TestDatabase.dbo.myNulls IN D:\BCP\myNulls.bcp -f D:\BCP\myNulls.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNulls;"
```

### <a name="using-bulk-insert-and-keeping-null-values-without-a-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и сохранение значений NULL без файла форматирования**<a name="bulk_null"></a>
Аргумент **KEEPNULLS** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO
TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
    FROM 'D:\BCP\myNulls.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-keeping-null-values-with-a-non-xml-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и сохранение значений NULL с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_null_fmt"></a>
**KEEPNULLS** и аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls; -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt',
        KEEPNULLS
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-using-default-values-without-a-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и значений по умолчанию без файла форматирования**<a name="bulk_default"></a>
Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-bulk-insert-and-using-default-values-with-a-non-xml-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и значений по умолчанию с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_default_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
BULK INSERT dbo.myNulls
   FROM 'D:\BCP\myNulls.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNulls.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-openrowsetbulk-and-keeping-null-values-with-a-non-xml-format-file"></a>**Использование инструкции [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и сохранение значений NULL с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset__null_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

### <a name="using-openrowsetbulk-and-using-default-values-with-a-non-xml-format-file"></a>**Использование инструкции [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и значений по умолчанию с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset__default_fmt"></a>
Табличное указание **KEEPDEFAULTS** и аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myNulls;  -- for testing
INSERT INTO dbo.myNulls
WITH (KEEPDEFAULTS) 
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNulls.bcp', 
        FORMATFILE = 'D:\BCP\myNulls.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNulls;
```

  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Сохранение значений идентификаторов при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)  
  
-   [Подготовка данных к массовому экспорту или импорту (SQL Server)](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)  
  
 **Использование файла форматирования**  
  
-   [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
-   [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
 **Использование форматов данных для массового импорта или экспорта**  
  
-   [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
 **Задание форматов данных для совместимости с помощью программы bcp**  
  
-   [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
-   [Определение длины префикса в файлах данных с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
-   [Указание типа файлового хранилища с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)  
  
  

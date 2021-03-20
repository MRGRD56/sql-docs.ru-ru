---
title: Сохранение значений идентификаторов при массовом импорте данных
description: При массовом импорте данных, содержащих значения идентификаторов, в экземпляр SQL Server по умолчанию присваиваются новые значения. Вы можете сохранить исходные значения.
ms.date: 09/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server], bulk imports
- data formats [SQL Server], identity values
- bulk importing [SQL Server], identity values
ms.assetid: 45894a3f-2d8a-4edd-9568-afa7d0d3061f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: aa721f4597836068c07e5ee969c1eeb97dd20670
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752534"
---
# <a name="keep-identity-values-when-bulk-importing-data-sql-server"></a>Сохранение значений идентификаторов при массовом импорте данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Для файлов данных, содержащих значения идентификаторов, можно выполнить массовый импорт в экземпляр Microsoft SQL Server.  По умолчанию значения столбца идентификаторов в импортируемом файле данных не учитываются, и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически присваивает им уникальные значения  на основе начального значения и значения приращения, указанных при создании таблицы.

Если файл данных не содержит значений для столбцов идентификаторов в таблице, то для указания того, что при импорте столбец идентификаторов в таблице нужно пропустить, применяется файл форматирования.  Дополнительные сведения см. в статье [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md) .

|Контур|
|---|
|[Сохранение значений идентификаторов](#keep_identity)<br />[Пример условий теста](#etc)<br />&emsp;&#9679;&emsp;[Образец таблицы](#sample_table)<br />&emsp;&#9679;&emsp;[Образец файла данных](#sample_data_file)<br />&emsp;&#9679;&emsp;[Образец файла форматирования в формате, отличном от XML](#nonxml_format_file)<br />[Примеры](#examples)<br />&emsp;&#9679;&emsp;[Использование команды bcp и сохранение значений идентификаторов без файла форматирования](#bcp_identity)<br />&emsp;&#9679;&emsp;[Использование команды bcp и сохранение значений идентификаторов с файлом форматирования](#bcp_identity_fmt)<br />&emsp;&#9679;&emsp;[Использование команды bcp и созданных значений идентификаторов без файла форматирования](#bcp_default)<br />&emsp;&#9679;&emsp;[Использование команды bcp и созданных значений идентификаторов с помощью файла форматирования в формате, отличном от XML](#bcp_default_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и сохранение значений идентификаторов без файла форматирования](#bulk_identity)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и сохранение значений идентификаторов с помощью файла форматирования в формате, отличном от XML](#bulk_identity_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и созданных значений идентификаторов без файла форматирования](#bulk_default)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и созданных значений идентификаторов с помощью файла форматирования в формате, отличном от XML](#bulk_default_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции OPENROWSET и сохранение значений идентификаторов с помощью файла форматирования в формате, отличном от XML](#openrowset_identity_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции OPENROWSET и созданных значений идентификаторов с помощью файла форматирования в формате, отличном от XML](#openrowset_default_fmt)<br /><p>                                                                                                                                                                                                                  </p>|

## <a name="keep-identity-values"></a>Сохранение значений идентификаторов <a name="keep_identity"></a>  
Чтобы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не присваивал значения идентификаторов при массовом импорте строк в таблицу, используется соответствующий квалификатор команды для сохранения значений идентификаторов.  При указании квалификатора сохранения значений идентификаторов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] пользуется значениями идентификаторов, имеющимися в файле данных.  Ниже приведены эти квалификаторы.

|Get-Help|Квалификатор сохранения значений идентификаторов|Тип квалификатора|  
|-------------|------------------------------|--------------------|  
|bcp|-E|Параметр|  
|BULK INSERT|KEEPIDENTITY|Аргумент|  
|Инструкции INSERT ... SELECT * FROM OPENROWSET(BULK...).|KEEPIDENTITY|Табличное указание|  
   
 Дополнительные сведения см. в статьях [Программа bcp](../../tools/bcp-utility.md), [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md), [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md), [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md), [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md) и [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md).  

> [!NOTE]
>  Сведения о том, как создать автоматически увеличивающееся числовое значение, которое может использоваться в нескольких таблицах или вызываться из приложений без ссылки на какие-либо таблицы, см. в разделе [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md).
 
## <a name="example-test-conditions"></a>Пример условий теста<a name="etc"></a>  
Примеры в этом разделе основаны на таблице, файле данных и файле форматирования, которые определены ниже.

### <a name="sample-table"></a>**Образец таблицы**<a name="sample_table"></a>
Приведенный ниже сценарий создает тестовую базу данных и таблицу с именем `myIdentity`.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myIdentity ( 
   PersonID smallint IDENTITY(1,1) NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date
   );
```
 
### <a name="sample-data-file"></a>**Образец файла данных**<a name="sample_data_file"></a>
С помощью Блокнота создайте пустой файл `D:\BCP\myIdentity.bcp` и вставьте данные, приведенные ниже.  
```
3,Anthony,Grosse,1980-02-23
2,Alica,Fatnowna,1963-11-14
1,Stella,Rosenhain,1992-03-02
4,Miller,Dylan,1954-01-05
```
  
Кроме того, можно выполнить следующий сценарий PowerShell для создания и заполнения файла данных:
```powershell
cls
# revise directory as desired
$dir = 'D:\BCP\';

$bcpFile = $dir + 'myIdentity.bcp';

# Confirm directory exists
IF ((Test-Path -Path $dir) -eq 0)
{
    Write-Host "The path $dir does not exist; please create or modify the directory.";
    RETURN;
};

# clear content, will error if file does not exist, can be ignored
Clear-Content -Path $bcpFile -ErrorAction SilentlyContinue;

# Add data
Add-Content -Path $bcpFile -Value '3,Anthony,Grosse,1980-02-23';
Add-Content -Path $bcpFile -Value '2,Alica,Fatnowna,1963-11-14';
Add-Content -Path $bcpFile -Value '1,Stella,Rosenhain,1992-03-02';
Add-Content -Path $bcpFile -Value '4,Miller,Dylan,1954-01-05';

#Review content
Get-Content -Path $bcpFile;
Invoke-Item $bcpFile;
```

### <a name="sample-non-xml-format-file"></a>**Образец файла форматирования в формате, отличном от XML**<a name="nonxml_format_file"></a>
SQL Server поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования.  Файл форматирования не в формате XML поддерживается более ранними версиями SQL Server.  Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myIdentity.fmt`в формате, отличном от XML, на основе схемы `myIdentity`.  Чтобы создать файл форматирования с помощью служебной программы [bcp](../../tools/bcp-utility.md) , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format также требует наличия параметра **-f** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, **t,** используется для указания запятой в качестве признака [конца поля](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md), а **T** используется для указания доверенного подключения с использованием встроенной системы безопасности.  В командной строке введите следующую команду:
  
```
bcp TestDatabase.dbo.myIdentity format nul -c -f D:\BCP\myIdentity.fmt -t, -T

REM Review file
Notepad D:\BCP\myIdentity.fmt
```
 
> [!IMPORTANT]
> Убедитесь, что файл форматирования не в формате XML заканчивается символом перевода строки или возврата каретки.  В противном случае, скорее всего, появится следующее сообщение об ошибке:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Примеры<a name="examples"></a>
В приведенных ниже примерах используется база данных, файл данных и файлы форматирования, созданные ранее.
  
### <a name="using-bcp-and-keeping-identity-values-without-a-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и сохранение значений идентификаторов без файла форматирования**<a name="bcp_identity"></a>
Параметр **-E** .  В командной строке введите следующую команду:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t, -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```

### <a name="using-bcp-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и сохранение значений идентификаторов с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_identity_fmt"></a>
Параметры **-E** и **-f** .  В командной строке введите следующую команду:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T -E

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
 
### <a name="using-bcp-and-generated-identity-values-without-a-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и созданных значений идентификаторов без файла форматирования**<a name="bcp_default"></a>
Использование значений по умолчанию.  В командной строке введите следующую команду:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -T -c -t,

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### <a name="using-bcp-and-generated-identity-values-with-a-non-xml-format-file"></a>**Использование команды [bcp](../../tools/bcp-utility.md) и созданных значений идентификаторов с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bcp_default_fmt"></a>
Использование значений по умолчанию и параметра **-f** .  В командной строке введите следующую команду:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myIdentity;"

REM Import data
bcp TestDatabase.dbo.myIdentity IN D:\BCP\myIdentity.bcp -f D:\BCP\myIdentity.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myIdentity;"
```
  
### <a name="using-bulk-insert-and-keeping-identity-values-without-a-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и сохранение значений идентификаторов без файла форматирования**<a name="bulk_identity"></a>
Аргумент **KEEPIDENTITY** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
    FROM 'D:\BCP\myIdentity.bcp'
    WITH (
        DATAFILETYPE = 'char',  
        FIELDTERMINATOR = ',',  
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и сохранение значений идентификаторов с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_identity_fmt"></a>
**KEEPIDENTITY** и аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity; -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt',
        KEEPIDENTITY
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-generated-identity-values-without-a-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и созданных значений идентификаторов без файла форматирования**<a name="bulk_default"></a>
Использование значений по умолчанию.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ','
      );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-bulk-insert-and-generated-identity-values-with-a-non-xml-format-file"></a>**Использование инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и созданных значений идентификаторов с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="bulk_default_fmt"></a>
Использование значений по умолчанию и аргумента **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
BULK INSERT dbo.myIdentity
   FROM 'D:\BCP\myIdentity.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myIdentity.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
### <a name="using-openrowsetbulk-and-keeping-identity-values-with-a-non-xml-format-file"></a>**Использование инструкции [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и сохранение значений идентификаторов с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset_identity_fmt"></a>
Табличное указание **KEEPIDENTITY** и аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
WITH (KEEPIDENTITY) 
(PersonID, FirstName, LastName, BirthDate)
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
 
### <a name="using-openrowsetbulk-and-generated-identity-values-with-a-non-xml-format-file"></a>**Использование инструкции [OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) и созданных значений идентификаторов с помощью [файла форматирования в формате, отличном от XML](../../relational-databases/import-export/non-xml-format-files-sql-server.md)** <a name="openrowset_default_fmt"></a>
Использование значений по умолчанию и аргумента **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
USE TestDatabase;
GO

TRUNCATE TABLE dbo.myIdentity;  -- for testing
INSERT INTO dbo.myIdentity
(FirstName, LastName, BirthDate)
    SELECT FirstName, LastName, BirthDate
    FROM OPENROWSET (
        BULK 'D:\BCP\myIdentity.bcp', 
        FORMATFILE = 'D:\BCP\myIdentity.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myIdentity;
```
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Сохранение значений NULL или использование значений по умолчанию при массовом импорте данных (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)  
  
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
  
1.  [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
2.  [Определение длины префикса в файлах данных с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
3.  [Указание типа файлового хранилища с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)  
[Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  

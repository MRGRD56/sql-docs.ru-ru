---
title: Использование символьного формата Юникода для импорта и экспорта данных
description: Формат символьных данных Юникода позволяет экспортировать данные из экземпляра SQL Server с помощью кодовой страницы, отличающейся от кодовой страницы, используемой клиентом.
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], Unicode character
- Unicode [SQL Server], bulk importing and exporting
ms.assetid: 74342a11-c1c0-4746-b482-7f3537744a70
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 8e763f88431b2ac605a39c7bd5d43b3d51176d2a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751464"
---
# <a name="use-unicode-character-format-to-import-or-export-data-sql-server"></a>Использование символьного формата Юникода для импорта и экспорта данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Символьный формат Юникода рекомендуется для массового переноса данных между несколькими экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] через файл данных, содержащий символы расширенной или двухбайтовой кодировки (DBCS). Формат символьных данных Юникода позволяет экспортировать данные из сервера в кодовой странице, отличающейся от кодовой страницы, используемой выполняющим операцию клиентом. В этих случаях использование символьного формата Юникода имеет следующие преимущества.  
  
* Если данные источника и назначения имеют тип данных Юникода, при использовании символьного формата Юникода все символьные данные сохраняются.  
  
* Если данные источника и назначения имеют тип данных, отличный от Юникода, использование символьного формата Юникода позволяет свести к минимуму потери дополнительных символов данных из источника, которые не могут быть представлены в назначении.

|В этом разделе.|
|---|
|[Рекомендации по использованию символьного формата Юникода](#considerations)|
|[Особые рекомендации по использованию символьного формата Юникода, bcp и файла форматирования](#special_considerations)|
|[Параметры команд для символьного формата Юникода](#command_options)|
|[Пример условий теста](#etc)<br />&emsp;&#9679;&emsp;[Образец таблицы](#sample_table)<br />&emsp;&#9679;&emsp;[Образец файла форматирования в формате, отличном от XML](#nonxml_format_file)|
|[Примеры](#examples)<br />&emsp;&#9679;&emsp;[Использование bcp и символьного формата Юникода для экспорта данных](#bcp_widechar_export)<br />&emsp;&#9679;&emsp;[Использование bcp и символьного формата Юникода для импорта данных без файла форматирования](#bcp_widechar_import)<br />&emsp;&#9679;&emsp;[Использование bcp и символьного формата Юникода для импорта данных с файлом форматирования, не являющимся XML](#bcp_widechar_import_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и символьного формата Юникода без файла форматирования](#bulk_widechar)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и символьного формата Юникода с файлом форматирования, не являющимся XML](#bulk_widechar_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции OPENROWSET и символьного формата Юникода с файлом форматирования, не являющимся XML](#openrowset_widechar_fmt)|
|[Связанные задачи](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
 
## <a name="considerations-for-using-unicode-character-format"></a>Рекомендации по использованию символьного формата Юникода<a name="considerations"></a>
При использовании символьного формата Юникода имейте в виду следующее.  

* По умолчанию [программа bcp](../../tools/bcp-utility.md) разделяет символьные поля данных символом табуляции, а записи — символом перевода строки.  Сведения о том, как указать другой признак конца поля, см. в статье [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

* Данные типа [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) , хранящиеся в файле данных символьного формата Юникод, обрабатываются таким же образом, что и данные файла данных символьного формата, за исключением того, что они хранятся как данные типа данных [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) , а не как данные типа [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) . Дополнительные сведения о формате символов см. в разделе [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md).  

## <a name="special-considerations-for-using-unicode-character-format-bcp-and-a-format-file"></a>Особые рекомендации по использованию символьного формата Юникода, bcp и файла форматирования<a name="special_considerations"></a>
Файлы данных символьного формата Юникода следуют соглашениям для файлов Юникода.  Первые два байта файла являются шестнадцатеричными числами 0xFFFE.  Эти байты служат в качестве меток порядка байтов, определяющих, хранится ли старший байт в файле первым или последним.  [Программа bcp](../../tools/bcp-utility.md) может неправильно интерпретировать метки порядка байтов и вызвать сбой части импорта. Вы можете получить сообщение об ошибке, аналогичное следующему:
```
Starting copy...
SQLState = 22005, NativeError = 0
Error = [Microsoft][ODBC Driver 13 for SQL Server]Invalid character value for cast specification
```

Метки порядка байтов могут быть неправильно интерпретированы при следующих условиях:
* для указания символа Юникода используется [программа bcp](../../tools/bcp-utility.md) и параметр **-w** ;

* используется файл форматирования;

* первое поле в файле данных не содержит символы.

Узнайте, можно ли использовать какой-либо из следующих обходных путей для решения *вашей конкретной* задачи:
* Не используйте файл форматирования.  Пример этого обходного пути представлен ниже: ознакомьтесь с разделом [Использование bcp и символьного формата Юникода для импорта данных без файла форматирования](#bcp_widechar_import).

* Используйте параметр **-c** вместо **-w**.

* Повторно экспортируйте данные, используя собственный формат.

* Используйте инструкцию [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) или [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md).  Примеры этого обходного пути представлены ниже: ознакомьтесь с разделами [Использование BULK INSERT и символьного формата Юникода с файлом форматирования, не являющимся XML](#bulk_widechar_fmt) и [Использование OPENROWSET и символьного формата Юникода с файлом форматирования, не являющимся XML](#openrowset_widechar_fmt).
 
* Вручную вставьте первую запись в целевой таблице, а затем используйте параметр **-F 2** , чтобы начать импорт со второй записи.

* Вручную вставьте первую фиктивную запись в файл данных, а затем используйте параметр **-F 2** , чтобы начать импорт со второй записи.  Пример этого обходного пути представлен ниже: ознакомьтесь с разделом [Использование bcp и символьного формата Юникода для импорта данных с файлом форматирования, не являющимся XML](#bcp_widechar_import_fmt).

* Используйте промежуточную таблицу, в которой первый столбец имеет символьный тип данных, или

* повторно экспортируйте данные и измените порядок полей данных так, чтобы первое поле данных содержало символы.  После этого используйте файл форматирования для повторного сопоставления поля данных с фактическим порядком в таблице.  Например, ознакомьтесь с разделом [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md).
  
## <a name="command-options-for-unicode-character-format"></a>Параметры команд для символьного формата Юникода<a name="command_options"></a>  
Импортировать в таблицу данные в символьном формате Юникода можно при помощи программы [bcp](../../tools/bcp-utility.md), инструкций [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) или [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Для команды [bcp](../../tools/bcp-utility.md) или инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) формат данных можно указать в инструкции.  Для инструкции [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) нужно указать формат данных в файле форматирования.  
  
Символьный формат Юникода поддерживается следующими параметрами командной строки:  
  
|Get-Help|Параметр|Описание|  
|-------------|------------|-----------------|  
|bcp|**-w**|Использует символьный формат Юникода.|  
|BULK INSERT|DATAFILETYPE **="widechar"**|Использует символьный формат Юникода при массовом импорте данных.|  
|OPENROWSET|Недоступно|Требуется использовать файл форматирования.|
  
> [!NOTE]
>  Также в файле форматирования можно указать форматирование для каждого поля. Дополнительные сведения см в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  
## <a name="example-test-conditions"></a>Пример условий теста<a name="etc"></a>  
Примеры в этой статье основаны на таблице и файле форматирования, которые определены ниже.

### <a name="sample-table"></a>**Образец таблицы**<a name="sample_table"></a>
Приведенный ниже скрипт создает тестовую базу данных, таблицу с именем `myWidechar` и заполняет таблицу начальными значениями.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidechar ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidechar
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidechar;
```

### <a name="sample-non-xml-format-file"></a>**Образец файла форматирования в формате, отличном от XML**<a name="nonxml_format_file"></a>
SQL Server поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования.  Файл форматирования не в формате XML поддерживается более ранними версиями SQL Server.  Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myWidechar.fmt`в формате, отличном от XML, на основе схемы `myWidechar`.  Чтобы создать файл форматирования с помощью служебной программы [bcp](../../tools/bcp-utility.md) , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format также требует наличия параметра **-f** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, а **T** используется для указания доверенного подключения, в рамках которого применяется встроенная система безопасности.  В командной строке введите следующие команды:

```
bcp TestDatabase.dbo.myWidechar format nul -f D:\BCP\myWidechar.fmt -T -w

REM Review file
Notepad D:\BCP\myWidechar.fmt
```

> [!IMPORTANT]
> Убедитесь, что файл форматирования не в формате XML заканчивается символом перевода строки или возврата каретки.  В противном случае, скорее всего, появится следующее сообщение об ошибке:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`


## <a name="examples"></a>Примеры<a name="examples"></a>
В приведенных ниже примерах используется база данных и файлы форматирования, созданные ранее.

### <a name="using-bcp-and-unicode-character-format-to-export-data"></a>**Использование bcp и символьного формата Юникода для экспорта данных**<a name="bcp_widechar_export"></a>
Параметр **-w** и команда **OUT** .  Примечание. Файл данных, созданный в этом примере, будет использоваться во всех последующих примерах.  В командной строке введите следующие команды:
```
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w

REM Review results
NOTEPAD D:\BCP\myWidechar.bcp
```

### <a name="using-bcp-and-unicode-character-format-to-import-data-without-a-format-file"></a>**Использование bcp и символьного формата Юникода для импорта данных без файла форматирования**<a name="bcp_widechar_import"></a>
Параметр **-w** и команда **IN** .  В командной строке введите следующие команды:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Import data
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -T -w

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-character-format-to-import-data-with-a-non-xml-format-file"></a>**Использование bcp и символьного формата Юникода для импорта данных с файлом форматирования, не являющимся XML**<a name="bcp_widechar_import_fmt"></a>
Параметры **-w** и **-f** switches и **IN** commи.  Так как этот пример включает bcp, файл форматирования, символ Юникода, а первое поле данных в файле данных не содержит символы, потребуется использовать обходной путь.  См. раздел [Особые рекомендации по использованию символьного формата Юникода, bcp и файла форматирования](#special_considerations)выше.  Файл данных `myWidechar.bcp` будет изменен: в него будет добавлена дополнительная фиктивная запись, которая затем будет пропущена по параметру `-F 2`.

В командной строке введите следующие команды и внесите требуемые изменения:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidechar;"

REM Open data file
Notepad D:\BCP\myWidechar.bcp
REM Copy first record and then paste as new first record.  This additional record is the "dummy" record.
REM Close file.

REM Import data instructing bcp to skip dummy record with the -F 2 switch.
bcp TestDatabase.dbo.myWidechar IN D:\BCP\myWidechar.bcp -f D:\BCP\myWidechar.fmt -T -F 2

REM Review results is SSMS

REM Return data file to original state for usage in other examples
bcp TestDatabase.dbo.myWidechar OUT D:\BCP\myWidechar.bcp -T -w
```
  
### <a name="using-bulk-insert-and-unicode-character-format-without-a-format-file"></a>**Использование инструкции BULK INSERT и символьного формата Юникода без файла форматирования**<a name="bulk_widechar"></a>
Аргумент **DATAFILETYPE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
    FROM 'D:\BCP\myWidechar.bcp'
    WITH (
        DATAFILETYPE = 'widechar'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### <a name="using-bulk-insert-and-unicode-character-format-with-a-non-xml-format-file"></a>**Использование инструкции BULK INSERT и символьного формата Юникода с файлом форматирования, не являющимся XML**<a name="bulk_widechar_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar; -- for testing
BULK INSERT TestDatabase.dbo.myWidechar
   FROM 'D:\BCP\myWidechar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidechar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
  
### <a name="using-openrowset-and-unicode-character-format-with-a-non-xml-format-file"></a>**Использование инструкции OPENROWSET и символьного формата Юникода с файлом форматирования, не являющимся XML**<a name="openrowset_widechar_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidechar;  -- for testing
INSERT INTO TestDatabase.dbo.myWidechar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidechar.bcp', 
        FORMATFILE = 'D:\BCP\myWidechar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidechar;
```
 
  
## <a name="related-tasks"></a>Связанные задачи<a name="RelatedTasks"></a>
Использование форматов данных для массового импорта или экспорта  
-   [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  

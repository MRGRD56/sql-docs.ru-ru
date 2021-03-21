---
title: Использование символьного формата для импорта и экспорта данных
description: В символьном формате все столбцы представлены в формате символьных данных. Это удобно для работы с другими программами или копирования в экземпляр от другого поставщика баз данных.
ms.date: 09/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], character
- character formats [SQL Server]
ms.assetid: d925e66a-1a73-43cd-bc06-1cbdf8174a4d
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: cb3a2d60145ceeadd1a7d13cb8a6cb788e2a0131
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753654"
---
# <a name="use-character-format-to-import-or-export-data-sql-server"></a>Использование символьного формата для импорта или экспорта данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Символьный формат рекомендуется применять при выполнении массового экспорта данных в текстовый файл, который предназначен для использования в других программах, а также при выполнении массового импорта данных из текстового файла, созданного другими программами.  

В символьном формате все столбцы представлены в формате символьных данных. Хранение данных в символьном формате удобно в том случае, когда данные используются в других программах (например электронных таблицах) или когда на экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо перенести данные из базы данных другого поставщика (например Oracle).  
  
> [!NOTE]
>  При массовом переносе данных между экземплярами [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , если файл данных содержит символьные данные в Юникоде, но не содержит расширенные символы и символы в двухбайтовой кодировке (DBCS), используйте символы в формате Юникод. Дополнительные сведения см. в разделе [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).
  
|В этом разделе.|
|---|
|[Замечания по использованию символьного формата](#considerations)|
|[Параметры команд для символьного формата](#command_options)|
|[Пример условий теста](#etc)<br />&emsp;&#9679;&emsp;[Образец таблицы](#sample_table)<br />&emsp;&#9679;&emsp;[Образец файла форматирования в формате, отличном от XML](#nonxml_format_file)<br />|
|[Примеры](#examples)<br />&emsp;&#9679;&emsp;[Использование bcp и символьного формата для экспорта данных](#bcp_char_export)<br />&emsp;&#9679;&emsp;[Использование bcp и символьного формата для импорта данных без файла форматирования](#bcp_char_import)<br />&emsp;&#9679;&emsp;[Использование bcp и символьного формата для импорта данных с файлом форматирования, не являющимся XML-файлом](#bcp_char_import_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и символьного формата без файла форматирования](#bulk_char)<br />&emsp;&#9679;&emsp;[Использование инструкции BULK INSERT и символьного формата с файлом форматирования, не являющимся XML-файлом](#bulk_char_fmt)<br />&emsp;&#9679;&emsp;[Использование инструкции OPENROWSET и символьного формата с файлом форматирования, не являющимся XML-файлом](#openrowset_char_fmt)|
|[Связанные задачи](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|


  
## <a name="considerations-for-using-character-format"></a>Замечания по использованию символьного формата<a name="considerations"></a>
При использовании символьного формата имейте в виду следующее.  
  
-   По умолчанию [программа bcp](../../tools/bcp-utility.md) разделяет символьные поля данных символом табуляции, а записи — символом перевода строки.  Сведения о том, как указать другой признак конца поля, см. в статье [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
-   По умолчанию перед выполнением массового импорта или экспорта символьных данных выполняются следующие преобразования.  
  
    |Направление массовой операции|Преобразование|  
    |---------------------------------|----------------|  
    |Экспорт|Преобразует данные в символьное представление. Если это явно запрошено, данные в символьных столбцах преобразуются в требуемую кодовую страницу. Если кодовая страница не указана, символьные данные преобразуются в кодовую страницу изготовителя оборудования (OEM) клиентского компьютера.|  
    |Импорт|Если необходимо, преобразует символьные данные в собственное представление, а также преобразует данные из кодовой страницы клиента в кодовую страницу целевого столбца.|  
  
-   Чтобы предотвратить потерю дополнительных символов при преобразовании, применяйте символьный формат Юникода либо укажите кодовую страницу.  
  
-   Значения типа [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) сохраняются в файле в символьном формате без метаданных. Каждое значение типа данных преобразуется в формат типа [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) в соответствии с правилами неявного преобразования данных. В столбец типа [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) данные импортируются как тип [char](../../t-sql/data-types/char-and-varchar-transact-sql.md). При импорте в столбец типа данных, отличного от типа [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), данные преобразуются из типа [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) в соответствии с правилами неявного преобразования. Дополнительные сведения о преобразовании данных см. в статье [Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
-   [Программа bcp](../../tools/bcp-utility.md) экспортирует значения [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) в символьный формат с четырьмя знаками после запятой и без символов-разделителей групп разрядов, таких как запятая. Например: для столбца типа [money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md) , содержащего значение 1 234 567,123456, будет выполнен массовый экспорт в файл данных в виде символьной строки "1234567,1235".  
  
## <a name="command-options-for-character-format"></a>Параметры команд для символьного формата<a name="command_options"></a>  
Для импорта символьных данных в таблицу используются команда [bcp](../../tools/bcp-utility.md), инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) или [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md). Для команды [bcp](../../tools/bcp-utility.md) или инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) формат данных можно указать в инструкции.  Для инструкции [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) нужно указать формат данных в файле форматирования.  
  
Символьный формат поддерживается следующими параметрами командной строки:  
  
|Get-Help|Параметр|Описание|  
|-------------|------------|-----------------|  
|bcp|**-c**|Предписывает служебной программе bcp использовать символьные данные.\*|  
|BULK INSERT|DATAFILETYPE **='char'**|Использует символьный формат при массовом импорте данных.|  
|OPENROWSET|Недоступно|Требуется использовать файл форматирования.|
  
 \* Чтобы загрузить символьные ( **-c**) данные в формате, совместимом с клиентами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предыдущих версий, используйте параметр **-V** . Дополнительные сведения см. в разделе [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
   
> [!NOTE]
>  Также в файле форматирования можно указать форматирование для каждого поля. Дополнительные сведения см в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).

## <a name="example-test-conditions"></a>Пример условий теста<a name="etc"></a>  
Примеры в этой статье основаны на таблице и файле форматирования, которые определены ниже.

### <a name="sample-table"></a>**Образец таблицы**<a name="sample_table"></a>
Приведенный ниже скрипт создает тестовую базу данных, таблицу с именем `myChar` и заполняет таблицу начальными значениями.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myChar ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myChar
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="sample-non-xml-format-file"></a>**Образец файла форматирования в формате, отличном от XML**<a name="nonxml_format_file"></a>
SQL Server поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования.  Файл форматирования не в формате XML поддерживается более ранними версиями SQL Server.  Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myChar.fmt`в формате, отличном от XML, на основе схемы `myChar`.  Чтобы создать файл форматирования с помощью служебной программы [bcp](../../tools/bcp-utility.md) , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format также требует наличия параметра **-f** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, а **T** используется для указания доверенного подключения, в рамках которого применяется встроенная система безопасности.  В командной строке введите следующую команду:

```cmd
bcp TestDatabase.dbo.myChar format nul -f D:\BCP\myChar.fmt -T -c 

REM Review file
Notepad D:\BCP\myChar.fmt
```

> [!IMPORTANT]
> Убедитесь, что файл форматирования не в формате XML заканчивается символом перевода строки или возврата каретки.  В противном случае, скорее всего, появится следующее сообщение об ошибке:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Примеры<a name="examples"></a>
В приведенных ниже примерах используется база данных и файлы форматирования, созданные ранее.

### <a name="using-bcp-and-character-format-to-export-data"></a>**Использование bcp и символьного формата для экспорта данных**<a name="bcp_char_export"></a>
Параметр **-c** и команда **OUT** .  Примечание. Файл данных, созданный в этом примере, будет использоваться во всех последующих примерах.  В командной строке введите следующую команду:

```cmd
bcp TestDatabase.dbo.myChar OUT D:\BCP\myChar.bcp -T -c

REM Review results
NOTEPAD D:\BCP\myChar.bcp
```

### <a name="using-bcp-and-character-format-to-import-data-without-a-format-file"></a>**Использование bcp и символьного формата для импорта данных без файла форматирования**<a name="bcp_char_import"></a>
Параметр **-c** и команда **IN** .  В командной строке введите следующую команду:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -T -c

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### <a name="using-bcp-and-character-format-to-import-data-with-a-non-xml-format-file"></a>**Использование bcp и символьного формата для импорта данных с файлом форматирования, не являющимся XML**<a name="bcp_char_import_fmt"></a>
Параметры **-c** и **-f** switches и **IN** commи.  В командной строке введите следующую команду:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myChar;"

REM Import data
bcp TestDatabase.dbo.myChar IN D:\BCP\myChar.bcp -f D:\BCP\myChar.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myChar;"
```

### <a name="using-bulk-insert-and-character-format-without-a-format-file"></a>**Использование инструкции BULK INSERT и символьного формата без файла форматирования**<a name="bulk_char"></a>
Аргумент **DATAFILETYPE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
    FROM 'D:\BCP\myChar.bcp'
    WITH (
        DATAFILETYPE = 'Char'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="using-bulk-insert-and-character-format-with-a-non-xml-format-file"></a>**Использование инструкции BULK INSERT и символьного формата с файлом форматирования, не являющимся XML**<a name="bulk_char_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar; -- for testing
BULK INSERT TestDatabase.dbo.myChar
   FROM 'D:\BCP\myChar.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myChar.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```

### <a name="using-openrowset-and-character-format-with-a-non-xml-format-file"></a>**Использование инструкции OPENROWSET и символьного формата с файлом форматирования, не являющимся XML**<a name="openrowset_char_fmt"></a>
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myChar;  -- for testing
INSERT INTO TestDatabase.dbo.myChar
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myChar.bcp', 
        FORMATFILE = 'D:\BCP\myChar.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myChar;
```
  
## <a name="related-tasks"></a>Связанные задачи<a name="RelatedTasks"></a>  
Использование форматов данных для массового импорта или экспорта 
  
-   [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
  

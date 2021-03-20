---
title: Использование собственного формата для импорта и экспорта данных
description: При импорте или экспорте в SQL Server собственный формат сохраняет собственные типы данных базы данных, обеспечивая высокоскоростную передачу данных между таблицами SQL Server.
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- data formats [SQL Server], native
ms.assetid: eb279b2f-0f1f-428f-9b8f-2a7fc495b79f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 30b9d8f332c1dbb8147b836309724f2216cb9de1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750404"
---
# <a name="use-native-format-to-import-or-export-data-sql-server"></a>Использование собственного формата для импорта или экспорта данных (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Собственный формат данных рекомендуется использовать при массовой передаче данных между несколькими экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] через файл данных, который не содержит символов в расширенной кодировке или символов в двухбайтовой кодировке (DBCS).  

> [!NOTE]
>  При массовой передаче данных между несколькими экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при помощи файла данных, содержащего символы в расширенной кодировке или символы DBCS, необходимо использовать собственный формат в Юникоде. Дополнительные сведения см. в разделе [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).

В собственном формате используются собственные типы данных базы данных. Собственный формат предназначен для высокоскоростной передачи данных между таблицами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Если используется файл форматирования, то исходная и целевая таблицы не обязаны быть идентичными. Передача данных происходит в два этапа:  
  
1.  Массовый экспорт данных из исходной таблицы в файл данных.  
  
2.  Массовый импорт данных из файла данных в целевую таблицу.  

Использование собственного формата между идентичными таблицами позволяет избежать бесполезного преобразования типов данных из символьного формата и обратно, экономя тем самым время и пространство хранения. Для достижения оптимальной скорости передачи, однако, уменьшается количество проверок форматирования данных. Чтобы избежать проблемы с загруженными данными, изучите следующий список ограничений.  


## <a name="restrictions"></a>Ограничения
Чтобы успешно импортировать данные в собственный формат, убедитесь, что:  
  
-   Файл данных создан в собственном формате.  
  
-   Либо целевая таблица должна быть совместима с файлом данных (иметь верное число столбцов, типы данных, длину, допустимость значений NULL и так далее), либо необходимо использовать файл форматирования для сопоставления каждого поля с соответствующим столбцом.  
  
    > [!NOTE]
    >  Если импортируются данные из файла, не совпадающего по структуре с целевой таблицей, то, хотя операция импорта может завершиться успешно, вставляемые в целевую таблицу данные, скорее всего, будут неправильными. Это происходит из-за того, что данные из файла интерпретируются при помощи формата целевой таблицы. Таким образом, любые различия приведут к вставке неправильных данных. Однако ни при каких обстоятельствах подобные различия не могут привести к логическому или физическому несоответствию в базе данных.  
  
     Дополнительные сведения см. в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 Успешный импорт не приводит к повреждению целевой таблицы.  
  
## <a name="how-bcp-handles-data-in-native-format"></a>Как bcp обрабатывает данные в собственном формате
 В этом разделе обсуждаются особые аспекты выполнения программой **bcp** операций экспорта и импорта данных в собственном формате.  
  
-   Несимвольные данные.  
  
     [Программа bcp](../../tools/bcp-utility.md) использует для записи несимвольных данных из таблицы в файл данных внутренний двоичный формат данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Данные типа[char](../../t-sql/data-types/char-and-varchar-transact-sql.md) или [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) .  
  
     В начале каждого поля [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) или [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) программа [bcp](../../tools/bcp-utility.md) добавляет длину префикса.  
  
    > [!IMPORTANT]
    >  По умолчанию при использовании собственного режима программа [bcp](../../tools/bcp-utility.md) перед копированием в файл данных преобразует символы из формата [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в символы OEM. [Программа bcp](../../tools/bcp-utility.md) преобразует символы из файла данных в символы ANSI перед их массовым импортом в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Во время подобных преобразований расширенные символьные данные могут быть потеряны. Для расширенных наборов символов необходимо либо использовать собственный формат в Юникоде, либо задать кодовую страницу.
  
-   [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) data  
  
     Если данные типа [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) сохраняются как SQLVARIANT в файле данных в собственном формате, то все характеристики данных сохраняются. Метаданные, в которых записан тип данных каждой величины, записываются вместе со значениями данных. Эти метаданные используются для повторного создания значений данных с тем же типом данных, что и в столбце назначения [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) .  
  
     Если в столбце назначения тип данных отличается от [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), то каждое значение данных преобразуется в тип данных столбца назначений согласно стандартным правилам неявного преобразования данных. При возникновении ошибки во время преобразования данных происходит откат текущего пакета. С любыми значениями [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) и [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md), которые передаются между столбцами [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), могут возникнуть проблемы преобразования кодовых страниц.  
  
     Дополнительные сведения о преобразовании данных см. в статье [Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
## <a name="command-options-for-native-format"></a>Параметры командной строки для собственного формата
Импортировать в таблицу данные в собственном формате можно при помощи программы [bcp](../../tools/bcp-utility.md), инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) или [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Для команды [bcp](../../tools/bcp-utility.md) или инструкции [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) формат данных можно указать в инструкции.  Для инструкции [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md) нужно указать формат данных в файле форматирования.  

Собственный формат поддерживается следующими параметрами командной строки:  

|Get-Help|Параметр|Описание|  
|-------------|------------|-----------------|  
|bcp|**-n**|Приводит к использованию служебной программой bcp собственных типов данных. *|  
|BULK INSERT|DATAFILETYPE **="native"**|Использует собственный тип данных или расширенный собственный тип данных. Учтите, что параметр DATAFILETYPE не нужен, если типы данных указываются в файле форматирования.|  
|OPENROWSET|Недоступно|Требуется использовать файл форматирования.|

  
 \*Чтобы загрузить собственные ( **-n**) данные в формате, совместимом с клиентами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предыдущих версий, используйте параметр **-V** . Дополнительные сведения см. в разделе [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
> [!NOTE]
>  Также в файле форматирования можно указать форматирование для каждого поля. Дополнительные сведения см в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  

## <a name="example-test-conditions"></a>Пример условий теста
Примеры в этой статье основаны на таблице и файле форматирования, которые определены ниже.

### <a name="sample-table"></a>Пример таблицы
Приведенный ниже скрипт создает тестовую базу данных, таблицу с именем `myNative` и заполняет таблицу начальными значениями.  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNative ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myNative
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="sample-non-xml-format-file"></a>Пример файла форматирования в формате, отличном от XML
SQL Server поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования.  Файл форматирования не в формате XML поддерживается более ранними версиями SQL Server.  Дополнительные сведения см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) .  Следующая команда будет использовать [служебную программу bcp](../../tools/bcp-utility.md) для создания файла форматирования `myNative.fmt`в формате, отличном от XML, на основе схемы `myNative`.  Чтобы создать файл форматирования с помощью служебной программы [bcp](../../tools/bcp-utility.md) , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** .  Параметр format также требует наличия параметра **-f** .  Кроме того, в этом примере квалификатор **c** используется для указания символьных данных, а **T** используется для указания доверенного подключения, в рамках которого применяется встроенная система безопасности.  В командной строке введите следующие команды:

```cmd
bcp TestDatabase.dbo.myNative format nul -f D:\BCP\myNative.fmt -T 

REM Review file
Notepad D:\BCP\myNative.fmt
```

> [!IMPORTANT]
> Убедитесь, что файл форматирования не в формате XML заканчивается символом перевода строки или возврата каретки.  В противном случае, скорее всего, появится следующее сообщение об ошибке:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Примеры
В приведенных ниже примерах используется база данных и файлы форматирования, созданные ранее.

### <a name="using-bcp-and-native-format-to-export-data"></a>Использование bcp и собственного формата для экспорта данных
Параметр **-n** и команда **OUT** .  Примечание. Файл данных, созданный в этом примере, будет использоваться во всех последующих примерах.  В командной строке введите следующие команды:

```cmd
bcp TestDatabase.dbo.myNative OUT D:\BCP\myNative.bcp -T -n

REM Review results
NOTEPAD D:\BCP\myNative.bcp
```

### <a name="using-bcp-and-native-format-to-import-data-without-a-format-file"></a>Использование bcp и собственного формата для импорта данных без файла форматирования
Параметр **-n** и команда **IN** .  В командной строке введите следующие команды:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -T -n

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bcp-and-native-format-to-import-data-with-a-non-xml-format-file"></a>Использование bcp и собственного формата для импорта данных с файлом форматирования в формате, отличном от XML
Параметры **-n** и **-f** switches и **IN** commи.  В командной строке введите следующие команды:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -f D:\BCP\myNative.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bulk-insert-and-native-format-without-a-format-file"></a>Использование инструкции BULK INSERT и собственного формата без файла форматирования
Аргумент **DATAFILETYPE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
    FROM 'D:\BCP\myNative.bcp'
    WITH (
        DATAFILETYPE = 'native'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-bulk-insert-and-native-format-with-a-non-xml-format-file"></a>Использование инструкции BULK INSERT и собственного формата с файлом форматирования в формате, отличном от XML
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
   FROM 'D:\BCP\myNative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-openrowset-and-native-format-with-a-non-xml-format-file"></a>Использование инструкции OPENROWSET и собственного формата с файлом форматирования в формате, отличном от XML
Аргумент **FORMATFILE** .  Выполните следующий запрос Transact-SQL в Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative;  -- for testing
INSERT INTO TestDatabase.dbo.myNative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNative.bcp', 
        FORMATFILE = 'D:\BCP\myNative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```
  
## <a name="related-tasks"></a>Связанные задачи
Использование форматов данных для массового импорта или экспорта 
  
-   [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование символьного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>См. также раздел  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [Импорт данных в собственном и символьном формате из предыдущих версий SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Использование собственного формата Юникод для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
  

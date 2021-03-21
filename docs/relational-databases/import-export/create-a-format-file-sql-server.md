---
title: Создание файла форматирования (SQL Server) | Документация Майкрософт
description: При массовом импорте или экспорте таблицы SQL Server файл форматирования позволяет записывать файлы данных с небольшим редактированием или считывать файлы данных из других программ.
ms.custom: ''
ms.date: 02/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], creating
ms.assetid: f680b4a0-630f-4052-9c79-d348c1076f7b
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a097686535a2e8fae1e5a26d8bcea69b91d31d0c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749694"
---
# <a name="create-a-format-file-sql-server"></a>Создание файла форматирования (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  При массовом импорте в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или массовом экспорте данных из таблицы вы можете воспользоваться файлом форматирования, который обеспечивает гибкую структуру записи файлов данных и практически не требует изменения для поддержки соответствия с другими форматами данных или для считывания данных из других программ.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает два типа файлов форматирования: файлы форматирования в формате, отличном от XML, и XML-файлы форматирования. Файл форматирования не в формате XML поддерживается более ранними версиями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Как правило, XML-файлы и файлы форматирования в формате, отличном от XML взаимозаменяемы. Однако рекомендуется пользоваться XML-синтаксисом новых файлов форматирования, так как он обеспечивает ряд преимуществ перед файлами форматирования в формате, отличном от XML.  
  
> [!NOTE]  
>  Версия служебной программы **bcp** (Bcp.exe), которая используется для чтения файла форматирования, должна соответствовать версии, используемой при создании этого файла форматирования, или более поздней версии. Например, служебная программа [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] **bcp** может считать файл форматирования версии 10.0, созданный служебной программой [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] **bcp**, но служебная программа [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] **bcp** не может считать файл форматирования версии 11.0, созданный служебной программой [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] **bcp**.  
  
 В этом разделе описано применение программы [bcp](../../tools/bcp-utility.md) для создания файла форматирования для таблицы. Файл форматирования основан на заданных параметрах типов данных ( **-n**, **-c**, **-w** или **-N**) и разделителях таблиц или представлений.  
  
## <a name="creating-a-non-xml-format-file"></a>Создание файла форматирования в формате, отличном от XML  
 Чтобы создать файл форматирования с помощью служебной программы **bcp** , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** . Параметр **format** также требует наличия параметра **-f** , например:  
  
 **bcp** _таблица_или_представление_ **format** nul **-f**_имя_файла_формата_  
  
> [!NOTE]  
> Для файлов форматирования в формате, отличном от XML рекомендуется использовать расширение FMT, например MyTable.fmt.  
  
 Сведения о структуре и полях файлов форматирования в формате, отличном от XML, см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
### <a name="examples"></a>Примеры  
 В этом разделе приведены следующие примеры, которые показывают, как пользоваться командами **bcp** для создания файлов форматирования в формате, отличном от XML:  
  
-   A. Создание файла форматирования в формате, отличном от XML для данных в собственном формате  
  
-   Б. Создание файла форматирования в формате, отличном от XML для символьных данных  
  
-   В. Создание файла форматирования в формате, отличном от XML для данных в собственном формате в кодировке Юникод  
  
-   Г. Создание файла форматирования в формате, отличном от XML для символьных данных в кодировке Юникод  
  
-   Е. Использование файла форматирования с параметром кодовой страницы  
  
 В этом примере используется таблица `HumanResources.Department` в образце базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Таблица `HumanResources.Department` содержит четыре столбца: `DepartmentID`, `Name`, `GroupName`и `ModifiedDate`.  
  
#### <a name="a-creating-a-non-xml-format-file-for-native-data"></a>A. Создание файла форматирования в формате, отличном от XML для данных в собственном формате  
 В следующем примере создается XML-файл форматирования `Department-n.xml`для таблицы [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . в котором используются собственные типы данных. Содержимое созданного файла форматирования приведено после команды.  
  
 Команда **bcp** содержит следующие квалификаторы:  
  
|Квалификаторы|Описание|  
|----------------|-----------------|  
|**formatnul-f** _формат_файла_|Задает файл форматирования в формате, отличном от XML.|  
|**-n**|Указывает собственные типы данных.|  
|**-T**|Указывает, что программа **bcp** устанавливает доверительное соединение с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием встроенной безопасности. Если параметр **-T** не указан, для входа необходимо указать параметры **-U** и **-P** .|  
  
 В командной строке Windows введите следующую команду `bcp` :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -n -f Department-n.fmt  
```  
  
 Созданный файл форматирования `Department-n.fmt`содержит следующие данные:  
  
```  
12.0  
4  
1  SQLSMALLINT   0       2       ""   1     DepartmentID         ""  
2  SQLNCHAR      2       100     ""   2     Name                 SQL_Latin1_General_CP1_CI_AS  
3  SQLNCHAR      2       100     ""   3     GroupName            SQL_Latin1_General_CP1_CI_AS  
4  SQLDATETIME   0       8       ""   4     ModifiedDate         ""  
```  
  
 Дополнительные сведения см в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
#### <a name="b-creating-a-non-xml-format-file-for-character-data"></a>Б. Создание файла форматирования в формате, отличном от XML для символьных данных  
 В следующем примере создается XML-файл форматирования `Department.fmt`для таблицы [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Файл форматирования использует формат символьных данных и признак конца поля, отличный от установленного по умолчанию (`,`). Содержимое созданного файла форматирования приведено после команды.  
  
 Команда **bcp** содержит следующие квалификаторы:  
  
|Квалификаторы|Описание|  
|----------------|-----------------|  
|**formatnul-f** _формат_файла_|Задает файл форматирования в формате, отличном от XML.|  
|**-c**|Задает символьные данные.|  
|**-T**|Указывает, что программа **bcp** устанавливает доверительное соединение с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием встроенной безопасности. Если параметр **-T** не указан, для входа необходимо указать параметры **-U** и **-P** .|  
  
 В командной строке Windows введите следующую команду `bcp` :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -f Department-c.fmt -T  
```  
  
 Созданный файл форматирования `Department-c.fmt`содержит следующие данные:  
  
```  
12.0  
4  
1  SQLCHAR       0       7       "\t"     1     DepartmentID            ""  
2  SQLCHAR       0       100     "\t"     2     Name                    SQL_Latin1_General_CP1_CI_AS  
3  SQLCHAR       0       100     "\t"     3     GroupName               SQL_Latin1_General_CP1_CI_AS  
4  SQLCHAR       0       24      "\r\n"   4     ModifiedDate            ""  
```  
  
 Дополнительные сведения см в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
#### <a name="c-creating-a-non-xml-format-file-for-unicode-native-data"></a>В. Создание файла форматирования в формате, отличном от XML для данных в собственном формате в кодировке Юникод  
 Чтобы создать для таблицы `HumanResources.Department` файл форматирования в формате, отличном от XML для данных в собственном формате с кодировкой Юникод, используется следующая команда:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -N -f Department-n.fmt  
```  
  
 Дополнительные сведения об использовании собственного формата данных Юникод см. в статье [Использование собственного формата Юникод для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="d-creating-a-non-xml-format-file-for-unicode-character-data"></a>Г. Создание файла форматирования в формате, отличном от XML для символьных данных в кодировке Юникод  
 Чтобы создать для таблицы `HumanResources.Department` файл форматирования в формате, отличном от XML для символьных данных в кодировке Юникод, использующий признак конца по умолчанию, применяется следующая команда:  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -T -w -f Department-w.fmt  
```  
  
 Дополнительные сведения об использовании символьного формата данных Юникод см. в статье [Использование символьного формата Юникод для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="f-using-a-format-file-with-the-code-page-option"></a>Е. Использование файла форматирования с параметром кодовой страницы  
При создании файла форматирования с помощью команды bcp (то есть с помощью `bcp format`) сведения о параметрах сортировки и кодовой странице будут записаны в файл форматирования.   
В следующем примере файла форматирования для таблицы с 5 столбцами указаны параметры сортировки.  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0          Cyrillic_General_CS_AS  
2  SQLCHAR         0       0       "**\t**"         2     c_1          Cyrillic_General_CS_AS  
3  SQLCHAR         0       3000    "**\t**"         3     c_2          Cyrillic_General_CS_AS  
4  SQLCHAR         0       5       "**\t**"         4     c_3          ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4          ""  
  
```  
  
 Если вы пытаетесь импортировать данные в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью `bcp in -c -C65001 -f format_file` …" или "`BULK INSERT`/`OPENROWSET` ... `FORMATFILE='format_file' CODEPAGE=65001` ...", сведения о параметрах сортировки или кодовой странице имеют приоритет над параметром 65001.  
Таким образом, после создания из файла форматирования необходимо вручную удалить сведения о параметрах сортировки перед началом импорта данных обратно в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
Ниже приведен пример файла форматирования без сведений о параметрах сортировки.  
  
```  
13.0  
5  
1  SQLCHAR         0       0       "**\t**"         1     c_0              ""  
2  SQLCHAR         0       0       "**\t**"         2     c_1              ""  
3  SQLCHAR         0       3000    "**\t**"         3     c_2              ""  
4  SQLCHAR         0       5       "**\t**"         4     c_3              ""  
5  SQLCHAR         0       41      "!!!\r\r\n"      5     c_4              ""  
```  
  
## <a name="creating-an-xml-format-file"></a>Создание XML-файла форматирования  
 Чтобы создать файл форматирования с помощью служебной программы **bcp** , укажите аргумент **format** , а вместо пути файла данных задайте значение **nul** . Параметр **format** всегда требует наличия параметра **-f** , а чтобы создать XML-файл форматирования, необходимо задать параметр **-x** , например:  
  
 **bcp** _таблица_или_представление_ **format nul-f** _имя_файла_формата_ **-x**  
  
> [!NOTE]  
> Для XML-файлов форматирования рекомендуется использовать расширение XML, например MyTable.xml.  
  
 Сведения о структуре и полях файлов форматирования в формате XML, см. в разделе [XML-файлы форматирования (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md).  
  
### <a name="examples"></a>Примеры  
 В этом разделе содержатся следующие примеры использования команд **bcp** для создания XML-файлов форматирования.  
  
-   A. Создание XML-файла форматирования для символьных данных  
-   Б. Создание XML-файла форматирования для данных в собственном формате  
  
 В этом примере используется таблица `HumanResources.Department` в образце базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Таблица `HumanResources.Department` содержит четыре столбца: `DepartmentID`, `Name`, `GroupName`и `ModifiedDate`.  
  
> [!NOTE]  
>  [!INCLUDE[ssSampleDBdesc](../../includes/sssampledbdesc-md.md)]  
  
#### <a name="a-creating-an-xml-format-file-for-character-data"></a>A. Создание XML-файла форматирования для символьных данных  
 В следующем примере создается XML-файл форматирования `Department.xml`для таблицы [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]`HumanResources.Department` . Файл форматирования использует формат символьных данных и признак конца поля, отличный от установленного по умолчанию (`,`). Содержимое созданного файла форматирования приведено после команды.  
  
 Команда **bcp** содержит следующие квалификаторы:  
  
|Квалификаторы|Описание|  
|----------------|-----------------|  
|**formatnul-f** _формат_файла_ **-x**|Задает XML-файл форматирования.|  
|**-c**|Задает символьные данные.|  
|**-t** `,`|Задает запятую ( **,** ) в качестве признака конца поля.<br /><br /> Примечание. Если в файле данных используется признак конца поля по умолчанию (`\t`), то параметр **-t** не нужен.|  
|**-T**|Указывает, что программа **bcp** устанавливает доверительное соединение с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием встроенной безопасности. Если параметр **-T** не указан, для входа необходимо указать параметры **-U** и **-P** .|  
  
 В командной строке Windows введите следующую команду `bcp` :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -c -x -f Department-c.xml -t, -T  
```  
  
 Созданный файл форматирования `Department-c.xml`содержит следующие XML-элементы:  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="7"/>  
  <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="," MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="24"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 Сведения о синтаксисе файлов такого формата см. в статье [Файлы формата XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md). Дополнительные сведения о символьных данных см. в статье [Использование символьного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).  
  
#### <a name="b-creating-an-xml-format-file-for-native-data"></a>Б. Создание XML-файла форматирования для данных в собственном формате  
 В следующем примере создается XML-файл форматирования `Department-n.xml`для таблицы `HumanResources.Department` . в котором используются собственные типы данных. Содержимое созданного файла форматирования приведено после команды.  
  
 Команда **bcp** содержит следующие квалификаторы:  
  
|Квалификаторы|Описание|  
|----------------|-----------------|  
|**formatnul-f** _формат_файла_ **-x**|Задает XML-файл форматирования.|  
|**-n**|Указывает собственные типы данных.|  
|**-T**|Указывает, что программа **bcp** устанавливает доверительное соединение с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием встроенной безопасности. Если параметр **-T** не указан, для входа необходимо указать параметры **-U** и **-P** .|  
  
 В командной строке Windows введите следующую команду `bcp` :  
  
```cmd
bcp AdventureWorks2012.HumanResources.Department format nul -x -f Department-n.xml -n -T  
```  
  
 Созданный файл форматирования `Department-n.xml`содержит следующие XML-элементы:  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativeFixed" LENGTH="2"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  <FIELD ID="4" xsi:type="NativeFixed" LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="DepartmentID" xsi:type="SQLSMALLINT"/>  
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="3" NAME="GroupName" xsi:type="SQLNVARCHAR"/>  
  <COLUMN SOURCE="4" NAME="ModifiedDate" xsi:type="SQLDATETIME"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
 Сведения о синтаксисе файлов такого формата см. в статье [Файлы формата XML (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md). Дополнительные сведения об использовании собственных данных см. в статье [Использование собственного формата для импорта и экспорта данных (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).  
  
## <a name="mapping-data-fields-to-table-columns"></a>Сопоставление полей данных со столбцами таблицы  
 Созданный при помощи служебной программы **bcp** файл форматирования надлежащим образом отображает все столбцы таблицы. Его можно изменить, переставив или исключив некоторые из строк. Это позволяет согласовать файл форматирования с файлом данных, если поля в нем не сопоставлены непосредственно со столбцами таблицы. Дополнительные сведения см. в следующих разделах:  
  
-   [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)   
 [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)   
 [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)   
 [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [XML-файлы форматирования (SQL Server)](../../relational-databases/import-export/xml-format-files-sql-server.md)  
  
  

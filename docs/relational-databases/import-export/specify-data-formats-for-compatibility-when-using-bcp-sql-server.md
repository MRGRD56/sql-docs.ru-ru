---
title: Указание форматов данных для совместимости с помощью программы bcp
description: При операциях массового экспорта в SQL Server с помощью служебной программы bcp форматы данных могут быть несовместимы с ожидаемым макетом. Файл форматирования в формате, отличном от XML, задает форматы данных совместимости.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], compatibility
- bulk importing [SQL Server], compatibility
- compatibility [SQL Server], data formats
- data formats [SQL Server], compatibility
- bcp utility [SQL Server], compatibility
ms.assetid: cd5fc8c8-eab1-4165-9468-384f31e53f0a
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 41f983239a2cf946bb46e04315cff69ba3b2abc2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751024"
---
# <a name="specify-compatibility-data-formats-when-using-bcp-sql-server"></a>Указание форматов данных для совместимости с помощью программы bcp (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  В этой статье описываются атрибуты формата данных, запросы командной строки для конкретных полей и хранение данных по полям в неформатированном файле формата, отличного от XML, в команде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**bcp** . Понимать эти возможности может быть полезно, если производится массовый экспорт данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для массового импорта например, в другую программу базы данных. Стандартные форматы данных в исходной таблице (native, character или Unicode) могут быть несовместимы с форматом данных, ожидаемым другой программой. Если несовместимость существует, когда вы экспортируете данные, необходимо описать формат данных.  
  
> [!NOTE]  
>  Описание форматов данных для импорта или экспорта см. в разделе [Форматы данных для массового экспорта или импорта (SQL Server)](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md).  
  
  
##  <a name="bcp-data-format-attributes"></a><a name="bcpDataFormatAttr"></a> Атрибуты формата данных в инструкции bcp  
 Команда **bcp** позволяет указать структуру каждого поля в файле данных в виде указанных ниже атрибутов формата данных.  
  
-   Тип файла хранилища  
  
     *Тип файла хранилища* описывает, каким образом данные хранятся в файле данных. Данные можно экспортировать в файл данных в формате таблиц баз данных (собственный формат), в символьном представлении (символьный формат) или в любом формате данных, поддерживающем неявное преобразование, например копирование данных типа **smallint** как **int**. Пользовательские типы данных экспортируются так же, как их базовые типы. Дополнительные сведения см. в разделе [Указание типа файлового хранилища с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).  
  
-   Длина префикса  
  
     Для наиболее компактного хранения файлов при массовом экспорте данных собственного формата в файл данных команда **bcp** ставит перед каждым полем один или несколько знаков, которые указывают длину этого поля. Эти символы называются *символами префикса длины*. Дополнительные сведения см. в разделе [Определение длины префикса в файлах данных с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md).  
  
-   Длина поля  
  
     Длина поля указывает максимальное количество символов, необходимых для представления данных в символьном формате. Если данные хранятся в собственном формате, то длина поля уже известна. Дополнительные сведения см. в разделе [Указание длины поля с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md).  
  
-   Признак конца поля  
  
     Для символьных полей данных можно определить символы, которые являются разделителями полей и строк в файле данных, указав *признак конца поля* и *признак конца строки*. Символы признака конца представляют собой один из способов сообщения программе, считывающей файл данных, где заканчивается одно поле или строка и начинается другое. Дополнительные сведения см. в разделе [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
  
##  <a name="overview-of-the-field-specific-prompts"></a><a name="FieldSpecificPrompts"></a> Обзор приглашений, относящихся к полям  
 Если интерактивная команда **bcp** содержит параметр **in** или **out** , но не содержит одновременно параметр формата файла ( **-f**) или параметр формата данных ( **-n**, **-c**, **-w** или **-N**), для каждого столбца в исходной или целевой таблице команда предлагает по очереди ввести указанные атрибуты. В каждом приглашении команда **bcp** предлагает значение по умолчанию, основанное на типе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбца таблицы. Принятие значений по умолчанию для всех приглашений эквивалентно указанию собственного формата ( **-n**) в командной строке. В каждом приглашении значение по умолчанию выводится в квадратных скобках: [*значение по умолчанию*]. Чтобы согласиться с указанным значением по умолчанию, нажмите клавишу ВВОД. Чтобы указать другое значение, введите его в приглашении.  
  
### <a name="example"></a>Пример  
 В следующем примере команда **bcp** используется для массового интерактивного экспорта данных из таблицы `HumanResources.myTeam` в файл `myTeam.txt` . Перед выполнением примера следует создать эту таблицу. Дополнительные сведения о таблице и способе ее создания см. в разделе [Образец таблицы HumanResources.myTeam (SQL Server)](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md).  
  
 Команда не указывает ни файл форматирования, ни тип данных, поэтому программа **bcp** запрашивает сведения о формате данных. В командной строке [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows введите:  
  
```cmd
bcp AdventureWorks.HumanResources.myTeam out myTeam.txt -T  
```  
  
 Для каждого столбца программа bcp запрашивает значения, относящиеся к полю. В следующем примере показаны относящиеся к полям приглашения для столбцов таблицы `EmployeeID` и `Name` , и для каждого столбца предлагается тип хранения файла по умолчанию (собственный формат). Длины префиксов столбцов `EmployeeID` и `Name` равны 0 и 2 соответственно. Пользователь указывает запятую (`,`) в качестве признака конца поля.  
  
 `Enter the file storage type of field EmployeeID [smallint]:`  
  
 `Enter prefix-length of field EmployeeID [0]:`  
  
 `Enter field terminator [none]:,`  
  
 `Enter the file storage type of field Name [nvarchar]:`  
  
 `Enter prefix length of field Name [2]:`  
  
 `Enter field terminator [none]:,`  
  
 `.`  
  
 `.`  
  
 `.`  
  
 Аналогичные приглашения (при необходимости) последовательно выводятся для каждого столбца таблицы.  
  
  
##  <a name="storing-field-by-field-data-in-a-non-xml-format-file"></a><a name="FieldByFieldNonXmlFF"></a> Хранение данных полей в файле форматирования в формате, отличном от XML  
 После указания всех столбцов таблицы команда **bcp** предлагает сформировать файл форматирования в формате, отличном от XML, в который будут записаны предоставленные сведения о полях данных (см. предыдущий пример). Создать файл форматирования можно как при экспорте данных из этой таблицы, так и при импорте данных схожей структуры в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Файл форматирования можно использовать для массового импорта данных из файла данных в экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или для массового экспорта данных из таблицы, чтобы не указывать формат повторно. Дополнительные сведения см в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 В следующем примере создается файл форматирования в формате, `myFormatFile.fmt`отличном от XML:  
  
 `Do you want to save this format information in a file? [Y/n] y`  
  
 `Host filename: [bcp.fmt]myFormatFile.fmt`  
  
 Имя по умолчанию для файла форматирования — bcp.fmt, но можно указать и другое имя файла.  
  
> [!NOTE]  
>  Для файла данных, использующего только один формат данных для типа файлового хранилища, например символьный или собственный, можно быстро создать файл форматирования без экспорта и импорта данных, воспользовавшись параметром **format** . Этот подход гораздо проще и позволяет создавать как XML-файлы форматирования, так и файлы форматирования в формате, отличном от XML. Дополнительные сведения см. в разделе [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
  
## <a name="related-tasks"></a>Связанные задачи  
  
-   [Указание типа файлового хранилища с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
-   [Определение длины префикса в файлах данных с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
-   [Указание длины поля с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)  
  
-   [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
## <a name="related-content"></a>См. также  
 Нет.  
  
## <a name="see-also"></a>См. также:  
 [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [Форматы данных для массового экспорта или импорта (SQL Server)](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)   
 [bcp Utility](../../tools/bcp-utility.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
  
  

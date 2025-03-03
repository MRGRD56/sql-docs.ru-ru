---
title: Определение признаков конца поля и строки (SQL Server) | Документация Майкрософт
description: Признаки конца полей и строк позволяют программам, считывающим файлы данных, разделять строки и столбцы.
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], terminators
- field terminators [SQL Server]
- data formats [SQL Server], terminators
- row terminators [SQL Server]
- terminators [SQL Server]
ms.assetid: f68b6782-f386-4947-93c4-e89110800704
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3208393d25df19d980da6b66c78dc3711f428ecd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752284"
---
# <a name="specify-field-and-row-terminators-sql-server"></a>Определение признаков конца поля и строки (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Для символьных полей данных можно определить символы, которые являются разделителями полей и строк в файле данных, указав *признак конца поля* и *признак конца строки*. Для программ, считывающих файлы данных, это единственный способ определить, какими символами в нем разделяются строки и столбцы.  
  
> [!IMPORTANT]
>  При использовании собственного формата или собственного формата в кодировке Юникод вместо признаков конца поля используются префиксы длины. Данные в собственном формате могут конфликтовать со знаками завершения, так как файл данных в собственном формате хранится во внутреннем двоичном формате данных [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="characters-supported-as-terminators"></a>Символы, поддерживаемые в качестве признаков конца полей и строк  
 Команда **bcp** , инструкция BULK INSERT и поставщик больших наборов строк OPENROWSET поддерживают в качестве признака конца поля или строки различные знаки и всегда производят поиск первого вхождения указанного знака завершения. В следующей таблице содержится список поддерживаемых в качестве признаков конца поля или строки символов.  
  
|Символ признака|Обозначается как|  
|---------------------------|------------------|  
|Вкладка|\t<br /><br /> Признак конца поля по умолчанию.|  
|Символ перевода строки|\n<br /><br /> Признак конца строки по умолчанию.|  
|Возврат каретки и перевод строки|\r|  
|Обратная косая черта*|\\\|  
|Знак завершения NULL (невидимый знак завершения)**|\0|  
|Любой печатаемый символ (управляющие символы, за исключением символа NULL, символов табуляции, перевода строки и возврата каретки, являются непечатными)|(*, A, t, l и т. д.)|  
|Строка, которая может содержать до десяти печатных символов, включая некоторые или все указанные выше признаки конца|(**\t\*\*, end, !!!!!!!!!!, \t-\n и т. д.)|  
  
 *Для обозначения управляющего символа с escape-символом обратной косой черты используются только знаки t, n, r, 0 и "\0".  
  
 **Хотя управляющий символ NULL (\0) не виден при печати, он является отдельным знаком в файле данных, то есть если управляющий символ NULL используется в качестве признака конца поля или строки, то это не то же самое, что и полное отсутствие признака конца.  
  
> [!IMPORTANT]  
>  Если символ признака конца поля или строки содержится в данных, он интерпретируется именно как признак, и данные после него интерпретируются как относящиеся к следующему полю или записи данных. Поэтому следует выбирать символы признака конца так, чтобы они никогда не встречались в передаваемых данных. Например, младший символ-заместитель признака конца поля не рекомендуется использовать в качестве признака конца поля, если этот младший символ-заместитель содержится в данных.  
  
## <a name="using-row-terminators"></a>Использование признаков конца строки  
 Признаком конца строки может быть тот же символ, что и символ признака конца последнего поля. Но все же лучше выделить в качестве признака конца строки отдельный символ. Например, для формирования табличного вывода завершайте последнее поле каждой строки символом перевода строки (\n), а конец поля — символом табуляции (\t). Чтобы каждая строка файла данных попала в отдельную строку таблицы, в качестве признака конца строки задайте сочетание \r\n.  
  
> [!NOTE]  
>  Если при использовании **bcp** в интерактивном режиме в качестве признака конца строки задан знак \n (перевод строки), **bcp** автоматически добавляет к ней префикс \r (возврат каретки), в результате чего формируется признак конца строки \r\n.  
  
## <a name="specifying-terminators-for-bulk-export"></a>Задание признаков конца полей и строк при массовом экспорте  
 При массовом экспорте данных типа **char** или **nchar** , если необходимо использовать знак завершения, отличный от назначенного по умолчанию, определите его для команды **bcp** . Это можно сделать одним из следующих способов:  
  
-   При помощи файла форматирования, задающего признаки конца каждого поля отдельно.  
  
    > [!NOTE]  
    >  Дополнительные сведения об использовании файлов форматирования см. в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server )](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
-   Существуют также следующие возможности без использования файла форматирования:  
  
    -   Указать параметр **-t** , задающий признак конца поля для всех полей, за исключением последнего поля в строке, и параметр **-r** , задающий признак конца строки.  
  
    -   Указать параметр символьного формата ( **-c** или **-w**) без параметра **-t** , который задает в качестве признака конца поля знак табуляции \t. Это аналогично указанию **-t**\t.  
  
        > [!NOTE]  
        >  Если указан параметр **-n** (данные в собственном формате) или **-N** (данные в собственном формате в кодировке Юникод), то признаки конца полей и строк не вставляются.  
  
    -   Если интерактивная команда **bcp** содержит параметр **in** или **out** без указания параметра файла форматирования ( **-f**) или параметра формата данных ( **-n**, **-c**, **-w** или **-N**) и если предписано не указывать длину префикса и поля, команда предлагает пользователю указать признак конца каждого поля, причем по умолчанию признак конца отсутствует.  
  
         `Enter field terminator [none]:`  
  
         Обычно приемлемо значение по умолчанию. Однако для полей данных типа **char** или **nchar** существуют особенности, которые рассмотрены в следующем подразделе «Правила использования признаков конца полей и строк». Пример с описанием данного запроса см. в разделе [Указание форматов данных для совместимости с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
        > [!NOTE]  
        >  После интерактивного заполнения всех полей в команде **bcp** появится запрос на сохранение введенных ответов для каждого поля в файле форматирования в формате, отличном от XML. Дополнительные сведения о файлах форматирования в формате, отличном от XML, см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
### <a name="guidelines-for-using-terminators"></a>Рекомендации по использованию признаков конца полей и строк  
 В некоторых ситуациях для данных типа **char** или **nchar** имеет смысл использовать признаки конца поля, Пример:  
  
-   Для столбца данных, содержащего значение NULL в файле данных, который будет импортирован в программу, не распознающую сведения о длине префикса.  
  
     Любой столбец данных, содержащий значение NULL, считается столбцом переменной длины. В отсутствие сведений о длине префикса признак конца необходим, чтобы указать конец поля NULL и удостовериться, что данные интерпретируются верно.  
  
-   Длинный столбец фиксированной длины, пространство которого лишь частично используется многими строками.  
  
     В этой ситуации задание признака конца может минимизировать занимаемый данными объем, что позволит обрабатывать поле как поле переменной длины.  

### <a name="specifying-n-as-a-row-terminator-for-bulk-export"></a>Определение `\n` в качестве символа конца строки при массовом экспорте

Когда вы определяете `\n` как символ конца строки при массовом экспорте или неявно используете такой символ по умолчанию, bcp выводит комбинацию символов возврата каретки и перевода строки как символ конца строки. Чтобы получить только символ перевода строки как символ конца строки (как это обычно происходит на компьютерах Unix и Linux), используйте шестнадцатеричную нотацию. Пример:

```cmd
bcp -r '0x0A'
```

### <a name="examples"></a>Примеры  
 В этом примере выполняется массовый экспорт данных из таблицы `AdventureWorks.HumanResources.Department` в файл данных `Department-c-t.txt` в символьном формате с запятой в качестве признака конца поля и знаком новой строки (\n) в качестве признака конца строки.  
  
 Команда **bcp** поддерживает следующие параметры.  
  
|Параметр|Описание|  
|------------|-----------------|  
|**-c**|Указывает, что поля данных должны загружаться как символьные данные.|  
|**-t** `,`|Задает запятую (,) в качестве признака конца поля.|  
|**-r** \n|Задает в качестве признака конца строки символ перевода строки. Это признак конца строки по умолчанию, поэтому его задание необязательно.|  
|**-T**|Указывает, что программа **bcp** устанавливает доверительное соединение с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием встроенной безопасности. Если параметр **-T** не указан, для входа необходимо указать **-U** и **-P** .|  
  
 Дополнительные сведения см. в разделе [bcp Utility](../../tools/bcp-utility.md).  
  
 В командной строке [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows введите:  
  
```cmd
bcp AdventureWorks.HumanResources.Department out C:\myDepartment-c-t.txt -c -t, -r \n -T  
```  
  
 В результате этого будет создан файл `Department-c-t.txt`, содержащий 16 записей, каждая из которых имеет четыре поля. Поля разделены с помощью символа запятой.  
  
## <a name="specifying-terminators-for-bulk-import"></a>Задание признаков конца для массового импорта  
 При массовом импорте данных типа **char** или **nchar** команда должна распознавать признаки конца полей и строк, используемые в файле данных. В зависимости от команды массового импорта признаки конца полей и строк могут задаваться следующим образом:  
  
-   **bcp**  
  
     При определении признака конца полей и строк в операциях импорта используется тот же самый синтаксис, что и для операций экспорта. Дополнительные сведения см. в подразделе «Задание признаков конца полей и строк при массовом экспорте» ранее в этом разделе.  
  
-   BULK INSERT  
  
     Признаки конца в файле форматирования могут быть определены как для отдельных полей, так и для всего файла данных при помощи квалификаторов, приведенных в следующей таблице.  
  
    |Квалификатор|Описание|  
    |---------------|-----------------|  
    |FIELDTERMINATOR  **='** _field_terminator_*_'_* (признак_конца_поля)|Задает признак конца поля, используемый для символьных файлов данных и файлов в кодировке Юникод.<br /><br /> Значением по умолчанию является \t (символ табуляции).|  
    |ROWTERMINATOR  **='** _row_terminator_*_'_* (признак_конца_строки)|Задает признак конца строки, используемый для символьных файлов данных и файлов в кодировке Юникод.<br /><br /> Значением по умолчанию является \n (символ перевода строки).|  
  
     Дополнительные сведения см. в разделе [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
-   Инструкции INSERT ... SELECT * FROM OPENROWSET(BULK...).  
  
     Для поставщика массовых наборов строк OPENROWSET признаки конца могут задаваться только в файле форматирования (который обязателен для всех типов данных, кроме типа данных больших объектов). Если в файле символьных данных используется признак, отличный от установленного по умолчанию, он должен быть определен в файле форматирования. Дополнительные сведения см. в разделах [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md) и [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).  
  
     Дополнительные сведения о предложении OPENROWSET BULK см. в разделе [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md).  

### <a name="specifying-n-as-a-row-terminator-for-bulk-import"></a>Определение `\n` в качестве символа конца строки при массовом импорте
Когда вы определяете `\n` как символ конца строки при массовом импорте или неявно используете такой символ по умолчанию, bcp и инструкция BULK INSERT ожидают комбинацию символов возврата каретки и перевода строки как символ конца строки. Если в исходном файле используется только символ перевода строки как символ конца строки (как это обычно происходит в файлах, созданных на компьютерах Unix и Linux), используйте шестнадцатеричную нотацию. Например, в инструкции BULK INSERT:

```sql
    ROWTERMINATOR = '0x0A'
```
 
### <a name="examples"></a>Примеры  
 В примерах, содержащихся в этом разделе, рассматривается массовый импорт символьных данных из файла данных `Department-c-t.txt` , созданного в предыдущем примере, в таблицу `myDepartment` образца базы данных [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . Перед выполнением примеров следует создать эту таблицу. Для этого в схеме **dbo** в редакторе запросов среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] выполните следующий код:  
  
```sql  
USE AdventureWorks;  
GO  
DROP TABLE myDepartment;  
CREATE TABLE myDepartment   
(DepartmentID smallint,  
Name nvarchar(50),  
GroupName nvarchar(50) NULL,  
ModifiedDate datetime not NULL CONSTRAINT DF_AddressType_ModifiedDate DEFAULT (GETDATE())  
);  
GO 
```  
  
#### <a name="a-using-bcp-to-interactively-specify-terminators"></a>A. Задание признаков конца полей и строк в интерактивном режиме с помощью bcp  
 В следующем примере выполняется массовый импорт файла данных `Department-c-t.txt` при помощи команды `bcp` . Эта команда использует те же самые параметры, что и команда массового экспорта. Дополнительные сведения см. в подразделе «Задание признаков конца полей и строк при массовом экспорте» ранее в этом разделе.  
  
 В командной строке Windows введите:  
  
```cmd
bcp AdventureWorks..myDepartment in C:\myDepartment-c-t.txt -c -t , -r \n -T  
```  
  
#### <a name="b-using-bulk-insert-to-interactively-specify-terminators"></a>Б. Задание признаков конца в интерактивном режиме с помощью инструкции BULK INSERT  
 В следующем примере производится массовый импорт файла данных `Department-c-t.txt` инструкцией `BULK INSERT` , которая использует квалификаторы, показанные в следующей таблице.  
  
|Параметр|attribute|  
|------------|---------------|  
|DATAFILETYPE **='** char **'**|Указывает, что поля данных должны загружаться как символьные данные.|  
|FIELDTERMINATOR **='** `,` **'**|Задает запятую (`,`) в качестве признака конца поля.|  
|ROWTERMINATOR **='** `\n` **'**|Задает в качестве признака конца строки символ перевода строки.|  
  
 В редакторе запросов в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] выполните следующий код:  
  
```sql  
USE AdventureWorks;  
GO  
BULK INSERT myDepartment FROM 'C:\myDepartment-c-t.txt'  
   WITH (  
      DATAFILETYPE = 'char',  
      FIELDTERMINATOR = ',',  
      ROWTERMINATOR = '\n'  
);  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [Указание длины поля с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [Определение длины префикса в файлах данных с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)   
 [Указание типа файлового хранилища с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
  

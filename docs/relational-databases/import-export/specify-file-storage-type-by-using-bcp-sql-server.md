---
title: Указание типа файлового хранилища с помощью программы bcp
description: Вы можете использовать bcp для экспорта данных в файл в виде типа таблицы базы данных, в символьном представлении или в виде типа данных, поддерживающего неявное преобразование.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], file storage types
- importing data, file storage types
- native data format [SQL Server]
- file storage types [SQL Server]
- data formats [SQL Server], file storage types
ms.assetid: 85e12df8-1be7-4bdc-aea9-05aade085c06
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: ffcde703125ecc798177461343b98da6077c534f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752274"
---
# <a name="specify-file-storage-type-using-bcp-sql-server"></a>Указание типа файлового хранилища с помощью программы bcp (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  *Тип файла хранилища* описывает, каким образом данные хранятся в файле данных. Данные можно экспортировать в файл данных в формате таблиц баз данных (собственный формат), в символьном представлении (символьный формат) или в любом формате данных, поддерживающем неявное преобразование, например копирование данных типа **smallint** как **int**. Пользовательские типы данных экспортируются так же, как их базовые типы.  
  
## <a name="the-bcp-prompt-for-file-storage-type"></a>Приглашение bcp указать тип файлового хранилища  
 Если интерактивная команда **bcp** содержит параметр **in** или **out** без параметра файла форматирования ( **-f**) или параметра формата данных ( **-n**, **-c**, **-w** или **-N**), команда запрашивает тип файлового хранилища для каждого поля данных следующим образом:  
  
 `Enter the file storage type of field <field_name> [<default>]:`  
  
 Ответ на такое приглашение зависит от выполняемой задачи.  
  
-   Для массового экспорта данных из экземпляра [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в файл данных в наиболее компактном хранилище (собственный формат данных) примите типы файловых хранилищ по умолчанию, которые предлагает программа **bcp**. Список собственных типов файловых хранилищ см. в разделе «Собственные типы файловых хранилищ» далее в этом подразделе.  
  
-   Для массового экспорта данных из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в файл данных в символьном формате укажите в качестве типа файлового хранилища **char** для всех столбцов таблицы.  
  
-   Для массового импорта данных в экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из файла данных укажите тип файлового хранилища **char** для типов, для которых данные хранятся в символьном формате, а для данных, хранящихся в формате собственного типа данных, укажите один из типов файлового хранилища.  
  
    |Тип файла хранилища|Введите в командной строке|  
    |-----------------------|-----------------------------|  
    |**char**|**c**[**har**]|  
    |**varchar**|**c[har]**|  
    |**nchar**|**w**|  
    |**nvarchar**|**w**|  
    |**text**|**T**[**ext**]|  
    |**ntext2**|**W**|  
    |**binary**|**x**|  
    |**varbinary**|**x**|  
    |**image**|**I**[**mage**]|  
    |**datetime**|**d[ate]**|  
    |**smalldatetime**|**D**|  
    |**time**|**te**|  
    |**date**|**de**|  
    |**datetime2**|**d2**|  
    |**datetimeoffset**|**do**|  
    |**decimal**|**n**|  
    |**numeric**|**n**|  
    |**float**|**f[loat]**|  
    |**real**|**r**|  
    |**Int**|**i[nt]**|  
    |**bigint**|**B[igint]**|  
    |**smallint**|**s[mallint]**|  
    |**tinyint**|**t[inyint]**|  
    |**money**|**m[oney]**|  
    |**smallmoney**|**M**|  
    |**bit**|**b[it]**|  
    |**uniqueidentifier**|**u**|  
    |**sql_variant**|**V[ariant]**|  
    |**timestamp**|**x**|  
    |**UDT** (определяемый пользователем тип данных)|**U**|  
    |**XML**|**X**|  
  
     \*Взаимосвязь длины поля, длины префикса и признаков конца определяет объем хранилища, выделяемый в файле данных для несимвольных данных, экспортируемых в качестве типа файлового хранилища **char** .  
  
     \*\* Типы данных **ntext**, **text** и **image** будут исключены в следующей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Следует избегать использования этих типов данных при новой разработке и запланировать изменение приложений, использующих их в настоящий момент. Вместо них следует использовать типы данных **nvarchar(max)** , **varchar(max)** и **varbinary(max)** .  
  
## <a name="native-file-storage-types"></a>Собственные типы файловых хранилищ  
 Все собственные типы файловых хранилищ записаны в файле форматирования как соответствующие типы данных основных файлов.  
  
|Тип файла хранилища|Тип данных файла|  
|-----------------------|-------------------------|  
|**char**|SQLCHAR|  
|**varchar**|SQLCHAR|  
|**nchar**|SQLNCHAR|  
|**nvarchar**|SQLNCHAR|  
|**text**|SQLCHAR|  
|**ntext**|SQLNCHAR|  
|**binary**|SQLBINARY|  
|**varbinary**|SQLBINARY|  
|**image**|SQLBINARY|  
|**datetime**|SQLDATETIME|  
|**smalldatetime**|SQLDATETIM4|  
|**decimal**|SQLDECIMAL|  
|**numeric**|SQLNUMERIC|  
|**float**|SQLFLT8|  
|**real**|SQLFLT4|  
|**int**|SQLINT|  
|**bigint**|SQLBIGINT|  
|**smallint**|SQLSMALLINT|  
|**tinyint**|SQLTINYINT|  
|**money**|SQLMONEY|  
|**smallmoney**|SQLMONEY4|  
|**bit**|SQLBIT|  
|**uniqueidentifier**|SQLUNIQUEID|  
|**sql_variant**|SQLVARIANT|  
|**timestamp**|SQLBINARY|  
|UDT (определяемый пользователем тип данных)|SQLUDT|  
  
 \*Для файлов данных, хранящихся в символьном формате, в качестве типа файлового хранилища используется **char** . Таким образом, для символьных файлов данных SQLCHAR — это единственный тип данных, допустимый в файле форматирования.  
  
 \*\*Невозможно массово импортировать данные в столбцы **text**, **ntext** и **image** , если они содержат значения DEFAULT.  
  
## <a name="additional-considerations-for-file-storage-types"></a>Дополнительные замечания относительно типов файловых хранилищ  
 При массовом экспорте данных из экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в файл данных:  
  
-   можно указать в качестве типа файлового хранилища **char** ;  
  
-   если указан тип файлового хранилища, представляющий недопустимое неявное преобразование, то работа **bcp** завершится ошибкой. Например, хотя можно указать **int** для данных **smallint** , если задать **smallint** для данных **int** , итогом станет ошибка переполнения;  
  
-   когда несимвольные типы данных, такие как **float**, **money**, **datetime** или **int** , хранятся с использованием соответствующих типов баз данных, то данные записываются в файл данных в собственном формате [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  После интерактивного заполнения всех полей в команде **bcp** появится запрос на сохранение введенных ответов для каждого поля в файле форматирования в формате, отличном от XML. Дополнительные сведения о файлах форматирования в формате, отличном от XML, см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Указание длины поля с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)   
 [Определение длины префикса в файлах данных с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
  

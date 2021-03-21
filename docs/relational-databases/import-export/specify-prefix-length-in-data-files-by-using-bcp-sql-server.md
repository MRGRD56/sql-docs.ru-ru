---
title: Определение длины префикса в файлах данных с помощью программы bcp
description: Эта статья описывает поле префикса, которое кодирует длину поля для обеспечения компактного хранения файлов при массовом экспорте в собственном формате в файл данных.
ms.date: 07/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server], prefix length
- prefix length [SQL Server]
- lengths [SQL Server], prefix characters
- data formats [SQL Server], prefix length
ms.assetid: ce32dd1a-26f1-4f61-b9fa-3f1feea9992e
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: f57d14262410c3b469cc4576e35f2eb0d6c625f6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745734"
---
# <a name="specify-prefix-length-in-data-files-using-bcp-sql-server"></a>Определение длины префикса в файлах данных с помощью программы bcp (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Для наиболее компактного хранения файлов при массовом экспорте данных собственного формата в файл данных команда **bcp** ставит перед каждым полем один или несколько знаков, которые указывают длину этого поля. Эти символы называются *символами префикса длины*.  
  
## <a name="the-bcp-prompt-for-prefix-length"></a>Запрос длины префикса программой bcp  
 Если интерактивная команда **bcp** содержит параметр **in** или **out** без параметра файла форматирования ( **-f**) или параметра формата данных ( **-n**, **-c**, **-w** или **-N**), то команда запрашивает длину префикса каждого поля данных следующим образом:  
  
 `Enter prefix length of field <field_name> [<default>]:`  
  
 Если указать значение 0, программа **bcp** попросит указать либо длину поля (для символьного типа данных), либо признак конца поля (для собственного несимвольного типа).  
  
> [!NOTE]  
>  После интерактивного заполнения всех полей в команде **bcp** появится запрос на сохранение введенных ответов для каждого поля в файле форматирования в формате, отличном от XML. Дополнительные сведения о файлах форматирования в формате, отличном от XML, см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
## <a name="overview-of-prefix-length"></a>Обзор параметра «Длина префикса»  
 Для сохранения длины префикса поля требуется число байтов, достаточное для представления максимальной длины этого поля. Необходимое число байтов зависит также от типа хранения файла, возможности столбца содержать значения NULL и от способа хранения данных в этом файле — в собственном или символьном формате. Например, типы данных **text** или **image** требуют четырех символов для хранения длины поля, а тип данных **varchar** требует двух символов. В файле данных эти символы префикса длины хранятся во внутреннем двоичном формате [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  При использовании собственного формата предпочтительнее использовать префиксы длины, а не признаки конца поля. Собственный формат данных может конфликтовать с признаками конца, поскольку файл данных в собственном формате хранится во внутреннем двоичном формате данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="prefix-lengths-for-bulk-export"></a><a name="PrefixLengthsExport"></a> Длины префиксов для массового экспорта  
  
> [!NOTE]  
>  Значение по умолчанию, предлагаемое при запросе длины префикса при экспорте поля, означает оптимальную длину префикса для этого поля.  
  
 Значения NULL отображаются в виде пустого поля. Для обозначения того, что поле пустое (значение NULL), префикс этого поля содержит значение -1, то есть для него необходим как минимум 1 байт. Обратите внимание, что если столбец таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] допускает значения NULL, для него необходима длина префикса не менее 1, вне зависимости от типа хранения файла.  
  
 При массовом экспорте данных и сохранении их в виде собственных типов данных или символьном формате используйте следующие значения длины префиксов.  
  
|SQL Server<br /><br /> тип данных|Собственный формат<br /><br /> NOT NULL|Собственный формат<br /><br /> NULL|Символьный формат<br /><br /> NOT NULL|Символьный формат<br /><br /> NULL|  
|------------------------------|--------------------------------|----------------------------|-----------------------------------|-------------------------------|  
|**char**|2|2|2|2|  
|**varchar**|2|2|2|2|  
|**nchar**|2|2|2|2|  
|**nvarchar**|2|2|2|2|  
|**text***|4|4|4|4|  
|**ntext***|4|4|4|4|  
|**binary**|2|2|2|2|  
|**varbinary**|2|2|2|2|  
|**image***|4|4|4|4|  
|**datetime**|0|1|0|1|  
|**smalldatetime**|0|1|0|1|  
|**decimal**|1|1|1|1|  
|**numeric**|1|1|1|1|  
|**float**|0|1|0|1|  
|**real**|0|1|0|1|  
|**int**|0|1|0|1|  
|**bigint**|0|1|0|1|  
|**smallint**|0|1|0|1|  
|**tinyint**|0|1|0|1|  
|**money**|0|1|0|1|  
|**smallmoney**|0|1|0|1|  
|**bit**|0|1|0|1|  
|**uniqueidentifier**|1|1|0|1|  
|**timestamp**|1|1|1|1|  
|**varchar(max)**|8|8|8|8|  
|**varbinary(max)**|8|8|8|8|  
|**UDT** (определяемый пользователем тип данных)|8|8|8|8|  
|**XML**|8|8|8|8|  
|**sql_variant**|8|8|8|8|  
  
 \*Типы данных **ntext**, **text** и **image** будут исключены из следующей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Следует избегать использования этих типов данных при новой разработке и запланировать изменение приложений, использующих их в настоящий момент. Вместо них следует использовать типы данных **nvarchar(max)** , **varchar(max)** и **varbinary(max)** .  
  
##  <a name="prefix-lengths-for-bulk-import"></a><a name="PrefixLengthsImport"></a> Длины префиксов для массового импорта  
 При массовом импорте данных длина префикса — это значение, указанное при первоначальном создании файла данных. Если файл данных не создан командой **bcp** , символы префикса длины, возможно, не существуют. В этом случае в качестве длины префикса нужно указать 0.  
  
> [!NOTE]  
>  Для определения длины префикса в файле данных, созданном без помощи программы **bcp**, используйте значения длины, указанные выше в разделе [Длины префиксов для массового экспорта](#PrefixLengthsExport).  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Указание длины поля с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)   
 [Определение признаков конца поля и строки (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)   
 [Указание типа файлового хранилища с помощью программы bcp (SQL Server)](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
  

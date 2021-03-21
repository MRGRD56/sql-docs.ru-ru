---
title: XML-файлы форматирования (SQL Server) | Документация Майкрософт
description: SQL Server 2019 предоставляет схему XML, которая определяет синтаксис для написания XML-файлов форматирования в целях использования при массовом импорте данных в таблицу SQL Server.
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], XML format files
- bulk importing [SQL Server], format files
- XML format files [SQL Server]
ms.assetid: 69024aad-eeea-4187-8fea-b49bc2359849
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 432f1c263e132a2883d70792146b56724452ab77
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104736224"
---
# <a name="xml-format-files-sql-server"></a>XML-файлы форматирования (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] предоставляет схему XML, которая определяет синтаксис для написания *XML-файлов форматирования* в целях использования при массовом импорте данных в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . XML-файлы форматирования должны придерживаться этой схемы, которая определена при помощи языка XML Schema Definition Language (XSDL). XML-файлы форматирования поддерживаются только при установке средств [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вместе с собственным клиентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Можно использовать XML-файл форматирования с командой **bcp**, инструкциями BULK INSERT или INSERT... Инструкция SELECT \* FROM OPENROWSET(BULK...). Команда **bcp** позволяет автоматически создать XML-файл форматирования для таблицы. Дополнительные сведения см. в разделе [bcp Utility](../../tools/bcp-utility.md).  
  
> [!NOTE]  
>  Для массового экспорта и импорта поддерживаются два типа файлов форматирования: *файлы форматирования, отличные от XML* , и *XML-файлы форматирования*. Они более гибкие и мощные по сравнению с файлом форматирования в формате, отличном от XML. Сведения о файлах форматирования в формате, отличном от XML, см. в разделе [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
 **В этом разделе:**  
  
-   [Преимущества использования XML-файлов форматирования](#BenefitsOfXmlFFs)  
  
-   [Структура XML-файлов форматирования](#StructureOfXmlFFs)  
  
-   [Синтаксис схемы для XML-файлов форматирования](#SchemaSyntax)  
  
-   [Образцы XML-файлов форматирования](#SampleXmlFFs)  
  
-   [Связанные задачи](#RelatedTasks)  
  
-   [См. также](#RelatedContent)  
  
##  <a name="benefits-of-xml-format-files"></a><a name="BenefitsOfXmlFFs"></a> Преимущества использования XML-файлов форматирования  
  
-   XML-файлы форматирования описывают сами себя, благодаря чему упрощается их чтение, создание и расширение. Они доступны для чтения человеком, что позволяет легко понять, как интерпретируются данные во время массовых операций.  
  
-   XML-файлы форматирования содержат типы данных целевых столбцов.  Запись XML четко описывает типы данных и элементы файла данных, а также соответствие элементов данных столбцам таблицы.  
  
     Это позволяет отделить представление данных в файле от типов данных полей. Например, если файл данных содержит данные в символьном представлении, то тип данных SQL соответствующего столбца будет утрачен.  
  
-   XML-файл форматирования позволяет загружать из файлов данных поля, содержащие единственный тип данных LOB.  
  
-   XML-файл форматирования можно улучшить, сохранив совместимость с предыдущими версиями. Кроме того, понятность записи XML облегчает создание нескольких файлов форматирования для некоторого файла данных. Это удобно при сопоставлении всех или некоторых полей данных со столбцами в различных таблицах и представлениях.  
  
-   Синтаксис XML-файла форматирования не зависит от направления операции; для операций массового импорта и массового экспорта синтаксис одинаков.  
  
-   XML-файлы форматирования можно использовать для массового импорта данных в таблицы или несекционированные представления и массового экспорта данных.  
  
-   Для функции OPENROWSET(BULK...) указание целевой таблицы является необязательным. Это обусловлено тем, что эта функция для чтения данных из файла данных использует XML-файл форматирования.  
  
    > [!NOTE]  
    >  Целевая таблица необходима при работе с командой **bcp** и инструкцией BULK INSERT, которая использует столбцы целевой таблицы при преобразовании типов.  
  
##  <a name="structure-of-xml-format-files"></a><a name="StructureOfXmlFFs"></a> Структура XML-файлов форматирования  
 XML-файлы форматирования, как и файл форматирования в формате, отличном от XML, определяют формат и структуру полей данных в файле данных и сопоставляют их со столбцами целевой таблицы.  
  
 XML-файл форматирования содержит два основных элемента, \<RECORD> и \<ROW>.  
  
-   \<RECORD> описывает способ хранения данных в файле данных.  
  
     Каждый элемент \<RECORD> содержит набор из одного элемента \<FIELD> или нескольких. Эти элементы соответствуют полям в файле данных. Базовый синтаксис:  
  
     \<RECORD>  
  
     \<FIELD .../> [ ...*n* ]  
  
     \</RECORD>  
  
     Каждый элемент \<FIELD> описывает содержимое определенного поля данных. Поле может быть сопоставлено только с одним столбцом таблицы. Столбцам не обязательно сопоставлять все поля.  
  
     Поле в файле данных может иметь фиксированную или переменную длину или завершаться определенным символом. *Значение поля* может быть представлено в следующем виде: символ (однобайтовое представление), широкий символ (двухбайтовое представление Юникода), собственный формат базы данных или имя файла. Если значение поля представляется в виде имени файла, оно указывает на файл, который содержит значение столбца BLOB в целевой таблице.  
  
-   \<ROW> описывает, как создавать строки данных из файла данных, который импортируется в таблицу сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     Элемент \<ROW> содержит набор элементов \<COLUMN>. Эти элементы соответствуют столбцам таблицы. Базовый синтаксис:  
  
     \<ROW>  
  
     \<COLUMN .../> [ ...*n* ]  
  
     \</ROW>  
  
     Каждый элемент \<COLUMN> можно сопоставить только с одним полем в файле данных. Порядок элементов \<COLUMN> в элементе \<ROW> задает порядок, в котором они будут возвращены массовой операцией. XML-файл форматирования назначает каждому элементу \<COLUMN> локальное имя, не имеющее отношения к столбцу целевой таблицы операции массового импорта.  
  
##  <a name="schema-syntax-for-xml-format-files"></a><a name="SchemaSyntax"></a> Синтаксис схемы для XML-файлов форматирования  
 Этот раздел содержит список элементов и атрибутов схемы XML для XML-файлов форматирования. Синтаксис файла форматирования не зависит от направления операции; для операций массового импорта и массового экспорта синтаксис одинаков. В разделе также рассматривается использование элементов \<ROW> и \<COLUMN> массовым импортом и помещение значения xsi:type элемента в набор данных.  
  
 Чтобы узнать, как этот синтаксис соответствует реальным XML-файлам форматирования, см. далее раздел [Образец XML-файлов форматирования](#SampleXmlFFs).  
  
> [!NOTE]  
>  Можно изменить файл форматирования, чтобы обеспечить возможность массового импорта данных из файла данных, в котором количество или порядок полей отличаются от количества или порядка столбцов таблицы. Дополнительные сведения см в разделе [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 **В этом разделе.**  
  
-   [Основной синтаксис схемы XML](#BasicSyntax)  
  
-   [Как массовый импорт использует \<ROW>элемент](#HowUsesROW)  
  
-   [Как массовый импорт использует \<COLUMN>элемент](#HowUsesColumn)  
  
-   [Помещение значения xsi:type в набор данных](#PutXsiTypeValueIntoDataSet)  
  
###  <a name="basic-syntax-of-the-xml-schema"></a><a name="BasicSyntax"></a> Основной синтаксис схемы XML  
 Данные инструкции синтаксиса показывают только элементы (\<BCPFORMAT>, \<RECORD>, \<FIELD>, \<ROW> и \<COLUMN>) и их основные атрибуты.  
  
 \<BCPFORMAT ...>  
  
 \<RECORD>  
  
 \<FIELD ID = "*fieldID*" xsi:type = "*fieldType*" [...]  
  
 />  
  
 \</RECORD>  
  
 \<ROW>  
  
 \<COLUMN SOURCE = "*fieldID*" NAME = "*columnName*" xsi:type = "*columnType*" [...]  
  
 />  
  
 \</ROW>  
  
 \</BCPFORMAT>  
  
> [!NOTE]  
>  Дополнительные атрибуты, связанные со значением типа xsi:type в элементах \<FIELD> или \<COLUMN>, описаны ниже в этом подразделе.  
  
 **В этом разделе.**  
  
-   [Элементы схемы](#SchemaElements)  
  
-   [Атрибуты элемента \<FIELD>](#AttrOfFieldElement) (и значения [Xsi:type элемента \<FIELD>](#XsiTypeValuesOfFIELD))  
  
-   [Атрибуты элемента \<COLUMN>](#AttrOfColumnElement) (и значения [Xsi:type элемента \<COLUMN>](#XsiTypeValuesOfCOLUMN))  
  
####  <a name="schema-elements"></a><a name="SchemaElements"></a> Элементы схемы  
 В этом разделе кратко описаны назначения каждого элемента, определяемого схемой XML для XML-файла форматирования. Атрибуты описаны в отдельных подразделах ниже в этом разделе.  
  
 \<BCPFORMAT>  
 Элемент файла форматирования, который определяет структуру записей данного файла данных и его соответствие столбцам строки таблицы.  
  
 \<RECORD .../>  
 Определяет составной элемент, содержащий один элемент \<FIELD> или несколько. Порядок, в котором поля объявлены в файле форматирования, является порядком, в котором эти поля будут расположены в файле данных.  
  
 \<FIELD .../>  
 Определяет поле в файле данных, которое содержит данные.  
  
 Атрибуты этого элемента описаны в разделе [Атрибуты элемента \<FIELD>](#AttrOfFieldElement) ниже в данной статье.  
  
 \<ROW .../>  
 Определяет составной элемент, содержащий один элемент \<COLUMN> или несколько. Порядок элементов \<COLUMN> не зависит от порядка элементов \<FIELD> в определении RECORD. Скорее, порядок элементов \<COLUMN> в файле форматирования определяет порядок столбцов результирующего набора строк. Поля данных загружаются в порядке, в котором соответствующие элементы \<COLUMN> объявлены в элементе \<COLUMN>.  
  
 Дополнительные сведения см. в разделе [Как массовый импорт использует элемент \<ROW>](#HowUsesROW) ниже в данной статье.  
  
 \<COLUMN>  
 Определяет столбец как элемент (\<COLUMN>). Каждый элемент \<COLUMN> соответствует элементу \<FIELD> (чей идентификатор задан в атрибуте SOURCE элемента \<COLUMN>).  
  
 Атрибуты этого элемента описаны в разделе [Атрибуты элемента \<COLUMN>](#AttrOfColumnElement) ниже в данной статье. См. также раздел [Как массовый импорт использует элемент \<COLUMN>](#HowUsesColumn) ниже в данной статье.  
  
 \</BCPFORMAT>  
 Требуется в конце файла форматирования.  
  
####  <a name="attributes-of-the-field-element"></a><a name="AttrOfFieldElement"></a> Атрибуты элемента \<FIELD>  
 В этом разделе описываются атрибуты элемента \<FIELD>, которые обобщены в следующем синтаксисе схемы.  
  
 Значения xsi:type элемента <FIELD  
  
 ID **="** _fieldID_ **"**  
  
 xsi **:** type **="** _fieldType_ **"**  
  
 [ LENGTH **="** _n_ **"** ]  
  
 [ PREFIX_LENGTH **="** _p_ **"** ]  
  
 [ MAX_LENGTH **="** _m_ **"** ]  
  
 [ COLLATION **="** _collationName_ **"** ]  
  
 [ TERMINATOR **="** _terminator_ **"** ]  
  
 />  
  
 Каждый элемент \<FIELD> независим от других. Поле описывается на основе следующих атрибутов:  
  
|Атрибут FIELD|Описание|Необязательный или<br /><br /> Обязательно|  
|---------------------|-----------------|------------------------------|  
|ID **="** _fieldID_ **"**|Задает логическое имя поля в файле данных. Идентификатор поля является ключом, используемым для обращения к полю.<br /><br /> \<FIELD ID**="**_fieldID_**"**/> соответствует \<COLUMN SOURCE**="**_fieldID_**"**/>|Обязательно|  
|xsi:type **="** _fieldType_ **"**|Это конструкция XML (используется как атрибут), которая указывает тип экземпляра элемента. Значение атрибута *fieldType* определяет, какой из необязательных атрибутов (см. ниже) необходим в данном экземпляре.|Обязательный (в зависимости от типа данных)|  
|LENGTH **="** _n_ **"**|Этот атрибут определяет длину для экземпляра типа данных фиксированной длины.<br /><br /> Значение *n* должно быть положительным целым числом.|Необязательный, когда не требуется значением xsi:type|  
|PREFIX_LENGTH **="** _p_ **"**|Этот атрибут определяет длину префикса для двоичного представления данных. Значение PREFIX_LENGTH, *p*, должно быть равно 1, 2, 4 или 8.|Необязательный, когда не требуется значением xsi:type|  
|MAX_LENGTH **="** _m_ **"**|Этот атрибут является максимальным числом байтов, которые могут храниться в данном поле. Без целевой таблицы максимальная длина столбца неизвестна. Атрибут MAX_LENGTH ограничивает максимальную длину выходного столбца символов, ограничивая хранилище, выделенное для значения столбца. Это особенно удобно при использовании параметра BULK функции OPENROWSET в предложении SELECT FROM.<br /><br /> Значение *m* должно быть положительным целым числом. По умолчанию максимальная длина 8000 символов для столбца типа **char** и 4000 символов для столбца типа **nchar** .|Необязательно|  
|COLLATION **="** _collationName_ **"**|Аргумент COLLATION допустим только для символьных полей. Список имен параметров сортировки SQL см. в разделе [Имя параметров сортировки SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)|Необязательно|  
|TERMINATOR **= "** _terminator_ **"**|Этот атрибут задает признак конца поля данных. Признаком конца может быть любой символ. Признак конца должен быть уникальным символом, который не является частью данных.<br /><br /> По умолчанию признаком конца поля является символ табуляции (представленный как «\t»). Чтобы указать знак абзаца, используйте сочетание символов «\r\n».|Используется только со значением xsi:type символьных данных, которые требуют наличия этого атрибута|  
  
#####  <a name="xsitype-values-of-the-field-element"></a><a name="XsiTypeValuesOfFIELD"></a> Значения Xsi:type элемента \<FIELD>  
 Значение xsi:type — это конструкция XML, используемая как атрибут и определяющая тип данных экземпляра элемента. Сведения по применению этой конструкции см. в пункте «Размещение значения xsi:type в наборе данных» ниже в этом разделе.  
  
 Значение xsi:type элемента \<FIELD> поддерживает следующие типы данных.  
  
|Значения \<FIELD> xsi:type|Обязательные XML-атрибуты<br /><br /> для типа данных|Необязательные XML-атрибуты<br /><br /> для типа данных|  
|-------------------------------|---------------------------------------------------|---------------------------------------------------|  
|**NativeFixed**|**LENGTH**|Нет.|  
|**NativePrefix**|**PREFIX_LENGTH**|MAX_LENGTH|  
|**CharFixed**|**LENGTH**|COLLATION|  
|**NCharFixed**|**LENGTH**|COLLATION|  
|**CharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH, COLLATION|  
|**NCharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH, COLLATION|  
|**CharTerm**|**TERMINATOR**|MAX_LENGTH, COLLATION|  
|**NCharTerm**|**TERMINATOR**|MAX_LENGTH, COLLATION|  
  
 Дополнительные сведения о типах данных [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).  
  
####  <a name="attributes-of-the-column-element"></a><a name="AttrOfColumnElement"></a> Атрибуты элемента \<COLUMN>  
 В этом разделе описываются атрибуты элемента \<COLUMN>, которые обобщены в следующем синтаксисе схемы.  
  
 Типы данных <COLUMN  
  
 SOURCE = "*fieldID*"  
  
 NAME = "*columnName*"  
  
 xsi:type = "*columnType*"  
  
 [ LENGTH = "*n*" ]  
  
 [ PRECISION = "*n*" ]  
  
 [ SCALE = "*value*" ]  
  
 [ NULLABLE = { "YES"  
  
 "NO" } ]  
  
 />  
  
 Поле сопоставлено со столбцом целевой таблицы с использованием следующих атрибутов:  
  
|Атрибут COLUMN|Описание|Необязательный или<br /><br /> Обязательно|  
|----------------------|-----------------|------------------------------|  
|SOURCE **="** _fieldID_ **"**|Задает идентификатор поля, сопоставляемого со столбцом.<br /><br /> \<COLUMN SOURCE**="**_fieldID_**"**/> соответствует \<FIELD ID**="**_fieldID_**"**/>|Обязательно|  
|NAME = "*columnName*"|Задает имя столбца в наборе строк, представленном файлом форматирования. Это имя столбца используется для идентификации столбца в результирующем наборе, и оно не обязательно должно соответствовать имени столбца целевой таблицы.|Обязательно|  
|xsi **:** type **="** _ColumnType_ **"**|Это конструкция XML (используется как атрибут), которая указывает тип данных экземпляра элемента. Значение атрибута *ColumnType* определяет, какой из необязательных атрибутов (см. ниже) необходим в данном экземпляре.<br /><br /> Примечание. Возможные значения *ColumnType* и связанные с ними атрибуты перечислены в таблице элементов \<COLUMN> в разделе [Значения Xsi:typ раздела элемента &lt;COLUMN&gt;](#XsiTypeValuesOfCOLUMN).|Необязательно|  
|LENGTH **="** _n_ **"**|Определяет длину для экземпляра типа данных фиксированной длины. Атрибут LENGTH используется только в том случае, если значение xsi:type является строковым типом данных.<br /><br /> Значение *n* должно быть положительным целым числом.|Необязательный (доступен только в том случае, если значение xsi:type является строковым типом данных)|  
|PRECISION **="** _n_ **"**|Указывает количество цифр в числе. Например, число 123,45 имеет точность 5.<br /><br /> Значение должно быть положительным целым числом.|Необязательный (доступен только в том случае, если значение xsi:type является переменным числовым типом данных)|  
|SCALE **="** _int_ **"**|Указывает количество цифр справа от десятичной запятой в числе. Например, число 123,45 имеет масштаб 2.<br /><br /> Значением должно быть целое число.|Необязательный (доступен только в том случае, если значение xsi:type является переменным числовым типом данных)|  
|NULLABLE **=** { **"** YES **"**<br /><br /> **"** NO **"** }|Указывает, может ли столбец принимать значение NULL. Этот атрибут полностью независим от FIELDS. Однако если столбец не NULLABLE и в поле указано значение NULL (значение не указано), результатом будет ошибка выполнения.<br /><br /> Атрибут NULLABLE используется только при выполнении простых инструкций SELECT FROM OPENROWSET(BULK...).|Необязательный (доступен для любого типа данных)|  
  
#####  <a name="xsitype-values-of-the-column-element"></a><a name="XsiTypeValuesOfCOLUMN"></a> Значения Xsi:type элемента \<COLUMN>  
 Значение xsi:type — это конструкция XML, используемая как атрибут и определяющая тип данных экземпляра элемента. Сведения по применению этой конструкции см. в пункте «Размещение значения xsi:type в наборе данных» ниже в этом разделе.  
  
 Элемент \<COLUMN> поддерживает следующие собственные типы данных SQL.  
  
|Категория типа|\<COLUMN> Типы данных|Обязательные XML-атрибуты<br /><br /> для типа данных|Необязательные XML-атрибуты<br /><br /> для типа данных|  
|-------------------|---------------------------|---------------------------------------------------|---------------------------------------------------|  
|исправление|**SQLBIT**, **SQLTINYINT**, **SQLSMALLINT**, **SQLINT**, **SQLBIGINT**, **SQLFLT4**, **SQLFLT8**, **SQLDATETIME**, **SQLDATETIM4**, **SQLDATETIM8**, **SQLMONEY**, **SQLMONEY4**, **SQLVARIANT** и **SQLUNIQUEID**|Нет.|NULLABLE|  
|Переменное число|**SQLDECIMAL** и **SQLNUMERIC**|Нет.|NULLABLE, PRECISION, SCALE|  
|Большой объект (LOB)|**SQLIMAGE**, **CharLOB**, **SQLTEXT** и **SQLUDT**|Нет.|NULLABLE|  
|Большой символьный объект (CLOB)|**SQLNTEXT**|Нет.|NULLABLE|  
|Двоичная строка|**SQLBINARY** и **SQLVARYBIN**|Нет.|NULLABLE, LENGTH|  
|Строка символов|**SQLCHAR**, **SQLVARYCHAR**, **SQLNCHAR** и **SQLNVARCHAR**|Нет.|NULLABLE, LENGTH|  
  
> [!IMPORTANT]  
>  Для массового экспорта или импорта данных SQLXML используется один из следующих типов данных в файле форматирования. SQLCHAR или SQLVARYCHAR (данные посылаются в кодовой странице клиента или в кодовой странице, предполагаемой параметрами сортировки), SQLNCHAR или SQLNVARCHAR (данные посылаются в формате Юникод) и SQLBINARY или SQLVARYBIN (данные посылаются без преобразования).  
  
 Дополнительные сведения о типах значений [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).  
  
###  <a name="how-bulk-import-uses-the-row-element"></a><a name="HowUsesROW"></a> Как массовый импорт использует элемент \<ROW>  
 Элемент \<ROW> не учитывается в некоторых контекстах. Влияние элемента \<ROW> на операцию массового импорта зависит от того, как выполняется операция.  
  
-   команда **bcp**  
  
     При загрузке данных в целевую таблицу команда **bcp** не учитывает компонент \<ROW>. Вместо этого команда **bcp** загружает данные с учетом типов столбцов целевой таблицы.  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции (команда BULK INSERT и поставщик массового набора строк OPENROWSET)  
  
     При мас=совом импорте данных в таблицу инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] используют компонент \<ROW> для формирования входного набора строк. Инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] также выполняют соответствующие преобразования типов, основываясь на типах столбцов, указанных для элемента \<ROW> и соответствующего столбца в целевой таблице. При несовпадении типов столбцов в файле форматирования и в целевой таблице производится дополнительное преобразование типов, Это дополнительное преобразование типа может привести к некоторым различиям (т. е. к потере точности) в работе команды BULK INSERT или поставщика массового набора строк OPENROWSET по сравнению с командой **bcp**.  
  
     Сведения в элементе \<ROW> позволяют построить строку без дополнительных сведений. По этой причине набор строк можно сформировать при помощи инструкции SELECT (SELECT \* FROM OPENROWSET(BULK *datafile* FORMATFILE=*xmlformatfile*).  
  
    > [!NOTE]  
    >  Чтобы использовать предложение OPENROWSET BULK, необходим файл форматирования (обратите внимание, что преобразование типа данных поля в тип данных столбца доступно только при наличии XML-файла форматирования).  
  
###  <a name="how-bulk-import-uses-the-column-element"></a><a name="HowUsesColumn"></a> Как массовый импорт использует элемент \<COLUMN>  
 Для массового импорта данных в таблицу элементы \<COLUMN> в файле форматирования сопоставляют поле файла данных со столбцами таблицы, указывая:  
  
-   позицию каждого поля в строке файла данных;  
  
-   тип столбца, используемый для преобразования типа данных поля в необходимый тип данных столбца.  
  
 Если с полем не сопоставлено ни одного столбца, поле не копируется в сформированные строки. Такое поведение позволяет файлам данных формировать строки с различными столбцами (в разных таблицах).  
  
 Аналогично для массового экспорта данных из таблицы каждый элемент \<COLUMN> в файле форматирования сопоставляет столбец строки входной таблицы с соответствующим полем выходного файла данных.  
  
###  <a name="putting-the-xsitype-value-into-a-data-set"></a><a name="PutXsiTypeValueIntoDataSet"></a> Помещение значения xsi:type в набор данных  
 Если XML-документ проверяется при помощи языка определения схемы XML (XSD), значение xsi:type не помещается в набор данных. Тем не менее, сведения xsi:type можно поместить в набор данных, загрузив XML-файл форматирования в XML-документ, например `myDoc`, как показано в следующем фрагменте кода:  
  
```cs
...;  
myDoc.LoadXml(xmlFormat);  
XmlNodeList ColumnList = myDoc.GetElementsByTagName("COLUMN");  
for(int i=0;i<ColumnList.Count;i++)  
{  
   Console.Write("COLUMN: xsi:type=" +ColumnList[i].Attributes["type",  
      "http://www.w3.org/2001/XMLSchema-instance"].Value+"\n");  
}  
```  
  
##  <a name="sample-xml-format-files"></a><a name="SampleXmlFFs"></a> Образцы XML-файлов форматирования  
 Этот раздел содержит сведения о различных способах использования XML-файлов форматирования, в том числе пример работы с базой данных [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] .  
  
> [!NOTE]  
>  В файлах данных, приведенных в следующих примерах, `<tab>` обозначает символ табуляции в файле данных, а `<return>` означает символ возврата каретки.  
  
 Эти примеры иллюстрируют ключевые аспекты применения XML-файлов форматирования.  
  
-   [упорядочивание полей с символьными данными по столбцам таблицы;](#OrderCharFieldsSameAsCols)  
  
-   [упорядочивание полей данных в порядке, отличающемся от порядка столбцов таблицы;](#OrderFieldsAndColsDifferently)  
  
-   [пропуск поля данных;](#OmitField)  
  
-   [сопоставление различных типов полей и столбцов;](#MapXSItype)  
  
-   [сопоставление XML-данных с таблицей;](#MapXMLDataToTbl)  
  
-   [импорт полей фиксированной длины и полей фиксированной ширины.](#ImportFixedFields)  
  
-   [Дополнительный пример](#AdditionalExamples)  
  
> [!NOTE]  
>  Сведения о создании файлов форматирования см. в разделе [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
###  <a name="a-ordering-character-data-fields-the-same-as-table-columns"></a><a name="OrderCharFieldsSameAsCols"></a> A. упорядочивание полей с символьными данными по столбцам таблицы;  
 В следующем примере представлен XML-файл форматирования, описывающий файл данных, в котором содержатся три поля символьных данных. Файл форматирования сопоставляет файл данных с таблицей, содержащей три столбца. Поля данных соответствуют «один к одному» столбцам таблицы.  
  
 **Таблица (строка):** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **Файл данных (запись):** Age\<tab>FirstName\<tab>LastName\<return>  
  
 Следующий XML-файл форматирования считывает данные из файла данных в таблицу.  
  
 В элементе `<RECORD>` файл форматирования представляет значения во всех трех полях в символьном виде. Для каждого поля атрибут `TERMINATOR` указывает признак конца поля, следующий за значением.  
  
 Поля данных соответствуют «один к одному» столбцам таблицы. В элементе `<ROW>` файла форматирования столбец `Age` сопоставляется с первым полем, столбец `FirstName` — со вторым, а столбец `LastName` — с третьим.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>   
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="2" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="3" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Эквивалентный пример [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] см. в разделе [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
###  <a name="b-ordering-data-fields-and-table-columns-differently"></a><a name="OrderFieldsAndColsDifferently"></a> Б. упорядочивание полей данных в порядке, отличающемся от порядка столбцов таблицы;  
 В следующем примере представлен XML-файл форматирования, описывающий файл данных, в котором содержатся три поля символьных данных. Файл форматирования сопоставляет файл данных с таблицей, содержащей три столбца, порядок следования которых отличается от порядка следования полей файла данных.  
  
 **Таблица (строка):** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **Файл данных** (запись): Age\<tab>Lastname\<tab>Firstname\<return>  
  
 В элементе `<RECORD>` файл форматирования представляет значения во всех трех полях в символьном виде.  
  
 В элементе `<ROW>` файла форматирования столбец `Age` сопоставляется с первым полем, столбец `FirstName` — с третьим, а столбец `LastName` — со вторым.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="2" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Эквивалентный пример [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] см. в разделе [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md).  
  
###  <a name="c-omitting-a-data-field"></a><a name="OmitField"></a> В. пропуск поля данных;  
 В следующем примере представлен XML-файл форматирования, описывающий файл данных, в котором содержатся четыре поля символьных данных. Файл форматирования сопоставляет файл данных с таблицей, содержащей три столбца. Второе поле данных не связывается ни с одним столбцом таблицы.  
  
 **Таблица (строка):** Person (Age int, FirstName Varchar(20), LastName Varchar(30))  
  
 **Файл данных (запись):** Age\<tab>employeeID\<tab>Firstname\<tab>Lastname\<return>  
  
 В элементе `<RECORD>` файла форматирования значения данных представлены во всех четырех полях как символьные данные. Для каждого поля атрибут TERMINATOR указывает признак конца, следующий за значением данных.  
  
 В элементе `<ROW>` файла форматирования столбец `Age` сопоставляется с первым полем, столбец `FirstName` — с третьим, а столбец `LastName` — с четвертым.  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="10"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="4" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  Эквивалентный пример [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] см. в разделе [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md).  
  
###  <a name="d-mapping-field-xsitype-to-column-xsitype"></a><a name="MapXSItype"></a> Г. Сопоставление \<FIELD> xsi: Type и \<COLUMN> xsi: Type  
 Следующий пример демонстрирует различные типы полей и их сопоставление со столбцами.  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT  
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <RECORD>  
      <FIELD xsi:type="CharTerm" ID="C1" TERMINATOR="\t"   
            MAX_LENGTH="4"/>  
      <FIELD xsi:type="CharFixed" ID="C2" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="CharPrefix" ID="C3" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharTerm" ID="C4" TERMINATOR="\t"   
         MAX_LENGTH="4"/>  
      <FIELD xsi:type="NCharFixed" ID="C5" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharPrefix" ID="C6" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NativeFixed" ID="C7" LENGTH="4"/>  
   </RECORD>  
   <ROW>  
      <COLUMN SOURCE="C1" NAME="Age" xsi:type="SQLTINYINT"/>  
      <COLUMN SOURCE="C2" NAME="FirstName" xsi:type="SQLVARYCHAR"   
      LENGTH="16" NULLABLE="NO"/>  
      <COLUMN SOURCE="C3" NAME="LastName" />  
      <COLUMN SOURCE="C4" NAME="Salary" xsi:type="SQLMONEY"/>  
      <COLUMN SOURCE="C5" NAME="Picture" xsi:type="SQLIMAGE"/>  
      <COLUMN SOURCE="C6" NAME="Bio" xsi:type="SQLTEXT"/>  
      <COLUMN SOURCE="C7" NAME="Interest"xsi:type="SQLDECIMAL"   
      PRECISION="5" SCALE="3"/>  
   </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="e-mapping-xml-data-to-a-table"></a><a name="MapXMLDataToTbl"></a> Д. сопоставление XML-данных с таблицей;  
 В следующем примере создается пустая таблица из двух столбцов (`t_xml`), первый столбец которой сопоставляется с типом данных `int` , а второй — с типом данных `xml` .  
  
```sql
CREATE TABLE t_xml (c1 int, c2 xml)  
```  
  
 Следующий XML-файл форматирования загружает файл данных в таблицу `t_xml`.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativePrefix" PREFIX_LENGTH="1"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="c1" xsi:type="SQLINT"/>  
  <COLUMN SOURCE="2" NAME="c2" xsi:type="SQLNCHAR"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="f-importing-fixed-length-or-fixed-width-fields"></a><a name="ImportFixedFields"></a> Е. импорт полей фиксированной длины и полей фиксированной ширины.  
 В следующем примере описываются поля фиксированной ширины в `10` или `6` символов каждое. Файл форматирования представляет длину и ширину этих полей в виде `LENGTH="10"` и `LENGTH="6"`соответственно. Каждая строка файлов данных заканчивается сочетанием символов возврата каретки и перевода строки, {CR}{LF}, которое в файле форматирования представлено в виде `TERMINATOR="\r\n"`.  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT  
       xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharFixed" LENGTH="10"/>  
    <FIELD ID="2" xsi:type="CharFixed" LENGTH="6"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="C1" xsi:type="SQLINT" />  
    <COLUMN SOURCE="2" NAME="C2" xsi:type="SQLINT" />  
  </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="additional-examples"></a><a name="AdditionalExamples"></a> Дополнительные примеры  
 Дополнительные примеры как для файлов форматирования в формате, отличном от XML, так и для XML-файлов форматирования см. в следующих разделах.  
  
-   [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Создание файла форматирования (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [Использование файла форматирования для массового импорта данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [Пропуск столбца таблицы с помощью файла форматирования (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [Использование файла форматирования для пропуска поля данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [Использование файла форматирования для сопоставления столбцов таблицы с полями файла данных (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> См. также  
 Нет.  
  
## <a name="see-also"></a>См. также:  
 [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Файлы формата, отличные от XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [Файлы форматирования для импорта или экспорта данных (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
  

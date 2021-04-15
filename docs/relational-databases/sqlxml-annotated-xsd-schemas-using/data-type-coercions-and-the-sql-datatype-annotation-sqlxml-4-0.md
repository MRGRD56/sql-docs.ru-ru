---
title: 'Преобразование типов данных с помощью SQL: DataType (SQLXML)'
description: 'Узнайте, как использовать атрибуты XSD: Type и SQL: DataType в SQLXML 4,0 для управления сопоставлением типов данных XSD и SQL Server типов данных.'
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping data types [SQLXML]
- type attribute
- sql:datatype
- data types [SQLXML], converting
- annotated XSD schemas, mapping data types
- xsd:type
- datatype annotation
- converting data types [SQLXML]
- data types [SQLXML], mapping data types
- XSD schemas [SQLXML], mapping data types
ms.assetid: db192105-e8aa-4392-b812-9d727918c005
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6027201a9af3064d1cf34b91c79885b46d9bbcdb
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492134"
---
# <a name="data-type-conversions-and-the-sqldatatype-annotation-sqlxml-40"></a>Преобразования типов данных и аннотации SQL: DataType (SQLXML 4,0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В схеме XSD атрибут **xsd: Type** указывает тип данных XSD элемента или атрибута. Если схема XSD используется для получения данных из базы данных, указанный тип данных используется для форматирования данных.  
  
 Помимо указания типа XSD в схеме, можно также указать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип данных Майкрософт с помощью аннотации **SQL: DataType** . Атрибуты **xsd: Type** и **SQL: DataType** управляют сопоставлением между типами данных XSD и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типами данных.  
  
## <a name="xsdtype-attribute"></a>Атрибут xsd:type  
 Атрибут **xsd: Type** можно использовать для указания типа данных XML атрибута или элемента, который сопоставляется со столбцом. **Xsd: Type** влияет на документ, возвращаемый с сервера, а также на выполняемый запрос XPath. При выполнении запроса XPath к схеме сопоставления, содержащей **xsd: Type**, XPath использует указанный тип данных при обработке запроса. Дополнительные сведения о том, как XPath использует **xsd: Type**, см. [в разделе Сопоставление типов данных XSD с типами данных XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md).  
  
 В возвращенном документе все типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] преобразуются в строковые представления. Для некоторых типов данных необходимо дополнительное преобразование. В следующей таблице перечислены преобразования, используемые для различных значений **xsd: Type** .  
  
|Тип данных XSD|Преобразование SQL Server|  
|-------------------|---------------------------|  
|Логическое|CONVERT(bit, COLUMN)|  
|Дата|LEFT(CONVERT(nvarchar(4000), COLUMN, 126), 10)|  
|Decimal|CONVERT(money, COLUMN)|  
|id/idref/idrefs|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|nmtoken/nmtokens|id-prefix + CONVERT(nvarchar(4000), COLUMN, 126)|  
|Время|SUBSTRING(CONVERT(nvarchar(4000), COLUMN, 126), 1+CHARINDEX(N'T', CONVERT(nvarchar(4000), COLUMN, 126)), 24)|  
|Все остальные|Дополнительное преобразование не выполняется|  
  
> [!NOTE]  
>  Некоторые значения, возвращаемые функцией, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут быть несовместимы с типами данных XML, заданными с помощью **xsd: Type**, поскольку преобразование невозможно (например, преобразование "XYZ" в тип данных **Decimal** ) или значение превышает диапазон этого типа данных (например,-100000, преобразованный в тип XSD **унсигнедшорт** ). Преобразования несовместимых типов может привести к недопустимым XML-документам или ошибкам [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="mapping-from-sql-server-data-types-to-xsd-data-types"></a>Сопоставление типов данных SQL Server с типами данных XSD  
 Следующая таблица показывает очевидные сопоставления типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типам данных XSD. Если известен тип [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], в данной таблице показан соответствующий XSD-тип, который можно указать в схеме XSD.  
  
|Тип данных SQL Server|Тип данных XSD|  
|--------------------------|-------------------|  
|**bigint**|**long**|  
|**binary**|**base64Binary**|  
|**bit**|**boolean**|  
|**char**|**string**|  
|**datetime**|**dateTime**|  
|**decimal**|**decimal**|  
|**float**|**double**|  
|**image**|**base64Binary**|  
|**int**|**int**|  
|**money**|**decimal**|  
|**nchar**|**string**|  
|**ntext**|**string**|  
|**nvarchar**|**string**|  
|**numeric**|**decimal**|  
|**real**|**float**|  
|**smalldatetime**|**dateTime**|  
|**smallint**|**short**|  
|**smallmoney**|**decimal**|  
|**sql_variant**|**string**|  
|**sysname**|**string**|  
|**text**|**string**|  
|**timestamp**|**dateTime**|  
|**tinyint**|**unsignedByte**|  
|**varbinary**|**base64Binary**|  
|**varchar**|**string**|  
|**uniqueidentifier**|**string**|  
  
## <a name="sqldatatype-annotation"></a>Заметка sql:datatype  
 Для указания типа данных используется аннотация **SQL: DataType** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Эта заметка должна быть указана в следующих случаях:  
  
-   Выполняется массовый запуск в  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбец типа DateTime из XSD **DateTime**, **Date** или **time** Type. В этом случае необходимо задать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип данных столбца с помощью **SQL: datatype = "DateTime"**. Это правило применяется только для диаграмм обновления.  
  
-   Выполняется массовый запуск в столбце [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типа **uniqueidentifier** , а ЗНАЧЕНИЕМ XSD является GUID, включающий фигурные скобки ({и}). При указании **SQL: datatype = "uniqueidentifier"** фигурные скобки удаляются из значения перед вставкой в столбец. Если **SQL: DataType** не указан, значение отправляется вместе с фигурными скобками, а Вставка или обновление завершается ошибкой.  
  
-   Тип данных XML **base64Binary** сопоставляется с различными [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типами данных (**binary**, **Image** или **varbinary**). Чтобы соотнести тип данных XML **base64Binary** с конкретным [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типом данных, используйте аннотацию **SQL: DataType** . Эта заметка указывает явный тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбца, которому сопоставляется атрибут. Это полезно, если данные сохраняются в базах данных. Указав аннотацию **SQL: DataType** , можно определить явный [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип данных.  
  
 Обычно в схеме рекомендуется указывать **тип данных SQL: DataType** .  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы на основе следующих примеров, необходимо выполнить определенные требования. Дополнительные сведения см. в разделе [требования для запуска примеров SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-xsdtype"></a>A. Указание заметки xsd:type  
 В этом примере показано, как тип **даты** XSD, заданный с помощью атрибута **xsd: Type** в схеме, влияет на результирующий XML-документ. Схема обеспечивает XML-представление таблицы Sales.SalesOrderHeader в базе данных AdventureWorks.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader">  
     <xsd:complexType>  
       <xsd:attribute name="SalesOrderID" type="xsd:string" />   
       <xsd:attribute name="CustomerID"   type="xsd:string" />   
       <xsd:attribute name="OrderDate"    type="xsd:date" />   
       <xsd:attribute name="DueDate"  />   
       <xsd:attribute name="ShipDate"  type="xsd:time" />   
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 В этой схеме XSD существует три атрибута, которые возвращают значение данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если схема:  
  
-   Указывает **xsd: Type = Date** для атрибута **OrderDate** , отображается часть значения даты, возвращаемая [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для атрибута **OrderDate** .  
  
-   Указывает **xsd: Type = Time** для атрибута **ShipDate** . отображается часть времени значения, возвращаемого [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для атрибута **ShipDate** .  
  
-   Не указывает **xsd: Type** для атрибута **DueDate** , отображается то же значение, которое возвращается функцией [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл с именем xsdType.xml.  
  
2.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните файл под именем xsdTypeT.xml в том же каталоге, где был сохранен файл xsdType.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="xsdType.xml">  
        /Order  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу схемы сопоставления (файл xsdType.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\xsdType.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  

     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Частичный результирующий набор:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Order SalesOrderID="43659"   
         CustomerID="676"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
  <Order SalesOrderID="43660"   
         CustomerID="117"   
         OrderDate="2001-07-01"   
         DueDate="2001-07-13T00:00:00"   
         ShipDate="00:00:00" />   
 ...  
</ROOT>  
```  
  
 Эквивалентная схема XDR:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  
<ElementType name="Order" sql:relation="Sales.SalesOrderHeader">  
    <AttributeType name="SalesOrderID" />  
    <AttributeType name="CustomerID"  />  
    <AttributeType name="OrderDate" dt:type="date" />  
    <AttributeType name="DueDate" />  
    <AttributeType name="ShipDate" dt:type="time" />  
  
    <attribute type="SalesOrderID" sql:field="OrderID" />  
    <attribute type="CustomerID" sql:field="CustomerID" />  
    <attribute type="OrderDate" sql:field="OrderDate" />  
    <attribute type="DueDate" sql:field="DueDate" />  
    <attribute type="ShipDate" sql:field="ShipDate" />  
</ElementType>  
</Schema>  
```  
  
### <a name="b-specifying-sql-data-type-using-sqldatatype"></a>Б. Указание типа данных SQL с помощью заметки sql:datatype  
 Рабочий пример см. в разделе пример инструкции для [групповой загрузки XML &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md). В этом примере выполняется массовая загрузка значения идентификатора GUID, включая "{" и "}". Схема в этом примере определяет тип данных **SQL: DataType** , чтобы указать значение [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типа **uniqueidentifier**. В этом примере показано, когда в схеме должно быть указано значение **SQL: DataType** .  
  
  

---
title: Пользовательские сопоставления XSD с таблицами и столбцами (SQLXML)
description: Узнайте, как создать пользовательское сопоставление в запросе SQLXML XPath между элементами и атрибутами XSD-схемы, а также таблицами и столбцами реляционной базы данных.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- XPath queries [SQLXML], annotated XSD schemas in queries
- sql:field
- row mapping [SQLXML]
- attribute mapping [SQLXML], explicit mapping
- field annotation
- XSD schemas [SQLXML], mapping attributes and elements
- names [SQLXML]
- relation annotation
- table/view mapping [SQLXML], explicit mapping
- sql:relation
- mapping schema [SQLXML], explicit mapping
- annotated XSD schemas, mapping attributes and elements
- column mapping [SQLXML]
- element mapping [SQLXML], explicit mapping
- table mapping [SQLXML], explicit mapping
- element/attribute mapping [SQLXML]
ms.assetid: 7a5ebeb6-7322-4141-a307-ebcf95976146
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 32400aaf2b51a5152315c8960129310268a1faaf
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492123"
---
# <a name="custom-xsd-mappings-to-tablescolumns-sqlxml"></a>Пользовательские сопоставления XSD с таблицами и столбцами (SQLXML)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  При использовании схемы XSD для представления реляционных баз данных в виде XML элементы и атрибуты схемы должны быть сопоставлены с таблицами и столбцами базы данных. Строки таблицы или представления базы данных будут сопоставлены с элементами в XML-документе. Значения столбцов базы данных сопоставляются с атрибутами и элементами.  
  
 При запросах XPath к схеме XSD c заметками данные для элементов и атрибутов схемы берутся из таблиц и столбцов, которым они сопоставлены.  Для получения единичного значения из базы данных сопоставление, заданное схемой XSD, должно содержать спецификации и связей, и полей.  Если имя элемента или атрибута отличается от имени таблицы или представления или имени столбца, с которым оно сопоставляется, заметки **SQL: relation** и **SQL: field** используются для указания сопоставления между элементом или атрибутом в XML-документе и таблицей (представления) или столбцом в базе данных.  
  
## <a name="sql-relation"></a>sql-relation  
 Для отображения XML-узла в схеме XSD в таблицу базы данных добавляется аннотация **SQL: relation** . Имя таблицы (представления) указывается в качестве значения заметки **SQL: relation** .  
  
 Если для элемента задано значение **SQL: relation** , область этой заметки применяется ко всем атрибутам и дочерним элементам, описанным в определении сложного типа этого элемента, таким образом предоставляя ярлык при записи заметок.  
  
 Аннотация **SQL: relation** также полезна, если идентификаторы, допустимые в [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , недопустимы в XML. Например, «Order Details» — допустимое имя таблицы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], но не в XML. В таких случаях можно использовать аннотацию **SQL: relation** для указания сопоставления, например:  
  
```  
<xsd:element name="OD" sql:relation="[Order Details]">  
```  
  
## <a name="sql-field"></a>sql-field  
 Заметка **SQL-Field** сопоставляет элемент или атрибут со столбцом базы данных. Заметка **SQL: field** добавляется для соответствия узла XML в схеме столбцу базы данных. Нельзя указать **SQL: field** для пустого элемента содержимого.  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы на основе следующих примеров, необходимо выполнить определенные требования. Дополнительные сведения см. в разделе [требования для запуска примеров SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-the-sqlrelation-and-sqlfield-annotations"></a>A. Задание заметок sql:relation и sql:field  
 В этом примере схема XSD состоит из **\<Contact>** элемента сложного типа с **\<FName>** **\<LName>** дочерними элементами и атрибутом **ContactID** .  
  
 Аннотация **SQL: relation** сопоставляет **\<Contact>** элемент с таблицей Person. Contact в базе данных AdventureWorks. Заметка **SQL: field** сопоставляет **\<FName>** элемент столбцу FirstName и **\<LName>** элементу со столбцом LastName.  
  
 Для атрибута **ContactID** не задана Аннотация. Поэтому атрибут по умолчанию сопоставляется со столбцом с тем же именем.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Contact" sql:relation="Person.Contact" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="FName"  
                     sql:field="FirstName"   
                     type="xsd:string" />   
        <xsd:element name="LName"    
                     sql:field="LastName"    
                     type="xsd:string" />  
     </xsd:sequence>  
        <xsd:attribute name="ContactID"   
                       type="xsd:integer" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл под именем MySchema-annotated.xml.  
  
2.  Скопируйте приведенный ниже шаблон и вставьте его в текстовый файл. Сохраните файл под именем MySchema-annotatedT.xml в том же каталоге, где был сохранен файл MySchema-annotated.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="MySchema-annotated.xml">  
        /Contact  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу схемы сопоставления (файл MySchema-annotated.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\MySchema-annotated.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Частичный результирующий набор:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
 <Contact ContactID="1">   
    <FName>Gustavo</FName>   
    <LName>Achong</LName>   
 </Contact>   
  .....  
</ROOT>  
```  
  
  

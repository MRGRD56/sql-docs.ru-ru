---
title: 'Исключение элементов схемы из XML-документа с помощью SQL: сопоставлено'
description: 'Узнайте, как использовать заметку SQL: Map для создания элемента в схеме XSD, который не сопоставлен с таблицей (представлением) или столбцом базы данных.'
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- element does not map [SQLXML]
- annotated XSD schemas, excluding schema elements
- mapped annotation
- table mapping [SQLXML], excluding schema elements
- sql:mapped
- excluding schema elements
- element mapping [SQLXML], excluding schema elements
- column mapping [SQLXML]
- XSD schemas [SQLXML], excluding schema elements
- attribute mapping [SQLXML], excluding schema elements
- table/view mapping [SQLXML], excluding schema elements
ms.assetid: 7d2649dd-0038-4a2c-b16d-f80f7c306966
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 96b3c4fc2eacccfd274dde8040a4473d3ed8503f
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492080"
---
# <a name="excluding-schema-elements-from-the-xml-document-using-sqlmapped"></a>Исключение элементов схемы из XML-документа с помощью sql:mapped
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В результате сопоставления по умолчанию каждый элемент и атрибут в схеме XSD будет сопоставлен с таблицей и столбцом в базе данных. Если необходимо создать элемент в схеме XSD, который не сопоставлен ни с одной таблицей (представлением) базы данных или столбцом, который не отображается в XML, можно указать заметку **SQL:** Map.  
  
 Если схема не может быть изменена или схема используется для проверки XML из других источников и содержит данные, не хранящиеся в базе данных **, это особенно** полезно. **Сопоставленная Аннотация SQL:** **имеет значение-Constant** в том, что несопоставленные элементы и атрибуты не отображаются в XML-документе.  
  
 **Сопоставленная Аннотация SQL:** принимает логическое значение (0 = false, 1 = true). Допустимые значения: 0, 1, true и false.  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы на основе следующих примеров, необходимо выполнить определенные требования. Дополнительные сведения см. в разделе [требования для запуска примеров SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-the-sqlmapped-annotation"></a>A. Задание заметки sql:mapped  
 Предположим, существует схема XSD, полученная из другого источника. Эта схема XSD состоит из **\<Person.Contact>** элемента с атрибутами **ContactID**, **FirstName**, **LastName** и **HomeAddress** .  
  
 При сопоставлении этой схемы XSD с таблицей Person. Contact в базе данных AdventureWorks в атрибуте **HomeAddress** указывается значение **SQL:** mappingd, поскольку в таблице Employees не хранятся домашние адреса сотрудников. В результате этот атрибут не сопоставлен с базой данных и не возвращается в результирующем XML-документе в ответ на запрос XPath к схеме сопоставления.  
  
 Для остальной части схемы используется сопоставление по умолчанию. **\<Person.Contact>** Элемент сопоставляется с таблицей Person. Contact, и все атрибуты сопоставляются со столбцами с тем же именем в таблице Person. Contact.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Person.Contact">  
    <xsd:complexType>  
      <xsd:attribute name="ContactID"   type="xsd:string"/>  
      <xsd:attribute name="FirstName"    type="xsd:string" />  
      <xsd:attribute name="LastName"     type="xsd:string" />  
      <xsd:attribute name="HomeAddress" type="xsd:string"   
                     sql:mapped="false" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл под именем sql-mapped.xml.  
  
2.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните файл под именем sql-mappedT.xml в том же каталоге, где был сохранен файл sql-mapped.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="sql-mapped.xml">  
            /Person.Contact[@ContactID < 10]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу для схемы сопоставления (MySchema.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\MyDir\sql-mapped.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  

     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Результирующий набор:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact ContactID="1" FirstName="Gustavo" LastName="Achong" />   
  <Person.Contact ContactID="2" FirstName="Catherine" LastName="Abel" />   
  <Person.Contact ContactID="3" FirstName="Kim" LastName="Abercrombie" />   
  <Person.Contact ContactID="4" FirstName="Humberto" LastName="Acevedo" />   
  <Person.Contact ContactID="5" FirstName="Pilar" LastName="Ackerman" />   
  <Person.Contact ContactID="6" FirstName="Frances" LastName="Adams" />   
  <Person.Contact ContactID="7" FirstName="Margaret" LastName="Smith" />   
  <Person.Contact ContactID="8" FirstName="Carla" LastName="Adams" />   
  <Person.Contact ContactID="9" FirstName="Jay" LastName="Adams" />   
</ROOT>  
```  
  
 Обратите внимание, что указываются ContactID, FirstName и LastName, но HomeAddress не так, поскольку схема сопоставления задается значение 0 для атрибута **SQL:** mappingd.  
  
## <a name="see-also"></a>См. также  
 [Сопоставление элементов и атрибутов XSD с таблицами и столбцами по умолчанию &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)  
  
  

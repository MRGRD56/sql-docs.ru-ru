---
title: 'Создание разделов CDATA с помощью SQL: use-CDATA (SQLXML)'
description: 'Узнайте, как создать разделы CDATA в SQLXML 4,0, используя аннотацию SQL: use-CDATA для экранирования блоков текста, содержащих символы разметки.'
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- markup characters [SQLXML]
- special characters [SQLXML]
- use-cdata annotation
- plain text [SQLXML]
- CDATA sections
- escaping blocks of text [SQLXML]
- annotated XSD schemas, CDATA sections
- sql:use-cdata
ms.assetid: 26d2b9dc-f857-44ff-bcd4-aaf64ff809d0
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 24048e64d0619c93026e88b2d24a28e29b4f7b0b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492155"
---
# <a name="creating-cdata-sections-using-sqluse-cdata-sqlxml-40"></a>Создание разделов CDATA с использованием sql:use-cdata (SQLXML 4.0)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В XML разделы CDATA используются для экранирования блоков текста, содержащих символы, которые в противном случае распознаются как символы разметки.  
  
 База данных в корпорации Майкрософт [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] иногда может содержать символы, которые обрабатываются синтаксическим анализатором XML как символы разметки, например угловые скобки (< и >), символ "меньше или равно" (<=), а амперсанд (&) обрабатывается как символы разметки. Но специальные символы такого типа можно заключить в раздел CDATA во избежание их обработки в качестве символов разметки. Текст в разделе CDATA обрабатывается средством синтаксического анализа XML как простой текст.  
  
 Аннотация **SQL: use-CDATA** указывает, что возвращаемые данные [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должны быть заключены в раздел CDATA (то есть указывает, следует ли заключать значение из столбца, указанного в параметре **SQL: field** в раздел CDATA). Аннотация **SQL: use-CDATA** может быть указана только для элементов, сопоставленных со столбцом базы данных.  
  
 Аннотация **SQL: use-CDATA** принимает логическое значение (0 = false, 1 = true). Допустимые значения: 0, 1, true и false.  
  
 Эту аннотацию нельзя использовать с типами атрибутов ID, IDREF, IDREFS, NMTOKEN и NMTOKENS в **кодировке SQL: URL-кодирование** .  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы на основе следующих примеров, необходимо выполнить определенные требования. Дополнительные сведения см. в разделе [требования для запуска примеров SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-sqluse-cdata-on-an-element"></a>A. Указание заметки sql:use-cdata для элемента  
 В следующей схеме **SQL: use-CDATA** устанавливается в значение 1 (true) для в **\<AddressLine1>** пределах **\<Address>** элемента. В результате этого данные возвращаются в разделе CDATA.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Address"   
               sql:relation="Person.Address"   
               sql:key-fields="AddressID" >  
   <xsd:complexType>  
        <xsd:sequence>  
          <xsd:element name="AddressID"  type="xsd:string" />  
          <xsd:element name="AddressLine1" type="xsd:string"   
                       sql:use-cdata="1" />  
        </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните этот файл с именем UseCData.xml.  
  
2.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните этот файл под именем UseCDataT.xml в том же каталоге, в котором был сохранен файл UseCData.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="UseCData.xml">  
            /Address[AddressID < 11]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу схемы сопоставления (файл UseCData.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\UseCData.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Ниже приведен частичный результирующий набор.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">   
  <Address>   
    <AddressID>1</CustomerID>   
    <AddressLine1>   
      <![CDATA[ 1970 Napa Ct.  ]]>   
    </AddressLine1>   
  </Address>  
  <Address>  
    <AddressLine1>   
      <![CDATA[ 9833 Mt. Dias Blv. ]]>   
    </AddressLine1>   
  </Address>  
  ...  
</ROOT>  
```  
  
  

---
title: 'Получение URL-ссылок на данные большого двоичного объекта с помощью SQL: Encoded (SQLXML)'
description: 'Узнайте, как запросить URL-ссылку на данные большого двоичного объекта, указав аннотацию SQL: Encoded в SQLXML 4,0.'
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:encode
- encode annotation
- URL references to BLOB data [SQLXML]
- references to BLOB data [SQLXML]
- annotated XSD schemas, URL references to BLOB data
- requesting URL references to BLOB data
- BLOBs, URL references
- Base 64-encoded format
ms.assetid: 2f8cd93b-c636-462b-8291-167197233ee0
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 81f5d833a18d3dbb5709715c47c506900f491a4a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492068"
---
# <a name="requesting-url-references-to-blob-data-using-sqlencode-sqlxml-40"></a>Получение URL-ссылок на данные BLOB с использованием sql:encode (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В схеме XSD с заметками, когда атрибут (или элемент) сопоставляется со столбцом BLOB в Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], данные внутри XML возвращаются в формате Base 64.  
  
 Если требуется вернуть ссылку на данные (URI), которые можно использовать позже для получения данных большого двоичного объекта в двоичном формате, укажите аннотацию **SQL: Encoded** . Можно указать **SQL: Encoded** для атрибута или элемента простого типа.  
  
 Укажите заметку **SQL: Encoded** , чтобы указать, что вместо значения поля должно возвращаться URL-адрес поля. **SQL: Encoded** зависит от первичного ключа для создания одноэлементного выбора в URL-адресе. Первичный ключ можно указать с помощью заметки **SQL: Key-Fields** .  
  
 Заметке **SQL: Encoded** можно присвоить значение "URL" или "default". Значение «default» возвращает данные в формате Base 64.  
  
 Аннотацию **SQL: Encoded** нельзя использовать с **SQL: use-CDATA** или для типов атрибутов ID, IDREF, IDREFS, NMTOKEN или NMTOKENS. Его также можно использовать с атрибутом **fixed** XSD.  
  
> [!NOTE]  
>  Столбцы типа BLOB невозможно использовать, как часть ключа или внешнего ключа.  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы на основе следующих примеров, необходимо выполнить определенные требования. Дополнительные сведения см. в разделе [требования для запуска примеров SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-sqlencode-to-obtain-a-url-reference-to-blob-data"></a>A. Указание заметки sql:encode для получения URL-ссылки на данные BLOB  
 В этом примере схема сопоставления задает **SQL: Encoded** для атрибута **LargePhoto** , чтобы получить ссылку URI на определенную фотографию продукта (вместо извлечения двоичных данных в формате Base 64).  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
  <xsd:element name="ProductPhoto" sql:relation="Production.ProductPhoto"   
               sql:key-fields="ProductPhotoID" >  
   <xsd:complexType>  
      <xsd:attribute name="ProductPhotoID"  type="xsd:int"  />  
     <xsd:attribute name="LargePhoto" type="xsd:string" sql:encode="url" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл с именем sqlEncode.xml.  
  
2.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните файл под именем sqlEncodeT.xml в том же каталоге, где был сохранен файл sqlEncode.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="sqlEncode.xml">  
            /ProductPhoto[@ProductPhotoID=100]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу схемы сопоставления (файл sqlEncode.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\SqlXmlTest\sqlEncode.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Результат:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <ProductPhoto ProductPhotoID="100"  
                 LargePhoto="dbobject/Production.ProductPhoto[@ProductPhotoID="100"]/@LargePhoto" />   
</ROOT>  
```  
  
  

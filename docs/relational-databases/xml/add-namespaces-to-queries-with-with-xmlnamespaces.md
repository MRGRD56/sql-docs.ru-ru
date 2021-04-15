---
title: Добавление пространств имен в запросы с помощью WITH XMLNAMESPACES | Документация Майкрософт
description: Узнайте, как добавлять пространства имен в запросы с помощью предложения WITH XMLNAMESPACES.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- ELEMENTS XSINIL directive
- adding namespaces
- XSINIL directive
- default namespaces
- queries [XML in SQL Server], WITH XMLNAMESPACES clause
- predefined namespaces [XML in SQL Server]
- FOR XML clause, WITH XMLNAMESPACES clause
- namespaces [XML in SQL Server]
- xml data type [SQL Server], WITH XMLNAMESPACES clause
- WITH XMLNAMESPACES clause
ms.assetid: 2189cb5e-4460-46c5-a254-20c833ebbfec
author: rothja
ms.author: jroth
ms.openlocfilehash: f321511106d37539c4ce26c64fe98f8413d97845
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489052"
---
# <a name="add-namespaces-to-queries-with-with-xmlnamespaces"></a>Добавление пространств имен в запросы с WITH XMLNAMESPACES
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Предложение[WITH XMLNAMESPACES (Transact-SQL)](../../t-sql/xml/with-xmlnamespaces.md) поддерживает пространство имен URI следующим образом:  
  
-   Разрешается сопоставление префикса пространства имен с адресом URI при [создании XML с помощью предложения FOR XML](../../relational-databases/xml/for-xml-sql-server.md) .  
  
-   Разрешается сопоставление пространства имен с адресом URI в статическом контексте пространств имен [методов типа данных xml](../../t-sql/xml/xml-data-type-methods.md).  
  
## <a name="using-with-xmlnamespaces-in-the-for-xml-queries"></a>Использование предложения WITH XMLNAMESPACES в запросах FOR XML  
 Использование предложения WITH XMLNAMESPACES позволяет включать пространства имен XML в запросы FOR XML. Например, рассмотрим следующий запрос FOR XML:  
  
```  
SELECT ProductID, Name, Color  
FROM   Production.Product  
WHERE  ProductID=316 or ProductID=317  
FOR XML RAW  
```  
  
 Результат:  
  
```  
<row ProductID="316" Name="Blade" />  
<row ProductID="317" Name="LL Crankarm" Color="Black" />  
  
```  
  
 Чтобы добавить пространства имен в XML-документ, созданный с помощью запроса FOR XML, вначале с помощью предложения WITH NAMESPACES укажите префикс пространства имен для сопоставления с адресом URI. Затем используйте эти префиксы пространств имен для указания имен в запросе, как показано в следующем измененном запросе. Обратите внимание, что предложение WITH XMLNAMESPACES указывает префикс пространства имен (`ns1`) для сопоставления с адресом URI (`uri`). Затем префикс `ns1` используется для указания создаваемого элемента и имен атрибутов в запросе FOR XML.  
  
```  
WITH XMLNAMESPACES ('uri' as ns1)  
SELECT ProductID as 'ns1:ProductID',  
       Name      as 'ns1:Name',   
       Color     as 'ns1:Color'  
FROM Production.Product  
WHERE ProductID=316 or ProductID=317  
FOR XML RAW ('ns1:Prod'), ELEMENTS  
  
```  
  
 Результирующий XML-документ содержит префиксы пространства имен:  
  
```  
<ns1:Prod xmlns:ns1="uri">  
  <ns1:ProductID>316</ns1:ProductID>  
  <ns1:Name>Blade</ns1:Name>  
</ns1:Prod>  
<ns1:Prod xmlns:ns1="uri">  
  <ns1:ProductID>317</ns1:ProductID>  
  <ns1:Name>LL Crankarm</ns1:Name>  
  <ns1:Color>Black</ns1:Color>  
</ns1:Prod>  
  
```  
  
 Предложение WITH XMLNAMESPACES имеет следующее применение:  
  
-   Оно может быть использовано в запросах FOR XML только в режимах RAW, AUTO и PATH. Режим EXPLICIT не поддерживается.  
  
-   Оно влияет только на префиксы пространств имен в запросах FOR XML и методах типа данных **xml** , но не на синтаксический анализатор XML. Например, в результате выполнения следующего запроса будет получена ошибка, так как в XML-документе не содержится пространства имени, связанного с префиксом myNS.  
  
-   Такие директивы FOR XML, как XMLSCHEMA и XMLDATA, не могут быть использованы в предложении WITH XMLNAMESPACES.  
  
    ```  
    CREATE TABLE T (x xml)  
    go  
    WITH XMLNAMESPACES ('https://abc' as myNS )  
    INSERT INTO T VALUES('<myNS:root/>')  
    ```  
  
## <a name="using-the-xsinil-directive"></a>Использование директивы XSINIL  
 При использовании директивы ELEMENTS XSINIL xsi-префиксы в предложении WITH XMLNAMESPACES использовать нельзя. При использовании директивы ELEMENTS XSINIL префиксы добавляются автоматически. В следующем запросе используется директива ELEMENTS XSINIL, формирующая ориентированный на элементы XML-документ, в котором пустые значения сопоставляются с элементами, для которых атрибут **xsi:nil** имеет значение True.  
  
```  
WITH XMLNAMESPACES ('uri' as ns1)  
SELECT ProductID as 'ns1:ProductID',  
       Name      as 'ns1:Name',   
       Color     as 'ns1:Color'  
FROM Production.Product  
WHERE ProductID=316   
FOR XML RAW, ELEMENTS XSINIL  
```  
  
 Результат:  
  
```  
<row xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:ns1="uri">  
  <ns1:ProductID>316</ns1:ProductID>  
  <ns1:Name>Blade</ns1:Name>  
  <ns1:Color xsi:nil="true" />  
</row>  
```  
  
## <a name="specifying-default-namespaces"></a>Указание пространств имен по умолчанию  
 Вместо объявления префикса пространства имени можно объявить пространство имени с помощью ключевого слова DEFAULT. В запросе FOR XML таким образом происходит привязка пространства имен по умолчанию к XML-узлам результирующего XML-документа. В представленном ниже примере с помощью предложения WITH XMLNAMESPACES задается два префикса пространства имен, которые определяются вместе с пространством имен по умолчанию.  
  
```  
WITH XMLNAMESPACES ('uri1' as ns1,   
                    'uri2' as ns2,  
                    DEFAULT 'uri2')  
SELECT ProductID,   
      Name,  
      Color  
FROM Production.Product   
WHERE ProductID=316 or ProductID=317  
FOR XML RAW ('ns1:Product'), ROOT('ns2:root'), ELEMENTS  
```  
  
 В результате выполнения запроса FOR XML формируется ориентированный на элементы XML-документ. Заметим, что в запросе используются оба префикса пространства имен в узле имен. В предложении SELECT параметры ProductID, Name и Color не указывают имен с какими-либо префиксами. Поэтому соответствующие элементы в результирующем XML-документе принадлежат к пространству имен по умолчанию.  
  
```  
<ns2:root xmlns="uri2" xmlns:ns2="uri2" xmlns:ns1="uri1">  
  <ns1:Product>  
    <ProductID>316</ProductID>  
    <Name>Blade</Name>  
  </ns1:Product>  
  <ns1:Product>  
    <ProductID>317</ProductID>  
    <Name>LL Crankarm</Name>  
    <Color>Black</Color>  
  </ns1:Product>  
</ns2:root>  
```  
  
 Приведенный ниже запрос повторяет предыдущий за исключением указания режима FOR XML AUTO.  
  
```  
WITH XMLNAMESPACES ('uri1' as ns1,  'uri2' as ns2,DEFAULT 'uri2')  
SELECT ProductID,   
      Name,  
      Color  
FROM Production.Product as "ns1:Product"  
WHERE ProductID=316 or ProductID=317  
FOR XML AUTO, ROOT('ns2:root'), ELEMENTS  
```  
  
## <a name="using-predefined-namespaces"></a>Использование предопределенных пространств имен  
 При использовании предопределенных пространств имен, кроме случаев использования пространств имен xml и xsi в директиве ELEMENTS XSINIL, необходимо явно указывать привязку пространств имен в предложении WITH XMLNAMESPACES. В следующем запросе происходит задание префикса пространства имен для привязки к адресу URI для предопределенного пространства имен (`urn:schemas-microsoft-com:xml-sql`).  
  
```  
WITH XMLNAMESPACES ('urn:schemas-microsoft-com:xml-sql' as sql)  
SELECT 'SELECT * FROM Customers FOR XML AUTO, ROOT("a")' AS "sql:query"  
FOR XML PATH('sql:root')  
```  
  
 Результат. Данный XML-шаблон известен пользователям SQLXML. Дополнительные сведения см. в статье [Основные понятия о программировании для SQLXML 4.0](../../relational-databases/sqlxml/sqlxml-4-0-programming-concepts.md).  
  
```  
<sql:root xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query>SELECT * FROM Customers FOR XML AUTO, ROOT("a")</sql:query>  
</sql:root>  
```  
  
 Только XML-префикс пространства имен может быть использован без явного задания в предложении WITH XMLNAMESPACES, как показано в следующем примере запроса в режиме PATH. Также при объявлении префикса его необходимо привязать к пространству имен http://www.w3.org/XML/1998/namespace. Имена, указанные в предложении SELECT, ссылаются на XML-префикс пространства имен, который не был явно задан с помощью предложения WITH XMLNAMESPACES.  
  
```  
SELECT 'en'    as "English/@xml:lang",  
       'food'  as "English",  
       'ger'   as "German/@xml:lang",  
       'Essen' as "German"  
FOR XML PATH ('Translation')  
go  
```  
  
 Атрибуты @xml:lang используют предопределенное пространство имен XML. Так как в XML версии 1.0 нет необходимости явно задавать привязку пространства имен XML, то она не включается в результат.  
  
 Результат:  
  
```  
<Translation>  
  <English xml:lang="en">food</English>  
  <German xml:lang="ger">Essen</German>  
</Translation>  
```  
  
## <a name="using-with-xmlnamespaces-with-the-xml-data-type-methods"></a>Использование предложения WITH XMLNAMESPACES с методами типа данных xml  
 Все [методы типа данных xml](../../t-sql/xml/xml-data-type-methods.md) , указанные в запросе SELECT (или UPDATE в случае метода **modify()** ), должны повторять в своем прологе объявление пространства имен. Это может занять некоторое время. Например, в результате выполнения следующего запроса получаются идентификаторы моделей, в описание каталогов которых включены спецификации. То есть те, для которых существует элемент <`Specifications`>.  
  
```  
SELECT ProductModelID, CatalogDescription.query('  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
    declare namespace  pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /pd:ProductDescription[(pd:Specifications)]'  
    ) = 1  
```  
  
 В представленном выше запросе в прологах обоих методов ( **query()** и **exist()** ) объявляются одинаковые пространства имен. Пример:  
  
```  
declare namespace pd="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
```  
  
 Того же результата можно достичь, если вначале объявить предложение WITH XMLNAMESPACES, а затем использовать указанные в нем префиксы пространства имен в запросе. В таком случае в прологах обоих методов ( **query()** и **exist()** ) пространства имен не объявляются.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' as pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Product   
        ProductModelID= "{ sql:column("ProductModelID") }"   
        />  
') AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('  
     /pd:ProductDescription[(pd:Specifications)]'  
    ) = 1  
Go  
```  
  
 Обратите внимание, что при явном объявлении в прологе запроса на языке XQuery, указанные в нем префиксы пространства имен переопределяют все указанные ранее и заданные по умолчанию с помощью предложения WITH префиксы.  
  
## <a name="see-also"></a>См. также:  
 [методов типа данных xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Справочник по языку XQuery (SQL Server)](../../xquery/xquery-language-reference-sql-server.md)   
 [WITH XMLNAMESPACES (Transact-SQL)](../../t-sql/xml/with-xmlnamespaces.md)   
 [FOR XML (SQL Server)](../../relational-databases/xml/for-xml-sql-server.md)  
  
  

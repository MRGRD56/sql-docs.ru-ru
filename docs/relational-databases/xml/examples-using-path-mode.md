---
title: 'Примеры: Использование режима PATH | Документация Майкрософт'
description: Рассмотрение примеров использования режима PATH для создания XML-кода из запроса SELECT.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- PATH FOR XML mode, examples
ms.assetid: 3564e13b-9b97-49ef-8cf9-6a78677b09a3
author: rothja
ms.author: jroth
ms.openlocfilehash: c0b79a861d10c2c0997d7455eb8e7f212706a495
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490830"
---
# <a name="examples-using-path-mode"></a>Примеры: Использование режима PATH
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В следующих примерах показано использование режима PATH при формировании XML из запроса SELECT. Многие из этих запросов являются запросами к XML-документам с инструкциями по производству велосипедов, хранящимся в столбце Instructions таблицы ProductModel.  
  
## <a name="specifying-a-simple-path-mode-query"></a>Указание простого запроса в режиме PATH  
 Этот запрос указывает режим FOR XML PATH.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT   
       ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML PATH;  
GO  
```  
  
 Следующий результат представляет собой элементный XML, в котором значение каждого столбца в итоговом наборе строк образует элемент. Поскольку предложение `SELECT` не указывает псевдонимы для имен столбцов, формируются имена дочерних элементов, совпадающие с именами соответствующих столбцов в предложении `SELECT`. Для каждой строки в наборе строк добавляется тег <`row`>.  
  
 ```
<row>  
  <ProductModelID>122</ProductModelID>  
  <Name>All-Purpose Bike Stand</Name>  
</row>  
<row>  
  <ProductModelID>119</ProductModelID>
  <Name>Bike Wash</Name>
</row>
```
  
 Следующий результат совпадает с результатом запроса в режиме `RAW` с указанным параметром `ELEMENTS` . Запрос возвращает элементный XML с установленным по умолчанию элементом <`row`> для каждой строки в результирующем наборе.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML RAW, ELEMENTS;  
```  
  
 Дополнительно можно указать имя элемента строки, которое переопределит значение по умолчанию <`row`>. Например, следующий запрос возвращает элемент <`ProductModel`> для каждой строки в наборе строк.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML PATH ('ProductModel');  
GO  
```  
  
 Итоговый XML-документ будет иметь указанное имя элемента строки.  
  
```
<ProductModel>
  <ProductModelID>122</ProductModelID>
  <Name>All-Purpose Bike Stand</Name>
</ProductModel>
<ProductModel>
  <ProductModelID>119</ProductModelID>
  <Name>Bike Wash</Name>
</ProductModel>
```
  
 Если задана строка нулевой длины, закрывающий тег не формируется.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID,  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML PATH ('');  
GO  
```  
  
 Результат:  
  
```
<ProductModelID>122</ProductModelID>
<Name>All-Purpose Bike Stand</Name>
<ProductModelID>119</ProductModelID>
<Name>Bike Wash</Name>
```
  
## <a name="specifying-xpath-like-column-names"></a>Указание имен столбцов, подобных синтаксису языка XPath  
 В следующем запросе указанное имя столбца `ProductModelID` начинается с символа \@ и не содержит косой черты (/). Поэтому в итоговом XML-документе создается атрибут элемента <`row`>, имеющий соответствующее значение столбца.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID AS "@id",  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 OR ProductModelID=119  
FOR XML PATH ('ProductModelData');  
GO  
```  
  
 Результат:  
  
```
<ProductModelData id="122">
  <Name>All-Purpose Bike Stand</Name>
</ProductModelData>
<ProductModelData id="119">
  <Name>Bike Wash</Name>
</ProductModelData>
```
  
 Указав в запросе `root` параметр `FOR XML`, можно добавить один элемент верхнего уровня.  
  
```  
SELECT ProductModelID AS "@id",  
       Name  
FROM Production.ProductModel  
WHERE ProductModelID=122 or ProductModelID=119  
FOR XML PATH ('ProductModelData'), root ('Root');  
GO  
```  
  
 Для формирования иерархии можно добавить синтаксис, подобный PATH. Если, например, изменить имя столбца `Name` на «SomeChild/ModelName», можно получить XML-документ с иерархией, показанной в следующем результате:  
  
```
<Root>
  <ProductModelData id="122">
    <SomeChild>
      <ModelName>All-Purpose Bike Stand</ModelName>
    </SomeChild>
  </ProductModelData>
  <ProductModelData id="119">
    <SomeChild>
      <ModelName>Bike Wash</ModelName>
    </SomeChild>
  </ProductModelData>
</Root>
```
  
 Следующий запрос извлекает не только код модели продукта и его имя, но и расположения производственных инструкций для модели продукции. Поскольку столбец Instructions имеет тип **xml** , то для получения расположения указывается метод **query()** типа данных **xml** .  
  
```  
SELECT ProductModelID AS "@id",  
       Name,  
       Instructions.query('declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
                /MI:root/MI:Location   
              ') AS ManuInstr  
FROM Production.ProductModel  
WHERE ProductModelID = 7  
FOR XML PATH ('ProductModelData'), root ('Root');  
GO  
```  
  
 Частичный результат. Поскольку в данном запросе имя столбца указано как ManuInstr, XML-данные, возвращенные методом **query()** , помещаются в тег <`ManuInstr`>, как показано ниже:  

```
<Root>
  <ProductModelData id="7">
    <Name>HL Touring Frame</Name>
    <ManuInstr>
      <MI:Location xmlns:MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"
        <MI:step>...</MI:step>...
      </MI:Location>
      ...
    </ManuInstr>
  </ProductModelData>
</Root> 
```

В предыдущий запрос FOR XML можно было включить пространства имен для элементов <`Root`> и <`ProductModelData`>. Для этого следовало сначала определить префикс пространства имен, создать привязку с помощью предложения WITH XMLNAMESPACES и затем использовать префиксы в запросе FOR XML. Дополнительные сведения с. в разделе [Добавление пространств имен в запросы с помощью WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
```  
USE AdventureWorks2012;  
GO  
WITH XMLNAMESPACES (  
   'uri1' AS ns1,    
   'uri2' AS ns2,  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' as MI)  
SELECT ProductModelID AS "ns1:ProductModelID",  
       Name           AS "ns1:Name",  
       Instructions.query('  
                /MI:root/MI:Location   
              ')   
FROM Production.ProductModel  
WHERE ProductModelID=7  
FOR XML PATH ('ns2:ProductInfo'), root('ns1:root');  
GO  
```  
  
 Обратите внимание, что префикс `MI` также определяется в предложении `WITH XMLNAMESPACES`. В результате метод **query()** заданного типа **xml** не определяет префикс в прологе запроса. Результат:  
  
```
<ns1:root xmlns:MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" xmlns="uri2" xmlns:ns2="uri2" xmlns:ns1="uri1">
  <ns2:ProductInfo>
    <ns1:ProductModelID>7</ns1:ProductModelID>
    <ns1:Name>HL Touring Frame</ns1:Name>
    <MI:Location xmlns:MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" LaborHours="2.5" LotSize="100" MachineHours="3" SetupHours="0.5" LocationID="10" xmlns="">
    <MI:step>
      Insert <MI:material>aluminum sheet MS-2341</MI:material> into the <MI:tool>T-85A framing tool</MI:tool>.
    </MI:step>
    ...
    </MI:Location>
    ...
  </ns2:ProductInfo>
</ns1:root>
```
  
## <a name="generating-a-value-list-using-path-mode"></a>Формирование списка значений с помощью режима PATH  
 Данный запрос строит список значений кодов продуктов для каждой модели продукции. Кроме того, для каждого кода продукта запрос создает вложенные элементы <`ProductName`>, как показано в следующем фрагменте XML:  

```
<ProductModelData ProductModelID="7" ProductModelName="..." ProductIDs="product id list in the product model">
  <ProductName>...</ProductName>
  <ProductName>...</ProductName>
  ...
</ProductModelData>
```
  
 Это запрос, создающий желаемый XML:  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID     AS "@ProductModelID",  
       Name               AS "@ProductModelName",  
      (SELECT ProductID AS "data()"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH ('')) AS "@ProductIDs",  
       (SELECT Name AS "ProductName"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
        FOR XML PATH ('')) AS "ProductNames"  
FROM   Production.ProductModel  
WHERE  ProductModelID= 7 or ProductModelID=9  
FOR XML PATH('ProductModelData');  
```  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
-   Первая вложенная инструкция `SELECT` возвращает список ProductID, для чего использует значение `data()` в качестве имени столбца. Поскольку в режиме `FOR XML PATH`запрос указывает для имени элемента строки пустую строку, формирование элемента не происходит. Вместо этого список значений назначается атрибуту `ProductID` .  
  
-   Вторая вложенная инструкция `SELECT` извлекает названия продуктов для модели продукта. Запрос формирует элементы <`ProductName`>, которые возвращаются в виде элементов <`ProductNames`>, поскольку в качестве имени столбца в запросе указано `ProductNames`.  
  
 Частичный результат:  
  
```
<ProductModelData PId="7" ProductModelName="HL Touring Frame" ProductIDs="885 887 ...">
  <ProductNames>
    <ProductName>HL Touring Frame - Yellow, 60</ProductName>
    <ProductName>HL Touring Frame - Yellow, 46</ProductName>
  </ProductNames>
  ...
</ProductModelData>
<ProductModelData PId="9" ProductModelName="LL Road Frame" ProductIDs="722 723 724 ...">
  <ProductNames>
    <ProductName>LL Road Frame - Black, 58</ProductName>
    <ProductName>LL Road Frame - Black, 60</ProductName>
    <ProductName>LL Road Frame - Black, 62</ProductName>
    ...
  </ProductNames>
</ProductModelData>
```
  
 Вложенный запрос, формирующий имена продуктов, возвращает результат в виде строки, которая преобразуется в сущность и затем добавляется в XML-документ. Если добавить директиву типа `FOR XML PATH (''), type`, вложенный запрос возвращает результат как тип **xml** , а преобразования в сущность не происходит.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT ProductModelID AS "@ProductModelID",  
      Name AS "@ProductModelName",  
      (SELECT ProductID AS "data()"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH ('')  
       ) AS "@ProductIDs",  
       (  
       SELECT Name AS "ProductName"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH (''), type  
       ) AS "ProductNames"  
  
FROM Production.ProductModel  
WHERE ProductModelID= 7 OR ProductModelID=9  
FOR XML PATH('ProductModelData');  
```  
  
## <a name="adding-namespaces-in-the-resulting-xml"></a>Добавление пространств имен в итоговый XML-документ  
 Как описано в разделе [Добавление пространств имен с помощью предложения WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md), предложение WITH XMLNAMESPACES может использоваться для включения пространств имен в запросы в режиме PATH. Например, имена, указанные в предложении SELECT, имеют префиксы пространства имен. Следующий запрос в режиме `PATH` строит XML-документ с пространствами имен.  
  
```  
SELECT 'en'    as "English/@xml:lang",  
       'food'  as "English",  
       'ger'   as "German/@xml:lang",  
       'Essen' as "German"  
FOR XML PATH ('Translation')  
GO  
```  
  
 Атрибут `@xml:lang`, добавляемый к элементу <`English`>, определяется в стандартном пространстве имен xml.  
  
 Результат:  

```
<Translation>
  <English xml:lang="en">food</English>
  <German xml:lang="ger">Essen</German>
</Translation>
```
  
 Следующий запрос похож на приведенный в примере В, отличаясь тем, что в нем пространства имен добавляются в результирующий XML-документ с помощью предложения `WITH XMLNAMESPACES` . Дополнительные сведения с. в разделе [Добавление пространств имен в запросы с помощью WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
```  
USE AdventureWorks2012;  
GO  
WITH XMLNAMESPACES ('uri1' AS ns1,  DEFAULT 'uri2')  
SELECT ProductModelID AS "@ns1:ProductModelID",  
      Name AS "@ns1:ProductModelName",  
      (SELECT ProductID AS "data()"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH ('')  
       ) AS "@ns1:ProductIDs",  
       (  
       SELECT ProductID AS "@ns1:ProductID",   
              Name AS "@ns1:ProductName"  
       FROM   Production.Product  
       WHERE  Production.Product.ProductModelID =   
              Production.ProductModel.ProductModelID  
       FOR XML PATH , type   
       ) AS "ns1:ProductNames"  
FROM Production.ProductModel  
WHERE ProductModelID= 7 OR ProductModelID=9  
FOR XML PATH('ProductModelData'), root('root');  
```  
  
 Результат:  
  
```
<root xmlns="uri2" 
  xmlns:ns1="uri1">
  <ProductModelData ns1:ProductModelID="7" ns1:ProductModelName="HL Touring Frame" ns1:ProductIDs="885 887 888 889 890 891 892 893">
    <ns1:ProductNames>
      <row xmlns="uri2" xmlns:ns1="uri1" ns1:ProductID="885" ns1:ProductName="HL Touring Frame - Yellow, 60" />
      <row xmlns="uri2" xmlns:ns1="uri1" ns1:ProductID="887" ns1:ProductName="HL Touring Frame - Yellow, 46" />
      ...
    </ns1:ProductNames>
  </ProductModelData>
  <ProductModelData ns1:ProductModelID="9" ns1:ProductModelName="LL Road Frame" ns1:ProductIDs="722 723 724 725 726 727 728 729 730 736 737 738">
    <ns1:ProductNames>
      <row xmlns="uri2" xmlns:ns1="uri1" ns1:ProductID="722" ns1:ProductName="LL Road Frame - Black, 58" />
      ...
    </ns1:ProductNames>
  </ProductModelData>
</root>
```
  
## <a name="see-also"></a>См. также:  
 [Использование режима PATH совместно с FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)  
  
  

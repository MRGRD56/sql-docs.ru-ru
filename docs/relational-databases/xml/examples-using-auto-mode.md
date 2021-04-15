---
title: 'Примеры: Использование режима AUTO | Документация Майкрософт'
description: Просмотр примеров запросов, использующих режим AUTO для FOR XML.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- AUTO FOR XML mode, examples
ms.assetid: 11e8d0e4-df8a-46f8-aa21-9602d4f26cad
author: rothja
ms.author: jroth
ms.openlocfilehash: e9224e6c4b64161edbebafb2b2a5ee073dcbcdee
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490844"
---
# <a name="examples-using-auto-mode"></a>Примеры: Использование режима AUTO
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В следующем примере иллюстрируется применение режима AUTO. Многие из этих запросов являются запросами к XML-документам с инструкциями по производству велосипедов, хранящимся в столбце Instructions таблицы ProductModel образца базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] .  
  
## <a name="example-retrieving-customer-order-and-order-detail-information"></a>Пример Извлечение сведений о заказчике, заказе и подробных сведений о заказе  
 Следующий запрос получает данные о заказчике, заказе и подробные данные о заказе определенного заказчика.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Cust.CustomerID,   
       OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       Detail.SalesOrderID, Detail.LineTotal, Detail.ProductID,   
       Product.Name,  
       Detail.OrderQty  
FROM Sales.Customer AS Cust  
INNER JOIN Sales.SalesOrderHeader AS OrderHeader   
    ON Cust.CustomerID = OrderHeader.CustomerID  
INNER JOIN Sales.SalesOrderDetail AS Detail  
    ON OrderHeader.SalesOrderID = Detail.SalesOrderID  
INNER JOIN Production.Product AS Product  
    ON Product.ProductID = Detail.ProductID  
WHERE Cust.CustomerID IN (29672, 29734)  
ORDER BY OrderHeader.CustomerID,  
         OrderHeader.SalesOrderID  
FOR XML AUTO;  
```  
  
 Так как в запросе указываются псевдонимы таблиц `Cust`, `OrderHeader`, `Detail`и `Product` , в режиме `AUTO` формируются соответствующие элементы. С другой стороны, порядок, в котором таблицы идентифицируются столбцами, задаваемыми в предложении `SELECT` , определяет иерархию этих элементов.  
  
 Частичный результат.  
  
 `<Cust CustomerID="29672">`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="43660">`  
  
 `<Detail SalesOrderID="43660" LineTotal="874.794000" ProductID="758" OrderQty="1">`  
  
 `<Product Name="Road-450 Red, 52" />`  
  
 `</Detail>`  
  
 `<Detail SalesOrderID="43660" LineTotal="419.458900" ProductID="762" OrderQty="1">`  
  
 `<Product Name="Road-650 Red, 44" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="47660">`  
  
 `<Detail SalesOrderID="47660" LineTotal="469.794000" ProductID="765" OrderQty="1">`  
  
 `<Product Name="Road-650 Black, 58" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `<OrderHeader CustomerID="29672" SalesOrderID="49857">`  
  
 `<Detail SalesOrderID="49857" LineTotal="44.994000" ProductID="852" OrderQty="1">`  
  
 `<Product Name="Women's Tights, S" />`  
  
 `</Detail>`  
  
 `</OrderHeader>`  
  
 `...`  
  
 `</Cust>`  
  
## <a name="example-specifying-group-by-and-aggregate-functions"></a>Пример Использование предложения GROUP BY и агрегатных функций  
 Следующий запрос возвращает отдельные идентификаторы заказчиков и номера заказов, запрашиваемых заказчиками.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT C.CustomerID, COUNT(*) AS NoOfOrders  
FROM Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
On C.CustomerID = SOH.CustomerID  
GROUP BY C.CustomerID  
FOR XML AUTO;This is the partial result:  
```  
  
 `<I CustomerID="11000" NoOfOrders="3" />`  
  
 `<I CustomerID="11001" NoOfOrders="3" />`  
  
 `...`  
  
## <a name="example-specifying-computed-columns-in-auto-mode"></a>Пример Задание вычисляемых столбцов в режиме AUTO  
 Этот запрос возвращает сцепленные имена заказчиков и данные для заказа. Так как вычисляемый столбец назначен самому внутреннему уровню, встретившемуся на данный момент, в этом примере — элементу <`SOH`>, сцепленные имена заказчиков добавляются в результат как атрибуты элемента <`SOH`>.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT P.FirstName + ' ' + P.LastName AS Name,  
       SOH.SalesOrderID  
FROM Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID  
INNER JOIN Person.Person AS P  
    ON P.BusinessEntityID = C.PersonID  
FOR XML AUTO;  
```  
  
 Частичный результат:  
  
```  
<SOH Name="Jon Yang" SalesOrderID="43793" />  
<SOH Name="Eugene Huang" SalesOrderID="43767" />  
```  
  
 Для получения элементов <`IndividualCustomer`> с атрибутом `Name`, содержащим данные заголовков всех заказов на продажу в качестве подчиненного элемента, запрос переписывается при помощи подзапроса SELECT. Внутренняя выборка создает временную таблицу `IndividualCustomer` с вычисляемым столбцом, содержащим имена отдельных заказчиков. Эта таблица затем соединяется с таблицей `SalesOrderHeader` для получения результата.  
  
 Обратите внимание на то, что в таблице `Sales.Customer` хранятся данные отдельных заказчиков, в том числе значения `PersonID` этих заказчиков. Затем идентификатор `PersonID` используется для поиска контактного лица в таблице `Person.Person` .  
  
```  
SELECT IndividualCustomer.Name, SOH.SalesOrderID  
FROM (SELECT FirstName+ ' '+LastName AS Name, C.PersonID, C.CustomerID  
      FROM Sales.Customer AS C, Person.Person AS P  
      WHERE C.PersonID = P.BusinessEntityID) AS IndividualCustomer  
LEFT OUTER JOIN  Sales.SalesOrderHeader AS SOH  
   ON IndividualCustomer.CustomerID = SOH.CustomerID  
ORDER BY IndividualCustomer.CustomerID, SOH.CustomerIDFOR XML AUTO;  
```  
  
 Частичный результат:  
  
 `<IndividualCustomer Name="Jon Yang">`  
  
 `<SOH SalesOrderID="43793" />`  
  
 `<SOH SalesOrderID="51522" />`  
  
 `<SOH SalesOrderID="57418" />`  
  
 `</IndividualCustomer>`  
  
 `...`  
  
 `...`  
  
## <a name="example-returning-binary-data"></a>Пример Возврат двоичных данных  
 Запрос возвращает фотографию продукта из таблицы `ProductPhoto` . `ThumbNailPhoto` — столбец **varbinary(max)** в таблице `ProductPhoto` . По умолчанию режим `AUTO` возвращает ссылку на двоичные данные, являющуюся относительным URL-адресом виртуального корня базы данных, в которой выполняется запрос. Для идентификации изображения необходимо указать ключевой атрибут `ProductPhotoID` . Как показано в этом примере, при поиске изображения для однозначной идентификации строки в предложении `SELECT` необходимо также указать первичный ключ таблицы.  
  
```  
SELECT ProductPhotoID, ThumbNailPhoto  
FROM   Production.ProductPhoto   
WHERE ProductPhotoID=70  
FOR XML AUTO;  
```  
  
 Результат:  
  
 `-- result`  
  
 `<Production.ProductPhoto`  
  
 `ProductPhotoID="70"`  
  
 `ThumbNailPhoto= "dbobject/Production.ProductPhoto[@ProductPhotoID='70']/@ThumbNailPhoto" />`  
  
 Такой же запрос выполняется с параметром `BINARY BASE64` . Запрос возвращает двоичные данные в формате кодировки base64.  
  
```  
SELECT ProductPhotoID, ThumbNailPhoto  
FROM   Production.ProductPhoto   
WHERE ProductPhotoID=70  
FOR XML AUTO, BINARY BASE64;  
```  
  
 Результат:  
  
 `-- result`  
  
 `<Production.ProductPhoto ProductPhotoID="70" ThumbNailPhoto="Base64 encoded photo" />`  
  
 По умолчанию при использовании режима AUTO для получения двоичных данных вместо самих двоичных данных возвращается ссылка на относительный URL-адрес виртуального корня базы данных, на которой выполняется запрос. Это происходит, если аргумент BINARY BASE64 не задается.  
  
 Если режим AUTO возвращает ссылку на URL-адрес двоичных данных в базах данных, в которых не учитывается регистр, запрос выполняется, даже если задаваемое в запросе имя таблицы или столбца не соответствует имени таблицы или столбца в базе данных. Однако значение регистра, возвращаемое в ссылке, не будет согласованным. Пример:  
  
```  
SELECT ProductPhotoID, ThumbnailPhoto  
FROM   Production.ProductPhoto   
WHERE  ProductPhotoID=70  
FOR XML AUTO;  
```  
  
 Результат:  
  
 `<Production.PRODUCTPHOTO`  
  
 `PRODUCTPHOTOID="70"`  
  
 `THUMBNAILPHOTO= "dbobject/Production.PRODUCTPHOTO[@ProductPhotoID='70']/@ThumbNailPhoto" />`  
  
 Это может быть проблемой, особенно если запросы на объекты базы данных выполняются для базы данных с учетом регистра. Во избежание этого регистр задаваемого в запросах имени таблицы или столбца должен соответствовать регистру имени таблицы или столбца в базе данных.  
  
## <a name="example-understanding-the-encoding"></a>Пример Основные сведения о кодировке  
 В этом примере демонстрируется использование различных кодировок, которые могут применяться в результате.  
  
 Создайте такую таблицу:  
  
```  
CREATE TABLE [Special Chars] (Col1 char(1) primary key, [Col#&2] varbinary(50));  
```  
  
 Добавьте в таблицу следующие данные:  
  
```  
INSERT INTO [Special Chars] VALUES ('&', 0x20), ('#', 0x20);  
```  
  
 Следующий запрос возвращает данные из таблицы. Задается режим FOR XML AUTO. Двоичные данные возвращаются в виде ссылки.  
  
```  
SELECT * FROM [Special Chars] FOR XML AUTO;  
```  
  
 Результат:  
  
 `<Special_x0020_Chars`  
  
 `Col1="#"`  
  
 `Col_x0023__x0026_2="dbobject/Special_x0020_Chars[@Col1='#']/@Col_x0023__x0026_2"`  
  
 `/>`  
  
 `<Special_x0020_Chars`  
  
 `Col1="&"`  
  
 `Col_x0023__x0026_2="dbobject/Special_x0020_Chars[@Col1='&']/@Col_x0023__x0026_2"`  
  
 `/>`  
  
 Далее показан процесс кодирования специальных символов в результате.  
  
-   В результате запроса специальные символы для XML и URL-адреса в возвращаемых именах элемента и атрибута кодируются при помощи шестнадцатеричного значения соответствующего символа Юникода. В предыдущем результате имя элемента <`Special Chars`> возвращается как <`Special_x0020_Chars`>. Имя атрибута <`Col#&2`> возвращается в виде <`Col_x0023__x0026_2`>. Кодируются специальные символы для XML, и URL-адресов.  
  
-   Если значения элементов или атрибутов содержат какие-либо из пяти стандартных сущностей символов XML (', "", \<, > и &), эти специальные символы XML всегда кодируются при помощи кодировки символов XML. В предыдущем результате значение `&` в значении атрибута <`Col1`> кодируется как `&`. Однако символ # остается #, так как это допустимый, а не специальный символ XML.  
  
-   Если значения элементов или атрибутов содержат специальные символы для URL-адреса, имеющие особый смысл, они кодируются только в значении DBOBJECT URL, и только если специальный символ является частью имени таблицы или столбца. В результате символ `#` , являющийся частью имени таблицы `Col#&2` , кодируется как `_x0023_ in the DBOJBECT URL`.  
  
## <a name="see-also"></a>См. также:  
 [Использование AUTO Mode с FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)  
  
  

---
title: Поддержка FOR XML для типа данных XML | Документация Майкрософт
description: Сведения об использовании запросов FOR XML к столбцам базы данных SQL типа данных XML.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], XML
- xml data type [SQL Server], FOR XML clause
ms.assetid: 365de07d-694c-4c8b-b671-8825be27f87c
author: rothja
ms.author: jroth
ms.openlocfilehash: 588771aa272ec10c9b2654e40bd71fe6e0ccbd4b
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490876"
---
# <a name="for-xml-support-for-the-xml-data-type"></a>Поддержка FOR XML для XML-данных
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Если в предложении SELECT запроса FOR XML указан **xml** -столбец, значения столбца сопоставляются как элементы в возвращенном коде XML, независимо от того, указана ли директива ELEMENTS. XML-декларации в **xml** -столбце не сериализуются.  
  
 Например, приведенный ниже запрос извлекает контактные данные заказчика, указанные в столбцах `BusinessEntityID`, `FirstName`и `LastName` , а также телефонные номера в столбце `AdditionalContactInfo` типа **xml** .  
  
```  
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
') AS PhoneNumber  
FROM Person.Person  
WHERE AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
')IS NOT NULL  
FOR XML AUTO, TYPE;  
```  
  
 Поскольку запрос не содержит директивы ELEMENTS, значения столбцов возвращаются в виде атрибутов. Исключения составляют значения дополнительных сведений о контактах, полученные из **xml** -столбца. Эти значения возвращаются в виде элементов.  
  
 Частичный результат:  
  
 `<Person.Person BusinessEntityID="291" FirstName="Gustavo" LastName="Achong">`  
  
 `<PhoneNumber>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1112</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1111</act:number>`  
  
 `</PhoneNumber>`  
  
 `</Person.Person>`  
  
 `<Person.Person BusinessEntityID="293" FirstName="Catherine" LastName="Abel">`  
  
 `<PhoneNumber>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-2222</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-1234</act:number>`  
  
 `</PhoneNumber>`  
  
```  
</Person.Person>  
...  
```  
  
 Если для XML-столбца, сформированного средствами языка XQuery, указывается псевдоним, этот псевдоним используется для добавления элемента оболочки вокруг кода XML, сформированного с помощью XQuery. Так, в следующем запросе в качестве псевдонима столбца указывается `MorePhoneNumbers` :  
  
```  
SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
') AS PhoneNumber  
FROM Person.Person  
WHERE AdditionalContactInfo.query('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
')IS NOT NULL  
FOR XML AUTO, TYPE;  
```  
  
 Код XML, возвращаемый средствами XQuery, упаковывается в элемент <`MorePhoneNumbers`>, как показано в следующем частичном результирующем наборе:  
  
 `<Person.Person BusinessEntityID="291" FirstName="Gustavo" LastName="Achong">`  
  
 `<MorePhoneNumbers>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1112</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">425-555-1111</act:number>`  
  
 `</MorePhoneNumbers>`  
  
 `</Person.Person>`  
  
 `<Person.Person BusinessEntityID="293" FirstName="Catherine" LastName="Abel">`  
  
 `<MorePhoneNumbers>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-2222</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">206-555-1234</act:number>`  
  
 `</MorePhoneNumbers>`  
  
```  
</Person.Person>  
...  
```  
  
 При указании в запросе директивы ELEMENTS в полученном после его выполнения коде XML значения BusinessEntityID, LastName и FirstName будут возвращены в виде элементов.  
  
 Как видно из следующего примера, процедура обработки FOR XML не сериализует XML-декларации в XML-данных из столбца типа **xml** :  
  
```  
create table t(i int, x xml)  
go  
insert into t values(1, '<?xml version="1.0" encoding="UTF-8" ?>  
                             <Root SomeID="10" />')  
select i, x  
from   t  
for xml auto;  
```  
  
 Результат. В результате XML-декларация <`?xml version="1.0" encoding="UTF-8" ?`> является несериализованной.  
  
```  
<root>  
  <t i="1">  
    <x>  
      <Root SomeID="10" />  
    </x>  
  </t>  
</root>  
```  
  
## <a name="returning-xml-from-a-user-defined-function"></a>Возвращение XML-данных из пользовательской функции  
 Запросы FOR XML можно использовать для возвращения XML-данных из определяемой пользователем функции, которая возвращает один из следующих объектов:  
  
-   таблицу с одним **xml** -столбцом;  
  
-   экземпляр **xml** -типа.  
  
 Так, следующая пользовательская функция возвращает таблицу с одним столбцом типа **xml**:  
  
```  
USE AdventureWorks2012;  
GO  
CREATE FUNCTION dbo.MyUDF (@ProudctModelID int)  
RETURNS @T TABLE  
  (  
     ProductDescription xml  
  )  
AS  
BEGIN  
  INSERT @T  
     SELECT CatalogDescription.query('  
declare namespace PD="https://www.adventure-works.com/schemas/products/description";  
                    //PD:ProductDescription  ')  
     FROM Production.ProductModel  
     WHERE ProductModelID = @ProudctModelID  
  RETURN  
END;  
```  
  
 Можно выполнять определяемую пользователем функцию и отправляет запросы к возвращаемой ею таблице. В следующем примере XML-значение, возвращенное в результате запроса к таблице, присваивается **xml** -переменной.  
  
```  
declare @x xml;  
set @x = (SELECT * FROM MyUDF(19));  
select @x;  
```  
  
 Вот еще один пример определяемой пользователем функции. Эта пользовательская функция возвращает экземпляр типа **xml** . В следующем примере определяемая пользователем функция возвращает типизированный экземпляр XML, поскольку здесь указано пространство имен схемы.  
  
```  
DROP FUNCTION dbo.MyUDF;  
GO  
CREATE FUNCTION MyUDF (@ProductModelID int)   
RETURNS xml ([Production].[ProductDescriptionSchemaCollection])  
AS  
BEGIN  
  declare @x xml  
  set @x =   ( SELECT CatalogDescription  
          FROM Production.ProductModel  
          WHERE ProductModelID = @ProductModelID )  
  return @x  
END;  
```  
  
 Возвращенный пользовательской функцией код XML может быть назначен переменной типа **xml** следующим образом:  
  
```  
declare @x xml;  
SELECT @x= dbo.MyUDF4 (19) ;  
select @x;  
```  
  
## <a name="see-also"></a>См. также:  
 [Поддержка FOR XML для различных типов данных SQL Server](../../relational-databases/xml/for-xml-support-for-various-sql-server-data-types.md)  
  
  

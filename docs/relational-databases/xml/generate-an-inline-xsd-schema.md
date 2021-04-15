---
title: Создание встроенных схем XSD | Документация Майкрософт
description: Узнайте, как создать встроенную XSD-схему с помощью параметра XMLSCHEMA в предложении FOR XML SQL-запроса.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XSD schemas [SQL Server]
- XMLSCHEMA option
- schemas [SQL Server], XML
- XDR schemas
- FOR XML clause, inline XSD schema generation
- inline XSD schema generation [SQL Server]
- XMLDATA option
ms.assetid: 04b35145-1cca-45f4-9eb7-990abf2e647d
author: rothja
ms.author: jroth
ms.openlocfilehash: d6f4282d9c134caf25052affc3132a2212e1fa49
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488244"
---
# <a name="generate-an-inline-xsd-schema"></a>Создание встроенных схем XSD
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В предложении FOR XML можно запросить, чтобы запрос возвращал встроенную схему вместе с результатами запроса. Если нужно получить XDR-схему, то в предложении FOR XML следует использовать ключевое слово XMLDATA. Если нужно получить XSD-схему, то тогда следует использовать ключевое слово XMLSCHEMA.  
  
 В этом разделе описывается ключевое слово XMLSCHEMA и объясняется структура результирующей встроенной XSD-схемы. Далее приведены ограничения, возникающие при запросе встроенных схем.  
  
-   Параметр XMLSCHEMA можно задать только в режимах RAW и AUTO, этого нельзя сделать в режиме EXPLICIT.  
  
-   Если во вложенном запросе FOR XML определена директива TYPE, результат запроса будет иметь тип данных **xml** и передаваться за экземпляр нетипизированных XML-данных. Дополнительные сведения см в разделе [Данные XML (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md).  
  
 Если в запросе FOR XML задан параметр XMLSCHEMA, то в результат запроса входит и схема, и XML-данные. Каждый элемент данных высшего уровня ссылается на предыдущую схему посредством заданного по умолчанию объявления пространства имен, которое, в свою очередь, ссылается на целевое пространство имен встроенной схемы.  
  
 Пример:  
  
```  
<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:schema="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">  
  <xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />  
  <xsd:element name="Production.ProductModel">  
    <xsd:complexType>  
      <xsd:attribute name="ProductModelID" type="sqltypes:int" use="required" />  
      <xsd:attribute name="Name" use="required">  
        <xsd:simpleType sqltypes:sqlTypeAlias="[AdventureWorks2012].[dbo].[Name]">  
          <xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033" sqltypes:sqlCompareOptions="IgnoreCase IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">  
            <xsd:maxLength value="50" />  
          </xsd:restriction>  
        </xsd:simpleType>  
      </xsd:attribute>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
<Production.ProductModel xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" ProductModelID="1" Name="Classic Vest" />  
```  
  
 В результат входит XML-схема и XML-результат. Элемент результата высшего уровня <`ProductModel`> ссылается на схему посредством заданного по умолчанию объявления пространства имен, xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1".  
  
 В присутствующей в результате схеме может находиться множество документов схемы, описывающих различные пространства имен. Возвращаться должны, как минимум, следующие две схемы.  
  
-   Один документ схемы для пространства имен **Sqltypes** , для которого возвращаются базовые типы SQL.  
  
-   Другой документ схемы, описывающий форму результата запроса FOR XML.  
  
 Кроме того, если в результат запроса входят какие-либо типизированные типы данных **xml** , то возвращаются схемы, связанные с этими типами данных **xml** .  
  
 Целевое пространство имен документа схемы, описывающее форму результата FOR XML, содержит фиксированную часть и числовую часть, которая автоматически увеличивается. Далее показан формат данного пространства имен, где *n* является положительным целым числом. Например, в предыдущем запросе целевым пространством имен было urn:schemas-microsoft-com:sql:SqlRowSet1.  
  
```  
urn:schemas-microsoft-com:sql:SqlRowSetn  
```  
  
 Изменения в целевых пространствах имен в результате, произошедшем между двумя выполнениями, могут быть нежелательны. Например, если запрашивается результирующий XML, то при изменении в целевых пространствах имен необходимо обновить запрос. При необходимости можно задать целевое пространство имен, когда в предложение FOR XML добавлен параметр XMLSCHEMA. Результирующий XML будет содержать указанное пространство имен и оставаться неизменным независимо от того, сколько раз выполняли запрос.  
  
```  
SELECT ProductModelID, Name  
FROM   Production.ProductModel  
WHERE ProductModelID=1  
FOR XML AUTO, XMLSCHEMA ('MyURI')  
```  
  
## <a name="entity-elements"></a>Элементы сущности  
 Для описания особенностей создания структуры XSD-схемы для результата запроса необходимо вначале привести общие сведения про элемент сущности.  
  
 Элемент сущности XML-данных, возвращаемых запросом FOR XML, является элементом, создаваемым таблицей, а не столбцом. Например, следующий запрос FOR XML возвращает контактную информацию из таблицы `Person` базы данных `AdventureWorks2012` .  
  
```  
SELECT BusinessEntityID, FirstName  
FROM Person.Person  
WHERE BusinessEntityID = 1  
FOR XML AUTO, ELEMENTS  
```  
  
 Результат:  
  
 `<Person>`  
  
 `<BusinessEntityID>1</BusinessEntityID>`  
  
 `<FirstName>Ken</FirstName>`  
  
 `</Person>`  
  
 В этом результате элементом сущности является <`Person`>. В XML-результате может быть множество элементов сущности, и каждый из них должен обладать глобальным объявлением во встроенной XSD-схеме. Например, следующий запрос извлекает из заголовка сведения о заказах на продажу, а также получает подробные данные о конкретном заказе.  
  
```  
SELECT  SalesOrderHeader.SalesOrderID, ProductID, OrderQty  
FROM    Sales.SalesOrderHeader, Sales.SalesOrderDetail  
WHERE   SalesOrderHeader.SalesOrderID = SalesOrderDetail.SalesOrderID  
AND     SalesOrderHeader.SalesOrderID=5001  
FOR XML AUTO, ELEMENTS, XMLSCHEMA  
```  
  
 Из-за того, что в запрос добавлена директива ELEMENTS, XML-результат формируется с использованием элементов. Также в этом запросе задана директива XMLSCHEMA, поэтому возвращается встроенная XSD-схема. Результат:  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:schema="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />`  
  
 `<xsd:element name="Sales.SalesOrderHeader">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="SalesOrderID" type="sqltypes:int" />`  
  
 `<xsd:element ref="schema:Sales.SalesOrderDetail" minOccurs="0" maxOccurs="unbounded" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `<xsd:element name="Sales.SalesOrderDetail">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" />`  
  
 `<xsd:element name="OrderQty" type="sqltypes:smallint" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
-   В этом результате элементами сущности являются <`SalesOrderHeader`> и <`SalesOrderDetail`>. Из-за этого они глобально объявлены в схеме. То есть объявление появляется на высшем уровне элемента <`Schema`>.  
  
-   <`SalesOrderID`>, <`ProductID`> и <`OrderQty`> не являются элементами сущности, так как они сопоставлены столбцам. Из-за директивы ELEMENTS данные столбца возвращаются в виде элементов в XML. Они сопоставлены локальным элементам сложного типа элемента сущности. Заметьте, что если директива ELEMENTS не задана, то значения `SalesOrderID`, `ProductID` и `OrderQty` сопоставляются с локальными атрибутами соответствующего сложного типа элемента сущности.  
  
## <a name="attribute-name-clashes"></a>Конфликты имен атрибутов  
 В следующих рассуждениях используются таблицы `CustOrder` и `CustOrderDetail` . Для проверки следующих образцов создайте такие же таблицы и добавьте собственные образцы данных:  
  
```  
CREATE TABLE CustOrder (OrderID int primary key, CustomerID int)  
GO  
CREATE TABLE CustOrderDetail (OrderID int, ProductID int, Qty int)  
GO  
```  
  
 В предложении FOR XML одинаковые имена иногда соответствуют различным свойствам и атрибутам. Например, следующий атрибутивный запрос режима RAW создает два атрибута с одним и тем же именем — OrderID. Это приводит к ошибке.  
  
```  
SELECT CustOrder.OrderID,   
       CustOrderDetail.ProductID,   
       CustOrderDetail.OrderID  
FROM   dbo.CustOrder, dbo.CustOrderDetail  
WHERE  CustOrder.OrderID = CustOrderDetail.OrderID  
FOR XML RAW, XMLSCHEMA  
```  
  
 Однако так как двум элементам допустимо иметь одно и то же имя, можно решить проблему добавлением директивы ELEMENTS:  
  
```  
SELECT CustOrder.OrderID,  
       CustOrderDetail.ProductID,   
       CustOrderDetail.OrderID  
from   dbo.CustOrder, dbo.CustOrderDetail  
where  CustOrder.OrderID = CustOrderDetail.OrderID  
FOR XML RAW, XMLSCHEMA, ELEMENTS  
```  
  
 Результат. Обратите внимание, что во встроенной XSD-схеме элемент OrderID определяется два раза. В одной из деклараций атрибуту minOccurs присваивается значение 0, соответствующее столбцу OrderID таблицы CustOrderDetail, во второй декларации устанавливается соответствие первичному ключевому столбцу OrderID таблицы `CustOrder` , и этот атрибут имеет значение 1 по умолчанию.  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" schemaLocation="https://schemas.microsoft.com/sqlserver/2004/sqltypes/sqltypes.xsd" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="OrderID" type="sqltypes:int" />`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" minOccurs="0" />`  
  
 `<xsd:element name="OrderID" type="sqltypes:int" minOccurs="0" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
## <a name="element-name-clashes"></a>Конфликты имен элементов  
 В предложении FOR XML одинаковые имена иногда соответствуют различным подэлементам. Например, следующий запрос получает значения продуктов ListPrice и DealerPrice, но определяет для этих двух столбцов один и тот же псевдоним Price. Поэтому в результирующем наборе строк будут присутствовать два столбца с одинаковым именем.  
  
### <a name="case-1-both-subelements-are-nonkey-columns-of-the-same-type-and-can-be-null"></a>Вариант 1. Оба подэлемента являются неключевыми столбцами одинакового типа и могут иметь значение NULL  
 В следующем запросе оба подэлемента являются неключевыми столбцами одинакового типа и могут иметь значение NULL.  
  
```  
DROP TABLE T  
go  
CREATE TABLE T (ProductID int primary key, ListPrice money, DealerPrice money)  
go  
INSERT INTO T values (1, 1.25, null)  
go  
  
SELECT ProductID, ListPrice Price, DealerPrice Price  
FROM   T  
for    XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Это получаемый в результате XML. Показана только часть встроенной XSD:  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" minOccurs="0" maxOccurs="2" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<ProductID>1</ProductID>`  
  
 `<Price>1.2500</Price>`  
  
 `</row>`  
  
 Обратите внимание на следующее во встроенной XSD-схеме.  
  
-   Оба значения, ListPrice и DealerPrice, имеют один и тот же тип данных, `money`, и оба могут иметь в таблице значение NULL. Таким образом, из-за того, что они могут не возвращаться в результирующем XML, в декларации сложного типа элемента <`row`> может быть только один дочерний элемент <`Price`> с параметрами minOccurs=0 и maxOccurs=2.  
  
-   В результате из-за того, что значение `DealerPrice` в таблице равно NULL, в качестве элемента <`Price`> возвращается только значение `ListPrice`. При добавлении параметра `XSINIL` к директиве ELEMENTS будут получены оба элемента со значением `xsi:nil`, равным TRUE, для элемента <`Price`>, соответствующего DealerPrice. Также будут получены два дочерних элемента <`Price`> в определении сложного типа <`row`> во встроенной схеме XSD. Для обоих атрибут `nillable` будет установлен в значение TRUE. Это промежуточный результат:  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="ProductID" type="sqltypes:int" nillable="1" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" nillable="1" />`  
  
 `<xsd:element name="Price" type="sqltypes:money" nillable="1" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`  
  
 `<ProductID>1</ProductID>`  
  
 `<Price>1.2500</Price>`  
  
 `<Price xsi:nil="true" />`  
  
 `</row>`  
  
### <a name="case-2-one-key-and-one-nonkey-column-of-the-same-type"></a>Вариант 2. Один ключевой столбец и один неключевой столбец одинакового типа  
 В следующем запросе находится один ключевой столбец и один неключевой столбец одинакового типа.  
  
```  
CREATE TABLE T (Col1 int primary key, Col2 int, Col3 nvarchar(20))  
go  
INSERT INTO T VALUES (1, 1, 'test')  
go   
```  
  
 Следующий запрос по отношению к таблице **T** задает один и тот же псевдоним для Col1 и Col2, где Col1 является столбцом первичного ключа и не может быть нулевым, а Col2 может быть нулевым. Это приводит к созданию двух элементов, являющихся дочерними элементами элемента <`row`>.  
  
```  
SELECT Col1 as Col, Col2 as Col, Col3  
FROM T  
FOR XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 Результат. Показана только часть встроенной XSD-схемы:  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="Col" type="sqltypes:int" />`  
  
 `<xsd:element name="Col" type="sqltypes:int" minOccurs="0" />`  
  
 `<xsd:element name="Col3" minOccurs="0">`  
  
 `<xsd:simpleType>`  
  
 `<xsd:restriction base="sqltypes:nvarchar"`  
  
 `sqltypes:localeId="1033"`  
  
 `sqltypes:sqlCompareOptions="IgnoreCase`  
  
 `IgnoreKanaType IgnoreWidth"`  
  
 `sqltypes:sqlSortId="52">`  
  
 `<xsd:maxLength value="20" />`  
  
 `</xsd:restriction>`  
  
 `</xsd:simpleType>`  
  
 `</xsd:element>`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<Col>1</Col>`  
  
 `<Col>1</Col>`  
  
 `<Col3>test</Col3>`  
  
 `</row>`  
  
 Обратите внимание, что во встроенной XSD-схеме элемент <`Col`> соответствует столбцу Col2 с параметром minOccurs, равным 0.  
  
### <a name="case-3-both-elements-of-different-types-and-corresponding-columns-can-be-null"></a>Случай 3. Оба элемента разных типов и соответствующие столбцы могут иметь значение NULL  
 Для случая 2 показан следующий запрос, заданный относительно таблицы-образца:  
  
```  
SELECT Col1, Col2 as Col, Col3 as Col  
FROM T  
FOR XML RAW, ELEMENTS, XMLSCHEMA  
```  
  
 В этом запросе Col2 и Col3 обладают одинаковыми псевдонимами. Это приводит к появлению двух элементов с одинаковыми именами, являющихся дочерними элементами элемента <`raw`> в результате. Оба столбца принадлежат различным типам и могут иметь значение NULL. Результат. Показана только часть встроенной XSD-схемы.  
  
 `...`  
  
 `<xsd:schema targetNamespace="urn:schemas-microsoft-com:sql:SqlRowSet1" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:sqltypes="https://schemas.microsoft.com/sqlserver/2004/sqltypes" elementFormDefault="qualified">`  
  
 `<xsd:import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes" />`  
  
 `<xsd:simpleType name="Col1">`  
  
 `<xsd:restriction base="sqltypes:int" />`  
  
 `</xsd:simpleType>`  
  
 `<xsd:simpleType name="Col2">`  
  
 `<xsd:restriction base="sqltypes:nvarchar" sqltypes:localeId="1033"`  
  
 `sqltypes:sqlCompareOptions="IgnoreCase`  
  
 `IgnoreKanaType IgnoreWidth" sqltypes:sqlSortId="52">`  
  
 `<xsd:maxLength value="20" />`  
  
 `</xsd:restriction>`  
  
 `</xsd:simpleType>`  
  
 `<xsd:element name="row">`  
  
 `<xsd:complexType>`  
  
 `<xsd:sequence>`  
  
 `<xsd:element name="Col1" type="sqltypes:int" />`  
  
 `<xsd:element name="Col" minOccurs="0" maxOccurs="2" type="xsd:anySimpleType" />`  
  
 `</xsd:sequence>`  
  
 `</xsd:complexType>`  
  
 `</xsd:element>`  
  
 `</xsd:schema>`  
  
 `<row xmlns="urn:schemas-microsoft-com:sql:SqlRowSet1">`  
  
 `<Col1>1</Col1>`  
  
 `<Col xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`  
  
 `xsi:type="Col1">1</Col>`  
  
 `<Col xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`  
  
 `xsi:type="Col2">test</Col>`  
  
 `</row>`  
  
 Обратите внимание на следующее во встроенной XSD-схеме.  
  
-   Так как Col2 и Col3 могут иметь значение NULL, то объявление элемента <`Col`> определяет minOccurs равным 0, а maxOccurs равным 2.  
  
-   Из-за того, что элементы <`Col`> имеют общего родителя, в схеме присутствует только одно объявление элемента. Также из-за того, что оба элемента относятся к различным типам (хотя оба эти типа простые), типом элемента в схеме является `xsd:anySimpleType`. В результате тип каждого экземпляра определяется атрибутом `xsi:type` .  
  
-   В результате каждый экземпляр элемента <`Col`> ссылается на свой тип экземпляра при помощи атрибута `xsi:type`.  
  
  

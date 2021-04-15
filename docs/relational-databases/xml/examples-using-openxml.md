---
title: 'Примеры: Использование инструкции OPENXML | Документация Майкрософт'
description: Ознакомьтесь с примерами использования инструкции OPENXML для создания представления набора строк XML-документа.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- ColPattern [XML in SQL Server]
- XML [SQL Server], mapping data
- OPENXML statement, about OPENXML statement
- overflow in XML document [SQL Server]
- mapping XML data [SQL Server]
- combining attribute-centric and element centric mapping
- unconsumed data
- attribute-centric mapping
- column patterns [XML in SQL Server]
- XML [SQL Server], overflow handling
- row patterns [XML in SQL Server]
- rowpattern [XML in SQL Server]
- flags parameter
- element-centric mapping [SQL Server]
- edge tables
ms.assetid: 689297f3-adb0-4d8d-bf62-cfda26210164
author: rothja
ms.author: jroth
ms.openlocfilehash: 6b16bd6ed6fd7499109e9891725766d6934811a1
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488481"
---
# <a name="examples-using-openxml"></a>Примеры: Использование OPENXML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Примеры в этом подразделе иллюстрируют использование инструкции OPENXML для создания представления наборов строк XML-документа. Дополнительные сведения о синтаксисе инструкции OPENXML см. в разделе [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md). Примеры показывают все аспекты инструкции OPENXML, но не определяют метасвойства в ней. Дополнительные сведения о том, как использовать метасвойства в OPENXML, см. в статье [Определение метасвойств в инструкции OPENXML](../../relational-databases/xml/specify-metaproperties-in-openxml.md).  
  
## <a name="examples"></a>Примеры  
 При получении данных шаблон *rowpattern* используется для определения узлов в XML-документе, которые определяют строки. Кроме того, шаблон *rowpattern* выражен на языке шаблонов XPath, который используется в реализации языка XPath в MSXML. Например, если шаблон заканчивается элементом или атрибутом, то строка создается для каждого узла элемента или атрибута, который выбран шаблоном *rowpattern*.  
  
 Значение параметра *flags* предоставляет сопоставление по умолчанию. Если параметр *ColPattern* не задан в элементе *SchemaDeclaration*, то предполагается сопоставление, указанное в параметре *flags* . Значение параметра *flags* игнорируется, если параметр *ColPattern* определен в элементе *SchemaDeclaration*. Задание параметра *ColPattern* определяет атрибутивное или элементное сопоставление, а также характер обработки переполнения и невостребованных данных.  
  
### <a name="a-executing-a-simple-select-statement-with-openxml"></a>A. Выполнение простой инструкции SELECT с предложением OPENXML  
 XML-документ в этом примере состоит из элементов <`Customer`>, <`Order`> и <`OrderDetail`>. Инструкция OPENXML получает из XML-документа сведения о заказчике в наборе строк из двух столбцов — **CustomerID** и **ContactName**.  
  
 Сначала вызывается хранимая процедура **sp_xml_preparedocument** , чтобы получить дескриптор документа. Дескриптор документа передается инструкции OPENXML.  
  
 Инструкция OPENXML иллюстрирует следующее:  
  
-   шаблон *rowpattern* (/ROOT/Customer) определяет, что следует обрабатывать узлы <`Customer`>;  
  
-   параметр *flags* имеет значение **1** , которое указывает на сопоставление с использованием атрибутивной модели; В результате XML-атрибуты сопоставляются со столбцами в наборе строк, определенном в элементе *SchemaDeclaration*;  
  
-   в элементе *SchemaDeclaration* предложения WITH заданные значения параметра *ColName* совпадают с соответствующими именами XML-атрибутов. Поэтому параметр *ColPattern* не указывается в элементе *SchemaDeclaration*.  
  
 Затем инструкция SELECT извлекает все столбцы из набора строк, предоставленного инструкцией OPENXML:  
  
```  
DECLARE @DocHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @DocHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@DocHandle, '/ROOT/Customer',1)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @DocHandle  
```  
  
 Результат:  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Поскольку элементы <`Customer`> не имеют подэлементов, то выполнение той же самой инструкции SELECT с параметром *flags* со значением **2**, которое указывает на сопоставление с использованием элементов, возвращает значения NULL столбцов **CustomerID** и **ContactName** для обоих пользователей.  
  
 Аргумент \@xmlDocument может также иметь тип **xml** или **(n)varchar(max)** .  
  
 Если <`CustomerID`> и <`ContactName`> в XML-документе являются подэлементами, то сопоставление с использованием элементов возвращает значения:  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer>  
   <CustomerID>VINET</CustomerID>  
   <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer>     
   <CustomerID>LILAS</CustomerID>  
   <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT    *  
FROM      OPENXML (@XmlDocumentHandle, '/ROOT/Customer',2)  
           WITH (CustomerID  varchar(10),  
                 ContactName varchar(20))  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 Результат:  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Обратите внимание, что дескриптор документа, возвращенного хранимой процедурой **sp_xml_preparedocument** , действителен в течение выполнения пакета, но не в течение сеанса.  
  
### <a name="b-specifying-colpattern-for-mapping-between-rowset-columns-and-the-xml-attributes-and-elements"></a>Б. Задание параметра ColPattern для сопоставления столбцов набора строк с XML-атрибутами и элементами  
 Данный пример показывает, как задается шаблон XPath в необязательном параметре *ColPattern* для сопоставления столбцов набора строк с XML-атрибутами и элементами.  
  
 XML-документ в этом примере состоит из элементов <`Customer`>, <`Order`> и <`OrderDetail`>. Инструкция OPENXML получает из XML-документа сведения о заказчике и заказе в виде набора строк (**CustomerID**, **OrderDate**, **ProdID** и **Qty**).  
  
 Сначала вызывается хранимая процедура **sp_xml_preparedocument** , чтобы получить дескриптор документа. Дескриптор документа передается инструкции OPENXML.  
  
 Инструкция OPENXML иллюстрирует следующее:  
  
-   шаблон *rowpattern* (/ROOT/Customer/Order) определяет, что следует обрабатывать узлы элемента <`OrderDetail`>.  
  
 В примере параметр *flags* имеет значение **2** , которое указывает на сопоставление с использованием элементов. Однако сопоставление, указанное в параметре *ColPattern* , перекрывает данное сопоставление. То есть шаблон XPath, заданный в параметре *ColPattern* , сопоставляет столбцы набора строк с атрибутами. Результатом является атрибутивное сопоставление.  
  
 В элементе *SchemaDeclaration* предложения WITH параметр *ColPattern* также задается параметрами *ColName* и *ColType* . Необязательный параметр *ColPattern* является заданным шаблоном XPath и указывает следующее:  
  
-   Столбцы **OrderID**, **CustomerID** и **OrderDate** в наборе строк сопоставляются с атрибутами родителя узлов, заданных шаблоном *rowpattern*, а шаблон *rowpattern* определяет узлы <`OrderDetail`>. Следовательно, столбцы **CustomerID** и **OrderDate** сопоставляются с атрибутами **CustomerID** и **OrderDate** элемента <`Order`>;  
  
-   столбцы **ProdID** и **Qty** в наборе строк сопоставляются с атрибутами **ProductID** и **Quantity** узлов, заданных в шаблоне *rowpattern*.  
  
 Затем инструкция SELECT извлекает все столбцы из набора строк, предоставленного инструкцией OPENXML:  
  
```  
DECLARE @XmlDocumentHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @XmlDocumentHandle OUTPUT, @XmlDocument  
-- Execute a SELECT stmt using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@XmlDocumentHandle, '/ROOT/Customer/Order/OrderDetail',2)  
WITH (OrderID     int         '../@OrderID',  
      CustomerID  varchar(10) '../@CustomerID',  
      OrderDate   datetime    '../@OrderDate',  
      ProdID      int         '@ProductID',  
      Qty         int         '@Quantity')  
EXEC sp_xml_removedocument @XmlDocumentHandle  
```  
  
 Результат:  
  
```  
OrderID CustomerID        OrderDate          ProdID    Qty  
-------------------------------------------------------------  
10248    VINET     1996-07-04 00:00:00.000     11       12  
10248    VINET     1996-07-04 00:00:00.000     42       10  
10283    LILAS     1996-08-16 00:00:00.000     72        3  
```  
  
 Шаблон XPath, заданный как параметр *ColPattern* , может также быть указан для сопоставления XML-элементов со столбцами набора строк. Результатом является сопоставление с использованием атрибутов. В следующем примере элементы <`CustomerID`> и <`OrderDate`> XML-документа являются подэлементами элемента <`Orders`>. Поскольку параметр *ColPattern* перезаписывает сопоставление, заданное в параметре *flags* , то параметр *flags* не указывается в инструкции OPENXML:  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order EmployeeID="5" >  
      <OrderID>10248</OrderID>  
      <CustomerID>VINET</CustomerID>  
      <OrderDate>1996-07-04T00:00:00</OrderDate>  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order  EmployeeID="3" >  
      <OrderID>10283</OrderID>  
      <CustomerID>LILAS</CustomerID>  
      <OrderDate>1996-08-16T00:00:00</OrderDate>  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail')  
WITH (CustomerID  varchar(10)   '../CustomerID',  
      OrderDate   datetime      '../OrderDate',  
      ProdID      int           '@ProductID',  
      Qty         int           '@Quantity')  
EXEC sp_xml_removedocument @docHandle  
```  
  
### <a name="c-combining-attribute-centric-and-element-centric-mapping"></a>В. Совместное применение атрибутивной и элементной моделей сопоставления  
 В этом примере параметр *flags* имеет значение **3** и указывает на применение как атрибутивного, так и элементного сопоставления. В этом случае сначала применяется атрибутивное сопоставление, а затем элементное сопоставление для всех столбцов, которые еще не обработаны:  
  
```  
DECLARE @docHandle int  
DECLARE @XmlDocument nvarchar(1000)  
SET @XmlDocument =N'<ROOT>  
<Customer CustomerID="VINET"  >  
     <ContactName>Paul Henriot</ContactName>  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
          OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" >   
     <ContactName>Carlos Gonzlez</ContactName>  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
          OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @XmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer',3)  
      WITH (CustomerID  varchar(10),  
            ContactName varchar(20))  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Результат:  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Атрибутивное сопоставление применяется к столбцу **CustomerID**. Атрибут **ContactName** отсутствует в элементе <`Customer`>. поэтому применяется элементное сопоставление.  
  
### <a name="d-specifying-the-text-xpath-function-as-colpattern"></a>Г. Задание функции text() языка XPath вместо параметра ColPattern  
 XML-документ в этом примере состоит из элементов <`Customer`> и <`Order`>. Инструкция OPENXML возвращает набор строк, который состоит из атрибута **oid** элемента <`Order`>, идентификатора родителя узла, заданного шаблоном *rowpattern*, и строки конечных значений содержимого элемента.  
  
 Сначала вызывается хранимая процедура **sp_xml_preparedocument** , чтобы получить дескриптор документа. Дескриптор документа передается инструкции OPENXML.  
  
 Инструкция OPENXML иллюстрирует следующее:  
  
-   шаблон *rowpattern* (/ROOT/Customer/Order) определяет, что следует обрабатывать узлы <`Order`>;  
  
-   параметр *flags* имеет значение **1** , которое указывает на сопоставление с использованием атрибутивной модели; В результате XML-атрибуты сопоставляются со столбцами в наборе строк, определенном в элементе *SchemaDeclaration*;  
  
-   в элементе *SchemaDeclaration* предложения WITH имена столбцов **oid** и **amount** в наборе строк совпадают с соответствующими именами XML-атрибутов. Поэтому параметр *ColPattern* не указывается. Для столбца **comment** в наборе строк функция XPath **text()** задается в виде параметра *ColPattern*. Это перезаписывает сопоставление с использованием атрибутивной модели, заданное в параметре *flags*, и столбец содержит строку конечных значений содержимого элемента.  
  
 Затем инструкция SELECT извлекает все столбцы из набора строк, предоставленного инструкцией OPENXML:  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied  
      </Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
            <Urgency>Important</Urgency>  
            Happy Customer.  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH (oid     char(5),   
           amount  float,   
           comment ntext 'text()')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Результат:  
  
```  
oid   amount        comment  
----- -----------   -----------------------------  
O1    3.5           NULL  
O2    13.4          Customer was very satisfied  
O3    100.0         Happy Customer.  
O4    10000.0       NULL  
```  
  
### <a name="e-specifying-tablename-in-the-with-clause"></a>Д. Задание элемента TableName в предложении WITH  
 Этот пример задает элемент *TableName* в предложении WITH вместо элемента *SchemaDeclaration*. Это полезно, если таблица имеет нужную структуру и не требуются шаблоны столбцов (параметр *ColPattern* ).  
  
 XML-документ в этом примере состоит из элементов <`Customer`> и <`Order`>. Инструкция OPENXML возвращает сведения о заказе в наборе строк из трех столбцов (**oid**, **date** и **amount**), полученные из XML-документа.  
  
 Сначала вызывается хранимая процедура **sp_xml_preparedocument** , чтобы получить дескриптор документа. Дескриптор документа передается инструкции OPENXML.  
  
 Инструкция OPENXML иллюстрирует следующее:  
  
-   шаблон *rowpattern* (/ROOT/Customer/Order) определяет, что следует обрабатывать узлы <`Order`>;  
  
-   в предложении WITH отсутствует элемент *SchemaDeclaration* , вместо него задано имя таблицы; поэтому схема таблицы используется в качестве схемы набора строк;  
  
-   параметр *flags* имеет значение **1** , которое указывает на сопоставление с использованием атрибутивной модели; поэтому атрибуты элементов, заданные шаблоном *rowpattern*, сопоставляются со столбцами набора строк с таким же именем.  
  
 Затем инструкция SELECT извлекает все столбцы из набора строк, предоставленного инструкцией OPENXML:  
  
```  
-- Create a test table. This table schema is used by OPENXML as the  
-- rowset schema.  
CREATE TABLE T1(oid char(5), date datetime, amount float)  
GO  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
-- Sample XML document  
SET @xmlDocument =N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very   
             satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/root/Customer/Order', 1)  
     WITH T1  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Результат:  
  
```  
oid   date                        amount  
----- --------------------------- ----------  
O1    1996-01-20 00:00:00.000     3.5  
O2    1997-04-30 00:00:00.000     13.4  
O3    1999-07-14 00:00:00.000     100.0  
O4    1996-01-20 00:00:00.000     10000.0  
```  
  
### <a name="f-obtaining-the-result-in-an-edge-table-format"></a>Е. Получение результата в формате краевой таблицы  
 В этом примере предложение WITH не задано в инструкции OPENXML. В результате набор строк, сформированный инструкцией OPENXML, имеет формат краевой таблицы. Инструкция SELECT возвращает все столбцы в краевой таблице.  
  
 Образец XML-документа в этом примере состоит из элементов <`Customer`>, <`Order`> и <`OrderDetail`>.  
  
 Сначала вызывается хранимая процедура **sp_xml_preparedocument** , чтобы получить дескриптор документа. Дескриптор документа передается инструкции OPENXML.  
  
 Инструкция OPENXML иллюстрирует следующее:  
  
-   шаблон *rowpattern* (/ROOT/Customer) определяет, что следует обрабатывать узлы <`Customer`>;  
  
-   предложение WITH не задано, поэтому инструкция OPENXML возвращает набор строк в формате краевой таблицы.  
  
 Затем инструкция SELECT возвращает все столбцы в краевой таблице.  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
SET @xmlDocument = N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer')  
  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Результат возвращается в виде краевой таблицы. Можно написать запрос к краевой таблице для получения данных. Пример:  
  
-   следующий запрос возвращает количество узлов **Customer** в документе. Поскольку предложение WITH не используется, инструкция OPENXML возвращает краевую таблицу. Инструкция SELECT запрашивает краевую таблицу:  
  
    ```  
    SELECT count(*)  
    FROM OPENXML(@docHandle, '/')  
    WHERE localname = 'Customer'  
    ```  
  
-   следующий запрос возвращает локальные имена XML-узлов типа элементов:  
  
    ```  
    SELECT distinct localname   
    FROM OPENXML(@docHandle, '/')   
    WHERE nodetype = 1   
    ORDER BY localname  
    ```  
  
### <a name="g-specifying-rowpattern-ending-with-an-attribute"></a>Ж. Задание шаблона rowpattern, заканчивающегося атрибутом  
 XML-документ в этом примере состоит из элементов <`Customer`>, <`Order`> и <`OrderDetail`>. Инструкция OPENXML возвращает сведения о заказе в наборе строк из трех столбцов (**ProductID**, **Quantity** и **OrderID**) из XML-документа.  
  
 Сначала вызывается хранимая процедура **sp_xml_preparedocument** , чтобы получить дескриптор документа. Дескриптор документа передается инструкции OPENXML.  
  
 Инструкция OPENXML иллюстрирует следующее:  
  
-   шаблон *rowpattern* (/ROOT/Customer/Order/OrderDetail/\@ProductID) заканчивается атрибутом XML — **ProductID**. В результирующем наборе строк для каждого выбранного в XML-документе узла атрибута создается строка;  
  
-   в этом примере параметр *flags* не задан; вместо него для указания сопоставлений используется параметр *ColPattern* .  
  
 В элементе *SchemaDeclaration* предложения WITH параметр *ColPattern* также задан с параметрами *ColName* и *ColType* . Необязательный параметр *ColPattern* является заданным шаблоном XPath и указывает следующее:  
  
-   шаблон XPath ( **.** ), указанный в виде параметра *ColPattern* для столбца **ProdID** в наборе строк, определяет контекстный узел — текущий узел. Согласно заданному шаблону *rowpattern*, он является атрибутом **ProductID** элемента <`OrderDetail`>;  
  
-   шаблон *ColPattern*, **../\@Quantity**, заданный для столбца **Qty** в наборе строк, определяет атрибут **Quantity** родительского узла <`OrderDetail`> контекстного узла \<ProductID>;  
  
-   аналогично шаблон *ColPattern*, **../../\@OrderID**, заданный для столбца **OID** в наборе строк, определяет атрибут **OrderID** родительского элемента, <`Order`>, родительского узла контекстного узла. Узлом родителя является <`OrderDetail`>, а контекстным узлом является <`ProductID`>.  
  
 Затем инструкция SELECT извлекает все столбцы из набора строк, предоставленного инструкцией OPENXML:  
  
```  
DECLARE @docHandle int  
DECLARE @xmlDocument nvarchar(1000)  
--Sample XML document  
SET @xmlDocument =N'<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @docHandle OUTPUT, @xmlDocument  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@docHandle, '/ROOT/Customer/Order/OrderDetail/@ProductID')  
       WITH ( ProdID  int '.',  
              Qty     int '../@Quantity',  
              OID     int '../../@OrderID')  
EXEC sp_xml_removedocument @docHandle  
```  
  
 Результат:  
  
```  
ProdID      Qty         OID  
----------- ----------- -------   
11          12          10248  
42          10          10248  
72          3           10283  
```  
  
### <a name="h-specifying-an-xml-document-that-has-multiple-text-nodes"></a>З. Задание XML-документа, имеющего несколько текстовых узлов  
 При наличии нескольких текстовых узлов в XML-документе инструкция SELECT с параметром *ColPattern*, **text()** , возвращает только первый текстовый узел, а не все. Пример:  
  
```  
DECLARE @h int  
EXEC sp_xml_preparedocument @h OUTPUT,  
         N'<root xmlns:a="urn:1">  
           <a:Elem abar="asdf">  
             T<a>a</a>U  
           </a:Elem>  
         </root>',  
         '<ns xmlns:b="urn:1" />'  
  
SELECT * FROM openxml(@h, '/root/b:Elem')  
      WITH (Col1 varchar(20) 'text()')  
EXEC sp_xml_removedocument @h  
```  
  
 Инструкция SELECT возвращает в качестве результата таблицу **T** , а не **TaU**.  
  
### <a name="i-specifying-the-xml-data-type-in-the-with-clause"></a>И. Задание типа данных XML в предложении WITH  
 В предложении WITH шаблон столбца, который сопоставлен со столбцом типа данных **xml** , типизированным или нетипизированным, должен вернуть либо пустую последовательность, либо последовательность элементов, инструкций обработки, текстовых узлов и комментариев. Данные приводятся к типу **xml** .  
  
 В следующем примере объявление схемы таблицы в предложении WITH включает столбцы типа **xml** :  
  
```  
DECLARE @h int  
DECLARE @x xml  
set @x = '<Root>  
  <row id="1"><lname>Duffy</lname>  
   <Address>  
            <Street>111 Maple</Street>  
            <City>Seattle</City>  
   </Address>  
  </row>  
  <row id="2"><lname>Wang</lname>  
   <Address>  
            <Street>222 Pine</Street>  
            <City>Bothell</City>  
   </Address>  
  </row>  
</Root>'  
  
EXEC sp_xml_preparedocument @h output, @x  
SELECT *  
FROM   OPENXML (@h, '/Root/row', 10)  
      WITH (id int '@id',  
  
            lname    varchar(30),  
            xmlname  xml 'lname',  
            OverFlow xml '@mp:xmltext')  
EXEC sp_xml_removedocument @h  
```  
  
 А именно, переменная (\@x) типа **xml** передается функции **sp_xml_preparedocument()** .  
  
 Результат:  
  
```  
id  lname   xmlname                   OverFlow  
--- ------- ------------------------------ -------------------------------  
1   Duffy   <lname>Duffy</lname>  <row><Address>  
                                   <Street>111 Maple</Street>  
                                   <City>Seattle</City>  
                                  </Address></row>  
2   Wang    <lname>Wang</lname>   <row><Address>  
                                    <Street>222 Pine</Street>  
                                    <City>Bothell</City>  
                                   </Address></row>  
```  
  
 Обратите внимание на следующие факты:  
  
-   для столбца **lname** типа **varchar(30)** его значение получено из соответствующего элемента <`lname`>;  
  
-   для столбца **xmlname** типа **xml** в качестве значения возвращается элемент с таким же именем;  
  
-   флаг принимает значение 10, означающее 2 + 8, где 2 указывает на элементное сопоставление, а 8 — на то, что к столбцу OverFlow, заданному в предложении WITH, должны быть добавлены только невостребованные XML-данные. Если флаг устанавливается в значение 2, то в столбец OverFlow, заданный в предложении WITH, копируется весь XML-документ;  
  
-   если столбец в предложении WITH является типизированным XML-столбцом, а экземпляр XML-документа не соответствует схеме, то возвращается ошибка.  
  
### <a name="j-retrieving-individual-values-from-multivalued-attributes"></a>К. Получение отдельных значений из многозначных атрибутов  
 XML-документ может иметь многозначные атрибуты. Например, атрибут **IDREFS** может быть многозначным. В XML-документе значения многозначных атрибутов задаются в виде строки со значениями, разделенными пробелом. В следующем XML-документе атрибут **attends** элемента \<Student> и атрибут **attendedBy** элемента \<Class> являются многозначными. Получение отдельных значений из многозначных XML-атрибутов и сохранение каждого значения в отдельной строке базы данных требуют дополнительных операций. Данный пример иллюстрирует процесс.  
  
 Данный образец XML-документа состоит из следующих элементов:  
  
-   \<Student>  
  
     Атрибуты **id** (идентификатор студента), **name** и **attends** . Атрибут **attends** является многозначным атрибутом.  
  
-   \<Class>  
  
     Атрибуты **id** (идентификатор класса), **name** и **attendedBy** . Атрибут **attendedBy** является многозначным атрибутом.  
  
 Атрибут **attends** элемента \<Student> и атрибут **attendedBy** элемента \<Class> представляют отношение **m : n** между таблицами Student и Class. Студент может посещать множество классов, а класс может иметь множество студентов.  
  
 Предположим, что нужно взять часть этого документа и сохранить ее в базе данных, как показано ниже.  
  
-   Сохраните данные \<Student> в таблице Students.  
  
-   Сохраните данные \<Class> в таблице Courses.  
  
-   Сохраните данные связи **m:n** между таблицами Student и Class в таблице CourseAttendence. Для извлечения значений требуются дополнительные действия. Для получения этих сведений и их сохранения в таблице используйте следующие хранимые процедуры:  
  
    -   **Insert_Idrefs_Values**  
  
         Вставляет значения идентификатора курса и идентификатора студента в таблицу CourseAttendence.  
  
    -   **Extract_idrefs_values**  
  
         Извлекает идентификаторы отдельных студентов из каждого элемента \<Course>. Краевая таблица используется для получения этих значений.  
  
 Ниже приводятся шаги:  
  
```  
-- Create these tables:  
DROP TABLE CourseAttendance  
DROP TABLE Students  
DROP TABLE Courses  
GO  
CREATE TABLE Students(  
                id   varchar(5) primary key,  
                name varchar(30)  
                )  
GO  
CREATE TABLE Courses(  
               id       varchar(5) primary key,  
               name     varchar(30),  
               taughtBy varchar(5)  
)  
GO  
CREATE TABLE CourseAttendance(  
             id         varchar(5) references Courses(id),  
             attendedBy varchar(5) references Students(id),  
             constraint CourseAttendance_PK primary key (id, attendedBy)  
)  
go  
-- Create these stored procedures:  
DROP PROCEDURE f_idrefs  
GO  
CREATE PROCEDURE f_idrefs  
    @t      varchar(500),  
    @idtab  varchar(50),  
    @id     varchar(5)  
AS  
DECLARE @sp int  
DECLARE @att varchar(5)  
SET @sp = 0  
WHILE (LEN(@t) > 0)  
BEGIN   
    SET @sp = CHARINDEX(' ', @t+ ' ')  
    SET @att = LEFT(@t, @sp-1)  
    EXEC('INSERT INTO '+@idtab+' VALUES ('''+@id+''', '''+@att+''')')  
    SET @t = SUBSTRING(@t+ ' ', @sp+1, LEN(@t)+1-@sp)  
END  
Go  
  
DROP PROCEDURE fill_idrefs  
GO  
CREATE PROCEDURE fill_idrefs   
    @xmldoc     int,  
    @xpath      varchar(100),  
    @from       varchar(50),  
    @to         varchar(50),  
    @idtable    varchar(100)  
AS  
DECLARE @t varchar(500)  
DECLARE @id varchar(5)  
  
/* Temporary Edge table */  
SELECT *   
INTO #TempEdge   
FROM OPENXML(@xmldoc, @xpath)  
  
DECLARE fillidrefs_cursor CURSOR FOR  
    SELECT CAST(iv.text AS nvarchar(200)) AS id,  
           CAST(av.text AS nvarchar(4000)) AS refs  
    FROM   #TempEdge c, #TempEdge i,  
           #TempEdge iv, #TempEdge a, #TempEdge av  
    WHERE  c.id = i.parentid  
    AND    UPPER(i.localname) = UPPER(@from)  
    AND    i.id = iv.parentid  
    AND    c.id = a.parentid  
    AND    UPPER(a.localname) = UPPER(@to)  
    AND    a.id = av.parentid  
  
OPEN fillidrefs_cursor  
FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
    IF (@@FETCH_STATUS <> -2)  
    BEGIN  
        execute f_idrefs @t, @idtable, @id  
    END  
    FETCH NEXT FROM fillidrefs_cursor INTO @id, @t  
END  
CLOSE fillidrefs_cursor  
DEALLOCATE fillidrefs_cursor  
Go  
-- This is the sample document that is shredded and the data is stored in the preceding tables.  
DECLARE @h int  
EXECUTE sp_xml_preparedocument @h OUTPUT, N'<Data>  
  <Student id = "s1" name = "Student1"  attends = "c1 c3 c6"  />  
  <Student id = "s2" name = "Student2"  attends = "c2 c4" />  
  <Student id = "s3" name = "Student3"  attends = "c2 c4 c6" />  
  <Student id = "s4" name = "Student4"  attends = "c1 c3 c5" />  
  <Student id = "s5" name = "Student5"  attends = "c1 c3 c5 c6" />  
  <Student id = "s6" name = "Student6" />  
  
  <Class id = "c1" name = "Intro to Programming"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c2" name = "Databases"   
         attendedBy = "s2 s3" />  
  <Class id = "c3" name = "Operating Systems"   
         attendedBy = "s1 s4 s5" />  
  <Class id = "c4" name = "Networks" attendedBy = "s2 s3" />  
  <Class id = "c5" name = "Algorithms and Graphs"   
         attendedBy =  "s4 s5"/>  
  <Class id = "c6" name = "Power and Pragmatism"   
         attendedBy = "s1 s3 s5" />  
</Data>'  
  
INSERT INTO Students SELECT * FROM OPENXML(@h, '//Student') WITH Students  
  
INSERT INTO Courses SELECT * FROM OPENXML(@h, '//Class') WITH Courses  
/* Using the edge table */  
EXECUTE fill_idrefs @h, '//Class', 'id', 'attendedby', 'CourseAttendance'  
  
SELECT * FROM Students  
SELECT * FROM Courses  
SELECT * FROM CourseAttendance  
  
EXECUTE sp_xml_removedocument @h  
```  
  
### <a name="k-retrieving-binary-from-base64-encoded-data-in-xml"></a>Л. Получение двоичных данных из данных в XML, закодированных методом base64  
 Двоичные данные часто включаются в XML с использованием метода кодировки base64. Если взять часть этого XML с помощью инструкции OPENXML, то будут получены данные, закодированные методом base64. Этот пример показывает, как можно преобразовать данные в кодировке Base 64 обратно в двоичные.  
  
-   создайте таблицу с образцами двоичных данных;  
  
-   используйте запрос FOR XML и параметр BINARY BASE64 для формирования XML, который содержит двоичные данные, закодированные методом base64;  
  
-   возьмите часть XML с помощью инструкции OPENXML. Данные, возвращенные инструкцией OPENXML, будут данными, закодированными методом base64. Затем вызовите функцию .value для преобразования этих данных в двоичные.  
  
```  
CREATE TABLE T (Col1 int primary key, Col2 varbinary(100))  
go  
-- Insert sample binary data  
INSERT T VALUES(1, 0x1234567890)   
go  
 -- Create test XML document that has base64 encoded binary data (use FOR XML query and specify BINARY BASE64 option)  
SELECT * FROM T  
FOR XML AUTO, BINARY BASE64  
go  
-- result  
-- <T Col1="1" Col2="EjRWeJA="/>  
  
-- Now shredd the sample XML using OPENXML.   
-- Call the .value function to convert   
-- the base64 encoded data returned by OPENXML to binary.  
DECLARE @h int ;  
EXEC sp_xml_preparedocument @h OUTPUT, '<T Col1="1" Col2="EjRWeJA="/>' ;  
SELECT Col1,   
CAST('<binary>' + Col2 + '</binary>' AS XML).value('.', 'varbinary(max)') AS BinaryCol   
FROM openxml(@h, '/T')   
WITH (Col1 integer, Col2 varchar(max)) ;  
EXEC sp_xml_removedocument @h ;  
GO  
```  
  
 Результат. возвращенные двоичные данные являются исходными двоичными данными таблицы T:  
  
```  
Col1        BinaryCol  
----------- ---------------------  
1           0x1234567890  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_xml_preparedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-preparedocument-transact-sql.md)   
 [sp_xml_removedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)   
 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML (SQL Server)](../../relational-databases/xml/openxml-sql-server.md)  
  
  

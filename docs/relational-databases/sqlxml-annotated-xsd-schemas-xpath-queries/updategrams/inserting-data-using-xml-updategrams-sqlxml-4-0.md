---
title: Вставка данных с помощью XML диаграмм обновления (SQLXML)
description: Узнайте, как вставлять данные с помощью диаграмм обновления XML в SQLXML 4,0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- xsi:nil attribute
- unique values
- <after> block
- id attribute
- data insertions [SQLXML]
- nil attribute
- <before> block
- updg:guid attribute
- multiple record insertions
- returnid attribute
- updategrams [SQLXML], inserting data
- updg:at-identity attribute
- invalid characters [SQLXML]
- updg:returnid attribute
- updg:id attribute
- namespaces [SQLXML], updategrams
- IDENTITY-type column
- guid attribute
- record insertion [SQLXML]
- null values [SQLXML]
- at-identity attribute
- xml data type [SQL Server], SQLXML
ms.assetid: 4dc48762-bc12-43fb-b356-ea1b9c1e287e
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6ee3b15137e43e0c82fd939030d3de44d8a46f2d
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491584"
---
# <a name="inserting-data-using-xml-updategrams-sqlxml-40"></a>Вставка данных с помощью диаграмм обновления XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Диаграмма обновления указывает операцию вставки, когда экземпляр записи появляется в **\<after>** блоке, но не в соответствующем **\<before>** блоке. В этом случае диаграмма обновления вставляет запись в **\<after>** блок в базу данных.  
  
 Ниже приведен формат диаграммы обновления для операции вставки:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema="SampleSchema.xml"]  >  
   [<updg:before>  
   </updg:before>]  
    <updg:after [updg:returnid="x y ...] >  
       <ElementName [updg:id="value"]   
                   [updg:at-identity="x"]   
                   [updg:guid="y"]  
                   attribute="value"   
                   attribute="value"  
                   ...  
       />  
      [<ElementName .../>... ]  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
## <a name="before-block"></a>\<before> Блок  
 **\<before>** Блок можно опустить для операции вставки. Если необязательный атрибут **Mapping-Schema** не указан, то объект **\<ElementName>** , указанный в диаграмма обновления, сопоставляется с таблицей базы данных, а дочерние элементы или атрибуты сопоставляются со столбцами в таблице.  
  
## <a name="after-block"></a>\<after> Блок  
 В блоке можно указать одну или несколько записей **\<after>** .  
  
 Если **\<after>** блок не предоставляет значение для определенного столбца, диаграмма обновления использует значение по умолчанию, указанное в схеме с заметками (если указана схема). Если в схеме не указано значение по умолчанию для столбца, диаграмма обновления не указывает явное значение для этого столбца, а вместо этого присваивает [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] этому столбцу значение по умолчанию (если указано). Если не существует значения по умолчанию [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и столбец допускает значение NULL, то диаграмма обновления задает для столбца значение NULL. Если столбец не имеет значения по умолчанию и не допускает значение NULL, команда завершается ошибкой и диаграмма обновления возвращает ошибку. Необязательный атрибут **атрибута updg: returnid '** используется для возвращения значения идентификатора, формируемого системой при добавлении записи в таблицу со столбцом типа Identity.  
  
## <a name="updgid-attribute"></a>Атрибут updg:id  
 Если диаграмма обновления вставляет только записи, диаграмма обновления не требует атрибута **атрибута updg: ID** . Дополнительные сведения о **атрибута updg: ID** см. в статье [Обновление данных с помощью XML-диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/updating-data-using-xml-updategrams-sqlxml-4-0.md).  
  
## <a name="updgat-identity-attribute"></a>Атрибут updg:at-identity  
 Когда диаграмма обновления вставляет запись в таблицу, имеющую столбец типа IDENTITY, диаграмма обновления может записать присвоенное системой значение, используя необязательный атрибут **атрибута updg: at-identity** . Диаграмма обновления может потом использовать это значение в последующих операциях. После выполнения диаграмма обновления можно вернуть значение идентификатора, созданное путем указания атрибута **атрибута updg: returnid '** .  
  
## <a name="updgguid-attribute"></a>Атрибут updg:guid  
 Атрибут **атрибута updg: GUID** является необязательным атрибутом, который создает глобальный уникальный идентификатор. Это значение остается в области для всего **\<sync>** блока, в котором он указан. Это значение можно использовать в любом месте **\<sync>** блока. Атрибут вызывает функцию **NewGuid ()** [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для создания уникального идентификатора.  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы с помощью следующих примеров, необходимо выполнить требования, указанные в [требованиях к запуску примеров SQLXML](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
 При использовании примеров диаграмм обновления необходимо учитывать следующие моменты.  
  
-   В большинстве примеров используется сопоставление по умолчанию (то есть в диаграмме обновления схема сопоставления не задана). Дополнительные примеры диаграмм обновления, в которых используются схемы сопоставления, см. [в разделе Указание схемы сопоставления с заметками в диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
-   В большинстве примеров задействован образец базы данных [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)]. Все обновления применяются к таблицам в этой базе данных.  
  
### <a name="a-inserting-a-record-by-using-an-updategram"></a>A. Вставка записи с помощью диаграммы обновления  
 Эта диаграмма обновления с атрибутивной моделью вставляет запись в таблицу HumanResources.Employee в базе данных [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)].  
  
 В этом примере диаграмма обновления не указывает схему сопоставления. Следовательно, диаграмма обновления использует сопоставление по умолчанию, при котором имя элемента сопоставляется с именем таблицы, а атрибуты или дочерние элементы сопоставляются со столбцами таблицы.  
  
 Схема [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] для таблицы HumanResources.Department налагает ограничение «not null» на все столбцы. Следовательно, диаграмма обновления должна содержать значения, указанные для всех столбцов. Столбец DepartmentID является столбцом типа IDENTITY. Поэтому для него значения не указаны.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department   
            Name="New Product Research"   
            GroupName="Research and Development"   
            ModifiedDate="2010-08-31"/>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем MyUpdategram.xml.  
  
2.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 При элементном сопоставлении диаграмма обновления выглядит следующим образом:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department>  
            <Name> New Product Research </Name>  
            <GroupName> Research and Development </GroupName>  
            <ModifiedDate>2010-08-31</ModifiedDate>  
       </HumanResources.Department>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 В диаграмме обновления со смешанной моделью (элементной и атрибутивной) элемент может иметь как атрибуты, так и вложенные элементы, как показано на следующей диаграмме обновления:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
       <HumanResources.Department   
            Name=" New Product Research "   
            <GroupName>Research and Development</GroupName>  
            <ModifiedDate>2010-08-31</ModifiedDate>  
       </HumanResources.Department>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
### <a name="b-inserting-multiple-records-by-using-an-updategram"></a>Б. Вставка нескольких записей с помощью диаграммы обновления  
 Данная диаграмма обновления добавляет две новые записи о сменах в таблицу HumanResources.Shift. В диаграмма обновления не указан необязательный **\<before>** блок.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync>  
    <updg:after >  
       <HumanResources.Shift Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем Updategram-AddShifts.xml.  
  
2.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Другой версией этого примера является диаграмма обновления, использующий два отдельных **\<after>** блока, а не один блок для вставки двух сотрудников. Это допустимо и может быть закодировано следующим образом:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync>  
    <updg:after >  
       <HumanResources.Shift Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
    <updg:before>  
    </updg:before>  
    <updg:after >  
       <HumanResources.Shift Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
### <a name="c-working-with-valid-sql-server-characters-that-are-not-valid-in-xml"></a>В. Работа с допустимыми символами SQL Server, не являющимися допустимыми в XML  
 В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] имена таблиц могут содержать пробелы, как, например, таблица Order Details в базе данных Northwind. Однако это недопустимо в XML-символах, которые являются допустимыми [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] идентификаторами, но недопустимые идентификаторы XML могут быть закодированы с помощью "__xHHHH \_ \_ " в качестве значения кодировки, где HHHH обозначает шестнадцатеричный восьмеричный код UCS-2 для символа в наиболее значимом битовом порядке.  
  
> [!NOTE]  
>  В данном примере используется образец базы данных Northwind. Базу данных Northwind можно установить с помощью скрипта SQL, доступного для загрузки с этого [веб-сайта корпорации Майкрософт](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs).  
  
 Кроме того, имя элемента должно быть заключено в квадратные скобки ([ ]). Поскольку символы [и] недопустимы в XML, их необходимо кодировать как _x005B \_ и _x005D \_ соответственно. Если используется схема сопоставления, то можно предоставить имена элементов, не содержащие недопустимых символов, таких как пробелы. Схема сопоставления выполняет необходимое сопоставление, следовательно, необязательно кодировать эти символы.  
  
 Эта диаграмма обновления добавляет запись в таблицу Order Details в базе данных Northwind:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after>  
      <_x005B_Order_x0020_Details_x005D_ OrderID="1"  
            ProductID="11"  
            UnitPrice="$1.0"  
            Quantity="1"  
            Discount="0.0" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Столбец UnitPrice в таблице Order Details имеет тип **money** . Чтобы применить соответствующее преобразование типа (из **строкового** типа к типу **money** ), в качестве части значения необходимо добавить символ доллара ($). Если диаграмма обновления не указывает схему сопоставления, то вычисляется первый символ **строкового** значения. Если первый символ — символ доллара ($), применяется соответствующее преобразование.  
  
 Если диаграмма обновления указана для схемы сопоставления, в которой столбец соответствующим образом помечен как **DT: Type = "fixed. 14,4"** или **SQL: datatype = "Money"**, то знак доллара ($) не требуется, а преобразование обрабатывается сопоставлением. Это рекомендуемый способ, который гарантирует, что соответствующее преобразование типов происходит.  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем UpdategramSpacesInTableName.xml.  
  
2.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="d-using-the-at-identity-attribute-to-retrieve-the-value-that-has-been-inserted-in-the-identity-type-column"></a>Г. Использование атрибута at-identity для получения значения, вставленного в столбец типа IDENTITY  
 Следующая диаграмма обновления вставляет две записи: одну в таблицу Sales.SalesOrderHeader, а другую — в таблицу Sales.SalesOrderDetail.  
  
 Сначала диаграмма обновления добавляет запись в таблицу Sales.SalesOrderHeader. В этой таблице столбец SalesOrderID является столбцом типа IDENTITY. Поэтому при добавлении этой записи в таблицу диаграмма обновления использует атрибут **at-identity** для записи назначенного значения SalesOrderID как «x» (значение заполнителя). Затем используется задает эту переменную **с идентификатором** в качестве значения атрибута SalesOrderID в \<Sales.SalesOrderDetail> элементе.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
 <updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
   <Sales.SalesOrderHeader updg:at-identity="x"   
             RevisionNumber="1"  
             OrderDate="2001-07-01 00:00:00.000"  
             DueDate="2001-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             CustomerID="676"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="24643.9362"  
             TaxAmt="1971.5149"  
             Freight="616.0984"  
             rowguid="00001111-2222-3333-4444-556677889900"  
             ModifiedDate="2001-07-08 00:00:00.000" />  
      <Sales.SalesOrderDetail SalesOrderID="x"  
                LineNumber="1"  
                OrderQty="1"  
                ProductID="776"  
                SpecialOfferID="1"  
                UnitPrice="2429.9928"  
                UnitPriceDiscount="0.00"  
                rowguid="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"  
                ModifiedDate="2001-07-01 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Если требуется возвратить значение идентификатора, созданное атрибутом **атрибута updg: at-identity** , можно использовать атрибут **атрибута updg: returnid '** . Ниже показана измененная диаграмма обновления, которая возвращает значение идентификатора. Эта диаграмма обновления добавляет две записи заказа и две записи подробностей заказа, чтобы немного усложнить пример.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
 <updg:sync>  
  <updg:before>  
  </updg:before>  
  <updg:after updg:returnid="x y" >  
       <HumanResources.Shift updg:at-identity="x" Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift updg:at-identity="y" Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
  </updg:after>  
 </updg:sync>  
</ROOT>  
```  
  
 При выполнении диаграммы обновления она возвращает результат, подобный следующему, который содержит сформированное значение идентификатора (сформированное значение столбца ShiftID, используемого для идентификатора таблицы):  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">   
  <returnid>   
    <x>4</x>   
    <y>5</y>   
  </returnid>   
</ROOT>  
```  
  
##### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем Updategram-returnId.xml.  
  
2.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="e-using-the-updgguid-attribute-to-generate-a-unique-value"></a>Д. Использование атрибута updg:guid для формирования уникального значения  
 В этом примере диаграмма обновления вставляет запись в таблицы Cust и CustOrder. Кроме того, диаграмма обновления создает уникальное значение для атрибута CustomerID с помощью атрибута **атрибута updg: GUID** .  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync >  
    <updg:before>  
    </updg:before>  
    <updg:after updg:returnid="x" >  
      <Cust updg:guid="x" >  
         <CustID>x</CustID>  
         <LastName>Fuller</LastName>  
      </Cust>  
      <CustOrder>  
         <CustID>x</CustID>  
         <OrderID>1</OrderID>  
      </CustOrder>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Диаграмма обновления указывает атрибут **returnid '** . В результате возвращается сформированное значение GUID:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <returnid>  
    <x>7111BD1A-7F0B-4CEE-B411-260DADFEFA2A</x>   
  </returnid>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем Updategram-GenerateGuid.xml.  
  
2.  Создайте следующие таблицы.  
  
    ```  
    USE tempdb  
    CREATE TABLE Cust (CustID uniqueidentifier, LastName varchar(20))  
    CREATE TABLE CustOrder (CustID uniqueidentifier, OrderID int)  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="f-specifying-a-schema-in-an-updategram"></a>Е. Указание схемы в диаграмме обновления  
 Диаграмма обновления в этом примере вставляет запись в следующую таблицу:  
  
```  
CustOrder(OrderID, EmployeeID, OrderType)  
```  
  
 В этой диаграмме обновления указана схема XSD (нет сопоставления по умолчанию элементов и атрибутов диаграммы обновления). Схема обеспечивает необходимое сопоставление элементов и атрибутов таблицам и столбцам базы данных.  
  
 Следующая схема (CustOrderSchema.xml) описывает **\<CustOrder>** элемент, состоящий из атрибутов **OrderID** и **EmployeeID** . Чтобы сделать схему более интересной, атрибуту **EmployeeID** присваивается значение по умолчанию. В диаграмме обновления значение атрибута по умолчанию используется только для операций вставки, и только если диаграмма обновления не указывает этот атрибут.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="CustOrder" >  
   <xsd:complexType>  
        <xsd:attribute name="OrderID"     type="xsd:integer" />   
        <xsd:attribute name="EmployeeID"  type="xsd:integer" />  
        <xsd:attribute name="OrderType  " type="xsd:integer" default="1"/>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Эта диаграмма обновления вставляет записи в таблицу CustOrder. Диаграмма обновления указывает только значения атрибутов OrderID и EmployeeID. Она не указывает значение атрибута OrderType. Поэтому в диаграмме обновления используется значение по умолчанию для атрибута EmployeeID, указанное в предшествующей схеме.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"  
             xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync mapping-schema='CustOrderSchema.xml'>  
<updg:after>  
       <CustOrder OrderID="98000" EmployeeID="1" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
 Дополнительные примеры диаграмм обновления, указывающие схему сопоставления, см. [в разделе Указание схемы сопоставления с заметками в диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Создайте эту таблицу в базе данных **tempdb** :  
  
    ```  
    USE tempdb  
    CREATE TABLE CustOrder(  
                     OrderID int,   
                     EmployeeID int,   
                     OrderType int)  
    ```  
  
2.  Скопируйте приведенную выше схему и вставьте ее в текстовый файл. Сохраните файл под именем CustOrderSchema.xml.  
  
3.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем CustOrderUpdategram.xml в той же папке, которая используется в предыдущем шаге.  
  
4.  Создайте тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs) и воспользуйтесь им для выполнения диаграммы обновления.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Эквивалентная схема XDR:  
  
```  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
 <ElementType name="CustOrder" >  
    <AttributeType name="OrderID" />  
    <AttributeType name="EmployeeID" />  
    <AttributeType name="OrderType" default="1" />  
    <attribute type="OrderID"  />  
    <attribute type="EmployeeID" />  
    <attribute type="OrderType" />  
  </ElementType>  
</Schema>  
```  
  
### <a name="g-using-the-xsinil-attribute-to-insert-null-values-in-a-column"></a>Ж. Использование атрибута xsi:nil для вставки в столбец значений NULL  
 Если необходимо вставить значение NULL в соответствующий столбец таблицы, можно указать атрибут **xsi: nil** для элемента в диаграмма обновления. В соответствующей схеме XSD также должен быть указан атрибут XSD- **обнуляемых** .  
  
 В качестве примера рассмотрим следующую схему XSD.  
  
```  
<?xml version="1.0"?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:element name="Student" sql:relation="Students">  
  <xsd:complexType>  
    <xsd:all>  
      <xsd:element name="fname" sql:field="first_name"   
                                type="xsd:string"   
                                 nillable="true"/>  
    </xsd:all>  
    <xsd:attribute name="SID"   
                        sql:field="StudentID"  
                        type="xsd:ID"/>      
    <xsd:attribute name="lname"       
                        sql:field="last_name"  
                        type="xsd:string"/>  
    <xsd:attribute name="minitial"    
                        sql:field="middle_initial"   
                        type="xsd:string"/>  
    <xsd:attribute name="years"       
                         sql:field="no_of_years"  
                         type="xsd:integer"/>  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 Схема XSD задает для элемента **значение обнуляемых = "true"** **\<fname>** . Следующая диаграмма обновления использует эту схему:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"  
      xmlns:updg="urn:schemas-microsoft-com:xml-updategram"  
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
<updg:sync mapping-schema='StudentSchema.xml'>  
  <updg:before/>  
  <updg:after>  
    <Student SID="S00004" lname="Elmaci" minitial="" years="2">  
      <fname xsi:nil="true">  
    </fname>  
    </Student>  
  </updg:after>  
</updg:sync>  
  
</ROOT>  
```  
  
 Диаграмма обновления указывает **xsi: nil** для **\<fname>** элемента в **\<after>** блоке. Поэтому при выполнении этой диаграммы обновления значение NULL вставляется для столбца first_name таблицы.  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Создайте в базе данных **tempdb** следующую таблицу:  
  
    ```  
    USE tempdb  
    CREATE TABLE Students (  
       StudentID char(6)NOT NULL ,  
       first_name varchar(50),  
       last_name varchar(50),  
       middle_initial char(1),  
       no_of_years int NULL)  
    GO  
    ```  
  
2.  Скопируйте приведенную выше схему и вставьте ее в текстовый файл. Сохраните файл под именем StudentSchema.xml.  
  
3.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем StudentUpdategram.xml в той же папке, которая использовалась на предыдущем шаге для сохранения файла StudentSchema.xml.  
  
4.  Создайте тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs) и воспользуйтесь им для выполнения диаграммы обновления.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="h-specifying-namespaces-in-an-updategram"></a>З. Указание пространств имен в диаграмме обновления  
 Диаграмма обновления может содержать элементы, которые принадлежат пространству имен, объявленному в этом же элементе диаграммы обновления. В этом случае соответствующая схема также должна объявлять то же пространство имен и элемент должен принадлежать этому целевому пространству имен.  
  
 Например, в следующем диаграмма обновления (UpdateGram-ElementHavingNamespace.xml) **\<Order>** элемент принадлежит пространству имен, объявленному в элементе.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync mapping-schema='XSD-ElementHavingNameSpace.xml'>  
    <updg:after>  
       <x:Order  xmlns:x="https://server/xyz/schemas/"  
             updg:at-identity="SalesOrderID"   
             RevisionNumber="1"  
             OrderDate="2001-07-01 00:00:00.000"  
             DueDate="2001-07-13 00:00:00.000"  
             OnlineOrderFlag="0"  
             CustomerID="676"  
             ContactID="378"  
             BillToAddressID="985"  
             ShipToAddressID="985"  
             ShipMethodID="5"  
             SubTotal="24643.9362"  
             TaxAmt="1971.5149"  
             Freight="616.0984"  
             rowguid="00009999-8888-7777-6666-554433221100"  
             ModifiedDate="2001-07-08 00:00:00.000" />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 В этом случае схема должна тоже объявлять пространство имен, как показано в следующей схеме:  
  
 Следующая схема (XSD-ElementHavingNamespace.xml) показывает, как должен быть декларирован соответствующий элемент и атрибуты.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
     xmlns:dt="urn:schemas-microsoft-com:datatypes"   
     xmlns:sql="urn:schemas-microsoft-com:mapping-schema"    
     xmlns:x="https://server/xyz/schemas/"   
     targetNamespace="https://server/xyz/schemas/" >  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader" type="x:Order_type"/>  
  <xsd:complexType name="Order_type">  
    <xsd:attribute name="SalesOrderID"  type="xsd:ID"/>  
    <xsd:attribute name="RevisionNumber" type="xsd:unsignedByte"/>  
    <xsd:attribute name="OrderDate" type="xsd:dateTime"/>  
    <xsd:attribute name="DueDate" type="xsd:dateTime"/>  
    <xsd:attribute name="ShipDate" type="xsd:dateTime"/>  
    <xsd:attribute name="Status" type="xsd:unsignedByte"/>  
    <xsd:attribute name="OnlineOrderFlag" type="xsd:boolean"/>  
    <xsd:attribute name="SalesOrderNumber" type="xsd:string"/>  
    <xsd:attribute name="PurchaseOrderNumber" type="xsd:string"/>  
    <xsd:attribute name="AccountNumber" type="xsd:string"/>  
    <xsd:attribute name="CustomerID" type="xsd:int"/>  
    <xsd:attribute name="ContactID" type="xsd:int"/>  
    <xsd:attribute name="SalesPersonID" type="xsd:int"/>  
    <xsd:attribute name="TerritoryID" type="xsd:int"/>  
    <xsd:attribute name="BillToAddressID" type="xsd:int"/>  
    <xsd:attribute name="ShipToAddressID" type="xsd:int"/>  
    <xsd:attribute name="ShipMethodID" type="xsd:int"/>  
    <xsd:attribute name="CreditCardID" type="xsd:int"/>  
    <xsd:attribute name="CreditCardApprovalCode" type="xsd:string"/>  
    <xsd:attribute name="CurrencyRateID" type="xsd:int"/>  
    <xsd:attribute name="SubTotal" type="xsd:decimal"/>  
    <xsd:attribute name="TaxAmt" type="xsd:decimal"/>  
    <xsd:attribute name="Freight" type="xsd:decimal"/>  
    <xsd:attribute name="TotalDue" type="xsd:decimal"/>  
    <xsd:attribute name="Comment" type="xsd:string"/>  
    <xsd:attribute name="rowguid" type="xsd:string"/>  
    <xsd:attribute name="ModifiedDate" type="xsd:dateTime"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Скопируйте приведенную выше схему и вставьте ее в текстовый файл. Сохраните файл под именем XSD-ElementHavingNamespace.xml.  
  
2.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем Updategram-ElementHavingNamespace.xml в той же папке, в которой был сохранен файл XSD-ElementHavingnamespace.xml.  
  
3.  Создайте тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs) и воспользуйтесь им для выполнения диаграммы обновления.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
### <a name="i-inserting-data-into-an-xml-data-type-column"></a>И. Вставка данных в столбец типа данных XML  
 Тип данных **XML** был введен в [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] . Диаграмм обновления можно использовать для вставки и обновления данных, хранящихся в столбцах типа данных **XML** , со следующими положениями:  
  
-   **XML-** столбец не может быть использован для определения существующей строки. Поэтому он не может быть добавлен в раздел **атрибута updg: before** диаграмма обновления.  
  
-   Пространства имен, которые находятся в области фрагмента XML, вставленного в **XML-** столбец, будут сохранены, а их объявления пространств имен будут добавлены к верхнему элементу вставленного фрагмента.  
  
 Например, в следующем диаграмма обновления (SampleUpdateGram.xml) **\<Desc>** элемент обновляет столбец ProductDescription в таблице Production>productModel в [!INCLUDE[ssSampleDBobject](../../../includes/sssampledbobject-md.md)] образце базы данных. Результатом этого диаграмма обновления является то, что XML-содержимое столбца ProductDescription обновляется с помощью XML-содержимого **\<Desc>** элемента.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
    <updg:sync mapping-schema="SampleSchema.xml" >  
       <updg:before>  
<ProductModel ProductModelID="19">  
   <Name>Mountain-100</Name>  
</ProductModel>  
    </updg:before>  
    <updg:after>  
 <ProductModel>  
    <Name>Mountain-100</Name>  
    <Desc><?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>  
        <p1:ProductDescription xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
              xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
              xmlns:wf="https://www.adventure-works.com/schemas/OtherFeatures"   
              xmlns:html="http://www.w3.org/1999/xhtml"   
              xmlns="">  
  <p1:Summary>  
     <html:p>Insert Example</html:p>  
  </p1:Summary>  
  <p1:Manufacturer>  
    <p1:Name>AdventureWorks</p1:Name>  
    <p1:Copyright>2002</p1:Copyright>  
    <p1:ProductURL>HTTP://www.Adventure-works.com</p1:ProductURL>  
  </p1:Manufacturer>  
  <p1:Features>These are the product highlights.   
    <wm:Warranty>  
       <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
       <wm:Description>parts and labor</wm:Description>  
    </wm:Warranty>  
    <wm:Maintenance>  
       <wm:NoOfYears>10 years</wm:NoOfYears>  
       <wm:Description>maintenance contract available through your dealer or any AdventureWorks retail store.</wm:Description>  
    </wm:Maintenance>  
    <wf:wheel>High performance wheels.</wf:wheel>  
    <wf:saddle>  
      <html:i>Anatomic design</html:i> and made from durable leather for a full-day of riding in comfort.</wf:saddle>  
    <wf:pedal>  
       <html:b>Top-of-the-line</html:b> clipless pedals with adjustable tension.</wf:pedal>  
    <wf:BikeFrame>Each frame is hand-crafted in our Bothell facility to the optimum diameter and wall-thickness required of a premium mountain frame. The heat-treated welded aluminum frame has a larger diameter tube that absorbs the bumps.</wf:BikeFrame>  
    <wf:crankset> Triple crankset; alumunim crank arm; flawless shifting. </wf:crankset>  
   </p1:Features>  
   <p1:Picture>  
      <p1:Angle>front</p1:Angle>  
      <p1:Size>small</p1:Size>  
      <p1:ProductPhotoID>118</p1:ProductPhotoID>  
   </p1:Picture>  
   <p1:Specifications> These are the product specifications.  
     <Material>Almuminum Alloy</Material>  
     <Color>Available in most colors</Color>  
     <ProductLine>Mountain bike</ProductLine>  
     <Style>Unisex</Style>  
     <RiderExperience>Advanced to Professional riders</RiderExperience>  
   </p1:Specifications>  
  </p1:ProductDescription>  
 </Desc>  
      </ProductModel>  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Диаграмма обновления ссылается на следующую схему XSD с заметками (SampleSchema.xml).  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
           xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
           xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">   
  <xsd:element name="ProductModel"  sql:relation="Production.ProductModel" >  
     <xsd:complexType>  
       <xsd:sequence>  
          <xsd:element name="Name" type="xsd:string"></xsd:element>  
          <xsd:element name="Desc" sql:field="CatalogDescription" sql:datatype="xml">  
           <xsd:complexType>  
            <xsd:sequence>  
              <xsd:element name="ProductDescription">  
                 <xsd:complexType>  
                   <xsd:sequence>  
                     <xsd:element name="Summary" type="xsd:anyType">  
                     </xsd:element>  
                   </xsd:sequence>  
                 </xsd:complexType>  
              </xsd:element>  
            </xsd:sequence>  
           </xsd:complexType>  
          </xsd:element>   
       </xsd:sequence>  
       <xsd:attribute name="ProductModelID" sql:field="ProductModelID"/>  
     </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Скопируйте приведенную выше схему и вставьте ее в текстовый файл. Сохраните файл под именем XSD-SampleSchema.xml.  
  
    > [!NOTE]  
    >  Так как диаграмм обновления поддерживает сопоставление по умолчанию, невозможно найти начало и конец типа данных **XML** . Это означает, что схема сопоставления необходима при вставке или обновлении таблиц со столбцами типа данных **XML** . Если схема не предоставляется, SQLXML возвращает ошибку, указывающую, что один из столбцов пропущен в таблице.  
  
2.  Скопируйте приведенную выше диаграмму обновления и вставьте ее в текстовый файл. Сохраните файл под именем SampleUpdategram.xml в том же каталоге, где был сохранен файл SampleSchema.xml.  
  
3.  Создайте тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs) и воспользуйтесь им для выполнения диаграммы обновления.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="see-also"></a>См. также  
 [Вопросы безопасности диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  

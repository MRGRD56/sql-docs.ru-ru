---
title: Примеры групповой загрузки XML (SQLXML)
description: Ознакомьтесь с подробными примерами функций групповой загрузки XML в СКККСМЛ 4,0 со схемами XSD и XDR для каждого примера.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- overflow-field annotation
- ConnectionCommand property
- XMLFragment property
- relationship chains [SQLXML]
- multiple table bulk loading
- examples [SQLXML], XML Bulk Load
- overflow data [SQLXML]
- sql:datatype
- transaction mode [SQLXML]
- CheckConstraints property
- sql:overflow-field
- XML Bulk Load [SQLXML], examples
- TempFilePath property
- datatype annotation
- Transaction property
- KeepIdentity property
- Execute method
- streaming XML data
- xml data type [SQL Server], SQLXML
- bulk load [SQLXML], examples
ms.assetid: 970e4553-b41d-4a12-ad50-0ee65d1f305d
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 136d5e2650faa9f092f331fee2b4fb4bafd86570
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492005"
---
# <a name="xml-bulk-load-examples-sqlxml-40"></a>Примеры массовой загрузки XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Следующие примеры демонстрируют функцию массовой загрузки XML в Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Каждый из примеров предоставляет собой схему XSD и эквивалентную ей схему XDR.  
  
## <a name="bulk-loader-script-validateandbulkloadvbs"></a>Скрипт массового загрузчика (ValidateAndBulkload.vbs)  
 Следующий скрипт, написанный в [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript), загружает XML-документ в XML DOM; проверяет его на соответствие схеме; и, если документ является допустимым, выполняет пакетную загрузку XML для загрузки XML в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] таблицу. Этот скрипт можно использовать с каждым из примеров, которые ссылаются на него далее в этом разделе.  
  
> [!NOTE]  
>  Массовая загрузка XML не выдает предупреждений и ошибок, если передача содержимого из файла данных не была выполнена. Поэтому рекомендуется перед выполнением операции массовой загрузки проверить файл XML-данных.  
  
```vbs  
Dim FileValid  
  
set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=MyServer;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
  
'Validate the data file prior to bulkload  
Dim sOutput   
sOutput = ValidateFile("SampleXMLData.xml", "", "SampleSchema.xml")  
WScript.Echo sOutput  
  
If FileValid Then  
   ' Check constraints and initiate transaction (if needed)  
   ' objBL.CheckConstraints = True  
   ' objBL.Transaction=True  
  'Execute XML bulkload using file.  
  objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
  set objBL=Nothing  
End If  
  
Function ValidateFile(strXmlFile,strUrn,strXsdFile)  
  
   ' Create a schema cache and add SampleSchema.xml to it.  
   Dim xs, fso, sAppPath  
   Set fso = CreateObject("Scripting.FileSystemObject")   
   Set xs = CreateObject("MSXML2.XMLSchemaCache.6.0")  
   sAppPath = fso.GetFolder(".")   
   xs.Add strUrn, sAppPath & "\" & strXsdFile  
  
   ' Create an XML DOMDocument object.  
   Dim xd   
   Set xd = CreateObject("MSXML2.DOMDocument.6.0")  
  
   ' Assign the schema cache to the DOM document.  
   ' schemas collection.  
   Set xd.schemas = xs  
  
   ' Load XML document as DOM document.  
   xd.async = False  
   xd.Load sAppPath & "\" & strXmlFile  
  
   ' Return validation results in message to the user.  
   If xd.parseError.errorCode <> 0 Then  
        ValidateFile = "Validation failed on " & _  
             strXmlFile & vbCrLf & _  
             "=======" & vbCrLf & _  
             "Reason: " & xd.parseError.reason & _  
             vbCrLf & "Source: " & _  
             xd.parseError.srcText & _  
             vbCrLf & "Line: " & _  
             xd.parseError.Line & vbCrLf  
             FileValid = False  
    Else  
        ValidateFile = "Validation succeeded for " & _  
             strXmlFile & vbCrLf & _  
             "========" & _  
             vbCrLf & "Contents to be bulkloaded" & vbCrLf  
             FileValid = True  
    End If  
End Function  
```  
  
## <a name="a-bulk-loading-xml-in-a-table"></a>A. Массовая загрузка XML-данных в таблицу  
 В этом примере устанавливается соединение с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , указанным в свойстве ConnectionString (MyServer). В примере также указывается свойство Еррорлогфиле. Поэтому ошибочный вывод сохраняется в указанном файле («C:\error.log»), для которого также можно указать другое место. Кроме того, обратите внимание, что метод Execute имеет в качестве параметров файл схемы сопоставления (SampleSchema.xml) и XML-файл данных (SampleXMLData.xml). При выполнении групповой загрузки таблица Cust, созданная в базе данных **tempdb** , будет содержать новые записи на основе содержимого XML-файла данных.  
  
#### <a name="to-test-a-sample-bulk-load"></a>Проверка образца массовой загрузки  
  
1.  Создайте такую таблицу:  
  
    ```sql  
    CREATE TABLE Cust(CustomerID  int PRIMARY KEY,  
                      CompanyName varchar(20),  
                      City        varchar(20));  
    GO  
    ```  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Добавьте в этот файл следующую схему XSD.  
  
    ```xml  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
       <xsd:element name="ROOT" sql:is-constant="1" >  
         <xsd:complexType>  
           <xsd:sequence>  
             <xsd:element name="Customers" sql:relation="Cust" maxOccurs="unbounded">  
               <xsd:complexType>  
                 <xsd:sequence>  
                   <xsd:element name="CustomerID"  type="xsd:integer" />  
                   <xsd:element name="CompanyName" type="xsd:string" />  
                   <xsd:element name="City"        type="xsd:string" />  
                 </xsd:sequence>  
               </xsd:complexType>  
             </xsd:element>  
           </xsd:sequence>  
          </xsd:complexType>  
         </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleXMLData.xml. Добавьте в этот файл следующий XML-документ.  
  
    ```xml  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Sean Chai</CompanyName>  
        <City>New York</City>  
      </Customers>  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Tom Johnston</CompanyName>  
         <City>Los Angeles</City>  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Institute of Art</CompanyName>  
        <City>Chicago</City>  
      </Customers>  
    </ROOT>  
    ```  
  
4.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем ValidateAndBulkload.vbs. Добавьте в этот файл код VBScript, приведенный в начале этого раздела. Измените строку соединения, указав соответствующее имя сервера. Укажите соответствующий путь для файлов, указанных в качестве параметров метода Execute.  
  
5.  Выполните код VBScript. При массовой загрузке XML-данные загружаются в таблицу Cust.  
  
 Эквивалентная схема XDR:  
  
```xml  
  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers"  sql:relation="Cust" >  
      <element type="CustomerID"  sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City"        sql:field="City" />  
  
   </ElementType>  
</Schema>  
```  
  
## <a name="b-bulk-loading-xml-data-in-multiple-tables"></a>Б. Массовая загрузка XML-данных в несколько таблиц  
 В этом примере XML-документ состоит из **\<Customer>** **\<Order>** элементов и.  
  
```xml  
<ROOT>  
  <Customers>  
    <CustomerID>1111</CustomerID>  
    <CompanyName>Sean Chai</CompanyName>  
    <City>NY</City>  
    <Order OrderID="1" />  
    <Order OrderID="2" />  
  </Customers>  
  <Customers>  
    <CustomerID>1112</CustomerID>  
    <CompanyName>Tom Johnston</CompanyName>  
     <City>LA</City>    
    <Order OrderID="3" />  
  </Customers>  
  <Customers>  
    <CustomerID>1113</CustomerID>  
    <CompanyName>Institute of Art</CompanyName>  
    <Order OrderID="4" />  
  </Customers>  
</ROOT>  
```  
  
 В этом примере производится Глобальная загрузка XML-данных в две таблицы: **cust** и **CustOrder**.  
  
-   Cust (CustomerID, CompanyName, City)  
  
-   CustOrder (OrderID, CustomerID)  
  
 Следующая схема XML определяет XML-представление этих таблиц. Схема указывает связь «родители-потомки» между **\<Customer>** **\<Order>** элементами и.  
  
```xml  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="CustCustOrder"  
          parent="Cust"  
          parent-key="CustomerID"  
          child="CustOrder"  
          child-key="CustomerID" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="ROOT" sql:is-constant="1" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Customers" sql:relation="Cust" >  
          <xsd:complexType>  
            <xsd:sequence>  
              <xsd:element name="CustomerID"  type="xsd:integer" />  
              <xsd:element name="CompanyName" type="xsd:string" />  
              <xsd:element name="City"        type="xsd:string" />  
              <xsd:element name="Order"   
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder" >  
                <xsd:complexType>  
                  <xsd:attribute name="OrderID" type="xsd:integer" />  
                </xsd:complexType>  
              </xsd:element>  
             </xsd:sequence>  
          </xsd:complexType>  
        </xsd:element>  
      </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 При выполнении операции XML-загрузки используется связь «первичный ключ-внешний ключ», указанная выше между **\<Cust>** элементами и, **\<CustOrder>** для выполнения групповой загрузки данных в обе таблицы.  
  
#### <a name="to-test-a-sample-bulk-load"></a>Проверка образца массовой загрузки  
  
1.  Создайте две таблицы в базе данных **tempdb** :  
  
    ```sql  
    USE tempdb;  
    CREATE TABLE Cust(  
           CustomerID  int PRIMARY KEY,  
           CompanyName varchar(20),  
           City        varchar(20));  
    CREATE TABLE CustOrder(        OrderID     int PRIMARY KEY,   
            CustomerID int FOREIGN KEY REFERENCES Cust(CustomerID));  
    ```  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Добавьте в этот файл схему XSD, приведенную в данном примере.  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleData.xml. Добавьте в этот файл XML-документ, приведенный ранее в данном примере.  
  
4.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем ValidateAndBulkload.vbs. Добавьте в этот файл код VBScript, приведенный в начале этого раздела. Измените строку соединения, указав соответствующие имена сервера и базы данных. Укажите соответствующий путь для файлов, указанных в качестве параметров метода Execute.  
  
5.  Выполните приведенный выше код VBScript. При массовой загрузке XML-данные загружаются в таблицы Cust и CustOrder.  
  
 Эквивалентная схема XDR:  
  
```xml  
  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >   
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust" >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
<sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
</Schema>  
```  
  
## <a name="c-using-chain-relationships-in-the-schema-to-bulk-load-xml"></a>В. Использование цепочечных связей в схеме для массовой загрузки XML  
 Этот пример показывает, как связь M:N, указанная в схеме сопоставления, используется для массовой XML-загрузки данных, представляющих связь M:N.  
  
 В качестве примера рассмотрим следующую схему XSD.  
  
```xml  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrderOD"  
          parent="Ord"  
          parent-key="OrderID"  
          child="OrderDetail"  
          child-key="OrderID" />  
  
    <sql:relationship name="ODProduct"  
          parent="OrderDetail"  
          parent-key="ProductID"  
          child="Product"  
          child-key="ProductID"   
          inverse="true"/>  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="ROOT" sql:is-constant="1" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Ord"   
                     sql:key-fields="OrderID" >  
          <xsd:complexType>  
            <xsd:sequence>  
             <xsd:element name="Product"  
                          sql:relation="Product"   
                          sql:key-fields="ProductID"  
                          sql:relationship="OrderOD ODProduct">  
               <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
               </xsd:complexType>  
             </xsd:element>  
           </xsd:sequence>  
           <xsd:attribute name="OrderID"   type="xsd:integer" />   
           <xsd:attribute name="CustomerID"   type="xsd:string" />  
         </xsd:complexType>  
       </xsd:element>  
      </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Схема задает **\<Order>** элемент с **\<Product>** дочерним элементом. **\<Order>** Элемент сопоставляется с таблицей **\<Product>** наведением, а элемент сопоставляется с таблицей Product в базе данных. Отношение цепочки, указанное в **\<Product>** элементе, определяет связь M:N, представленную таблицей OrderDetail. (в заказ может входить множество продуктов, а продукт может входить во множество заказов).  
  
 При массовой загрузке XML-документа с этой схемой записи добавляются к таблицам Ord, Product и OrderDetail.  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Создайте три таблицы.  
  
    ```sql  
    CREATE TABLE Ord (  
             OrderID     int  PRIMARY KEY,  
             CustomerID  varchar(5));  
    GO  
    CREATE TABLE Product (  
             ProductID   int PRIMARY KEY,  
             ProductName varchar(20));  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID));  
    GO  
    ```  
  
2.  Сохраните схему, приведенную выше в этом примере, в файле SampleSchema.xml.  
  
3.  Сохраните следующий образец XML-данных в файле SampleXMLData.xml.  
  
    ```xml  
    <ROOT>    
      <Order OrderID="1" CustomerID="ALFKI">  
        <Product ProductID="1" ProductName="Chai" />  
        <Product ProductID="2" ProductName="Chang" />  
      </Order>  
      <Order OrderID="2" CustomerID="ANATR">  
        <Product ProductID="3" ProductName="Aniseed Syrup" />  
        <Product ProductID="4" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем ValidateAndBulkload.vbs. Добавьте в этот файл код VBScript, приведенный в начале этого раздела. Измените строку соединения, указав соответствующие имена сервера и базы данных. Раскомментируйте в этом примере следующие строки исходного кода.  
  
    ```vbs  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    ```  
  
5.  Выполните код VBScript. Массовая загрузка XML загружает XML-документ в таблицы Ord и Product.  
  
## <a name="d-bulk-loading-in-identity-type-columns"></a>Г. Массовая загрузка в столбцы типа идентификаторов  
 Этот пример показывает, каким образом при массовой загрузке обрабатываются столбцы идентификаторов. В этом примере выполняется массовая загрузка данных в три таблицы (Ord, Product и OrderDetail).  
  
 В таблицах имеются следующие данные.  
  
-   OrderID в таблице Ord является столбцом типа идентификаторов.  
  
-   ProductID в таблице Product является столбцом типа идентификаторов.  
  
-   Столбцы OrderID и ProductID в таблице OrderDetail являются внешними ключевыми столбцами, ссылающимися на соответствующие первичные ключевые столбцы в таблицах Ord и Product.  
  
 Ниже перечислены схемы таблиц для этого примера.  
  
```  
Ord (OrderID, CustomerID)  
Product (ProductID, ProductName)  
OrderDetail (OrderID, ProductID)  
```  
  
 В этом примере массовой загрузки XML свойство KeepIdentity объектной модели BulkLoad имеет значение false. Поэтому [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] формирует значения идентификаторов для столбцов ProductID и OrderID соответственно (пропускаются любые значения, указанные в документах для массовой загрузки).  
  
 В этом случае массовая загрузка XML идентифицирует связь «первичный-внешний ключ» между таблицами. Сначала массовая загрузка вставляет записи в таблицы с первичным ключом, а затем распространяет значения идентификаторов, сформированные [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на таблицы с внешними ключевыми столбцами. В следующем примере массовая загрузка XML вставляет данные в таблице в следующем порядке.  
  
1.  Продукт  
  
2.  Ord  
  
3.  OrderDetail  
  
    > [!NOTE]  
    >  Чтобы распространить на внешние ключи значения идентификаторов, сформированные в таблицах Products и Orders, логика обработки требует от массовой загрузки XML отслеживания этих значений для последующей вставки в таблицу OrderDetails. Для этого массовая загрузка XML создает промежуточные таблицы, заполняет эти таблицы данными и впоследствии удаляет их.  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Создайте следующие таблицы.  
  
    ```  
    CREATE TABLE Ord (  
             OrderID     int identity(1,1)  PRIMARY KEY,  
             CustomerID  varchar(5));  
    GO  
    CREATE TABLE Product (  
             ProductID   int identity(1,1) PRIMARY KEY,  
             ProductName varchar(20));  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID));  
    GO  
    ```  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Добавьте в этот файл следующую схему XSD.  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
     <xsd:annotation>  
       <xsd:appinfo>  
        <sql:relationship name="OrderOD"  
              parent="Ord"  
              parent-key="OrderID"  
              child="OrderDetail"  
              child-key="OrderID" />  
        <sql:relationship name="ODProduct"  
              parent="OrderDetail"  
              parent-key="ProductID"  
              child="Product"  
              child-key="ProductID"   
              inverse="true"/>  
       </xsd:appinfo>  
     </xsd:annotation>  
  
      <xsd:element name="Order" sql:relation="Ord"   
                                sql:key-fields="OrderID" >  
       <xsd:complexType>  
         <xsd:sequence>  
            <xsd:element name="Product" sql:relation="Product"   
                         sql:key-fields="ProductID"  
                         sql:relationship="OrderOD ODProduct">  
              <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
              </xsd:complexType>  
            </xsd:element>  
         </xsd:sequence>  
            <xsd:attribute name="OrderID"   type="xsd:integer" />   
            <xsd:attribute name="CustomerID"   type="xsd:string" />  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleXMLData.xml. Добавьте следующий XML-документ.  
  
    ```  
    <ROOT>    
      <Order OrderID="11" CustomerID="ALFKI">  
        <Product ProductID="11" ProductName="Chai" />  
        <Product ProductID="22" ProductName="Chang" />  
      </Order>  
      <Order OrderID="22" CustomerID="ANATR">  
         <Product ProductID="33" ProductName="Aniseed Syrup" />  
        <Product ProductID="44" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем ValidateAndBulkload.vbs. Добавьте следующий код VBScript в этот файл. Измените строку соединения, указав соответствующие имена сервера и базы данных. Укажите соответствующий путь для файлов, которые служат параметрами метода **EXECUTE** .  
  
    ```  
    Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "C:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction = False  
    objBL.KeepIdentity = False  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    Set objBL = Nothing  
    MsgBox "Done."  
    ```  
  
5.  Выполните код VBScript. Массовая загрузка XML загрузит данные в соответствующие таблицы.  
  
## <a name="e-generating-table-schemas-before-bulk-loading"></a>Д. Формирование схем таблиц перед массовой загрузкой  
 При необходимости массовая загрузка XML может создать таблицы, если они не существовали до начала массовой загрузки. Установка для свойства SchemaGen объекта Склксмлбулклоад значения TRUE. При необходимости можно также запросить возможность выполнения групповой загрузки XML, чтобы удалить все существующие таблицы и создать их повторно, задав для свойства Сгдроптаблес значение TRUE. Следующий пример на языке VBScript показывает использование этих свойств.  
  
 Кроме того, в этом примере присваивается значение TRUE двум дополнительным свойствам.  
  
-   CheckConstraints. Присвоение этому свойству значения TRUE гарантирует, что данные, вставляемые в таблицы, не нарушают ограничений, указанных для таблиц (в данном случае ограничений PRIMARY KEY/FOREIGN KEY, определенных между таблицами Cust и CustOrder). При нарушении ограничений массовая загрузка завершается ошибкой.  
  
-   XMLFragment. Этому свойству должно быть присвоено значение TRUE, так как образец XML-документа (источник данных) не содержит единственного элемента верхнего уровня (и, таким образом, является фрагментом).  
  
 Код на языке VBScript.  
  
```  
Dim objBL   
Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
  
objBL.CheckConstraints=true  
objBL.XMLFragment = True  
objBL.SchemaGen = True  
objBL.SGDropTables = True  
  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
Set objBL = Nothing  
```  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Добавьте в этот файл схему XSD, приведенную в предыдущем примере «Использование цепочечных связей в схеме для массовой загрузки XML».  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleXMLData.xml. Добавьте в этот файл XML-документ, приведенный в предыдущем примере «Использование цепочечных связей в схеме для массовой загрузки XML». Удалите \<ROOT> элемент из документа (чтобы сделать его фрагментом).  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем ValidateAndBulkload.vbs. Добавьте в этот файл код VBScript из этого примера. Измените строку соединения, указав соответствующие имена сервера и базы данных. Укажите соответствующий путь для файлов, указанных в качестве параметров метода Execute.  
  
4.  Выполните код VBScript. Массовая загрузка XML создает необходимые таблицы на основе заданной схемы сопоставления и выполняет массовую загрузку данных в нее.  
  
## <a name="f-bulk-loading-from-a-stream"></a>Е. Массовая загрузка из потока  
 Метод Execute модели объектов с массовым загрузкой XML принимает два параметра. Первый параметр — это файл схемы сопоставления. Второй параметр представляет XML-данные, которые предстоит загрузить в базу данных. Существует два способа передачи XML-данных методу Execute при выполнении операции XML-загрузки.  
  
-   Указать имя файла в качестве параметра.  
  
-   Передать поток, содержащий XML-данные.  
  
 В этом примере показано, как выполнить массовую загрузку данных из потока.  
  
 Сначала код VBScript выполняет инструкцию SELECT для получения данных о заказчике из таблицы Customers в базе данных Northwind. Поскольку в инструкции SELECT указано предложение FOR XML (с параметром ELEMENTS), запрос возвращает XML-документ, состоящий из следующих элементов.  
  
```  
<Customer>  
  <CustomerID>..</CustomerID>  
  <CompanyName>..</CompanyName>  
  <City>..</City>  
</Customer>  
...  
```  
  
 Затем скрипт передает XML в виде потока в метод Execute в качестве второго параметра. Метод Execute выполняет массовый загрузку данных в таблицу Cust.  
  
 Так как этот скрипт задает для свойства SchemaGen значение TRUE, а свойству Сгдроптаблес — значение TRUE, при выполнении операции XML-загрузки в указанной базе данных создается таблица Cust. (если таблица уже существует, то она будет удалена и создана повторно).  
  
 Пример на языке VBScript.  
  
```  
Set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
Set objCmd = CreateObject("ADODB.Command")  
Set objConn = CreateObject("ADODB.Connection")  
Set objStrmOut = CreateObject ("ADODB.Stream")  
  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile     = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.SchemaGen        = True  
objBL.SGDropTables     = True  
objBL.XMLFragment      = True  
' Open a connection to the instance of SQL Server to get the source data.  
  
objConn.Open "provider=SQLOLEDB;server=(local);database=tempdb;integrated security=SSPI"  
Set objCmd.ActiveConnection = objConn  
objCmd.CommandText = "SELECT CustomerID, CompanyName, City FROM Customers FOR XML AUTO, ELEMENTS"  
  
' Open the return stream and execute the command.  
Const adCRLF = -1  
Const adExecuteStream = 1024  
objStrmOut.Open  
objStrmOut.LineSeparator = adCRLF  
objCmd.Properties("Output Stream").Value = objStrmOut  
objCmd.Execute , , adExecuteStream  
objStrmOut.Position = 0  
  
' Execute bulk load. Read source XML data from the stream.  
objBL.Execute "SampleSchema.xml", objStrmOut  
  
Set objBL = Nothing  
```  
  
 Следующая схема сопоставления XSD предоставляет необходимые сведения для создания таблицы.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:element name="ROOT" sql:is-constant="true" >  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element ref="Customers"/>  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
<xsd:element name="Customers" sql:relation="Cust" >  
  <xsd:complexType>  
    <xsd:sequence>  
      <xsd:element name="CustomerID"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(5)"/>  
      <xsd:element name="CompanyName"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(40)"/>  
      <xsd:element name="City"  
                   type="xsd:string"  
                   sql:datatype="nvarchar(40)"/>  
    </xsd:sequence>  
  </xsd:complexType>  
</xsd:element>  
</xsd:schema>  
```  
  
 Эквивалентная схема XDR.  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
    </ElementType>  
</Schema>  
```  
  
### <a name="opening-a-stream-on-an-existing-file"></a>Открытие потока для существующего файла  
 Можно также открыть поток в существующем XML-файле данных и передать его в качестве параметра методу Execute (вместо передачи имени файла в качестве параметра).  
  
 Пример передачи потока на языке Visual Basic.  
  
```  
Private Sub Form_Load()  
Dim objBL As New SQLXMLBulkLoad  
Dim objStrm As New ADODB.Stream  
Dim objFileSystem As New Scripting.FileSystemObject  
Dim objFile As Scripting.TextStream  
  
MsgBox "Begin BulkLoad..."  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.SchemaGen = True  
objBL.SGDropTables = True  
' Here again a stream is specified that contains the source data   
' (instead of the file name). But this is just an illustration.  
' Usually this is useful if you have an XML data   
' stream that is created by some other means that you want to bulk   
' load. This example starts with an XML text file, so it may not be the   
' best to use a stream (you can specify the file name directly).  
' Here you could have specified the file name itself.   
Set objFile = objFileSystem.OpenTextFile("c:\SampleData.xml")  
objStrm.Open  
objStrm.WriteText objFile.ReadAll  
objStrm.Position = 0  
objBL.Execute "c:\SampleSchema.xml", objStrm  
  
Set objBL = Nothing  
MsgBox "Done."  
End Sub  
```  
  
 Чтобы протестировать приложение, используйте XML-документ в файле (SampleData.xml) и схему XSD, приведенную в этом примере.  
  
 Источник XML-данных (SampleData.xml):  
  
```  
<ROOT>  
  <Customers>  
    <CustomerID>1111</CustomerID>  
    <CompanyName>Hanari Carnes</CompanyName>  
    <City>NY</City>  
    <Order OrderID="1" />  
    <Order OrderID="2" />  
  </Customers>  
  
  <Customers>  
    <CustomerID>1112</CustomerID>  
    <CompanyName>Toms Spezialitten</CompanyName>  
     <City>LA</City>  
    <Order OrderID="3" />  
  </Customers>  
  <Customers>  
    <CustomerID>1113</CustomerID>  
    <CompanyName>Victuailles en stock</CompanyName>  
    <Order CustomerID= "4444" OrderID="4" />  
</Customers>  
</ROOT>  
```  
  
 Эквивалентная схема XDR:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
             <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
</Schema>  
```  
  
## <a name="g-bulk-loading-in-overflow-columns"></a>Ж. Массовая загрузка в столбцы переполнения  
 Если схема сопоставления задает столбец переполнения с помощью заметки **SQL: overflow-поля** , при выполнении XML-загрузки выполняется копирование всех невостребованных данных из исходного документа в этот столбец.  
  
 Рассмотрим следующую схему XSD.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustCustOrder"  
          parent="Cust"  
          parent-key="CustomerID"  
          child="CustOrder"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  <xsd:element name="Customers" sql:relation="Cust"  
                                sql:overflow-field="OverflowColumn" >  
   <xsd:complexType>  
     <xsd:sequence>  
       <xsd:element name="CustomerID"  type="xsd:integer" />  
       <xsd:element name="CompanyName" type="xsd:string" />  
       <xsd:element name="City"        type="xsd:string" />  
       <xsd:element name="Order"   
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder" >  
         <xsd:complexType>  
          <xsd:attribute name="OrderID" type="xsd:integer" />  
          <xsd:attribute name="CustomerID" type="xsd:integer" />  
         </xsd:complexType>  
       </xsd:element>  
     </xsd:sequence>  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Схема определяет столбец переполнения (OverflowColumn) для таблицы Cust. В результате все неиспользованные XML-данные для каждого **\<Customer>** элемента добавляются в этот столбец.  
  
> [!NOTE]  
>  Все абстрактные элементы (элементы, для которых указан **абстрактный = "true"** ) и все запрещенные атрибуты (атрибуты, для которых указано **запрещенное = "true"** ) считаются переполнением при выполнении групповой загрузки XML и добавляются в столбец переполнения, если они указаны. (в противном случае они пропускаются).  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Создайте две таблицы в базе данных **tempdb** :  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust (  
                  CustomerID     int         PRIMARY KEY,  
                  CompanyName    varchar(20) NOT NULL,  
                  City           varchar(20) DEFAULT 'Seattle',  
                  OverflowColumn nvarchar(200));  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID    int PRIMARY KEY,  
                  CustomerID int FOREIGN KEY   
                                 REFERENCES Cust(CustomerID));  
    GO  
    ```  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Добавьте в этот файл схему XSD, приведенную в данном примере.  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleXMLData.xml. Добавьте в этот файл следующий XML-документ.  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City><![CDATA[NY]]> </City>  
        <Junk>garbage in overflow</Junk>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
      </Customers>  
  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <![CDATA[LA]]>   
        <!-- <xyz><address>111 Maple, Seattle</address></xyz>   -->  
        <Order OrderID="3" />  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
    </Customers>  
    </ROOT>  
    ```  
  
4.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем ValidateAndBulkload.vbs. Добавьте в этот файл следующий код на языке VBScript. Измените строку соединения, указав соответствующие имена сервера и базы данных. Укажите соответствующий путь для файлов, указанных в качестве параметров метода Execute.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Выполните код VBScript.  
  
 Эквивалентная схема XDR:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"   
                       sql:overflow-field="OverflowColumn"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
             <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
</Schema>  
```  
  
## <a name="h-specifying-the-file-path-for-temp-files-in-transaction-mode"></a>З. Задание пути к файлу для временных файлов в режиме транзакции  
 При выполнении групповой загрузки в режиме транзакций (т. е. Если свойство Transaction имеет значение TRUE) необходимо также задать свойство TempFilePath, если выполняется одно из следующих условий.  
  
-   Выполняется массовая загрузка на удаленный сервер.  
  
-   Для хранения временных файлов, созданных в режиме транзакции, необходимо использовать другой локальный диск или папку (отличный от пути, указанного переменной среды TEMP).  
  
 Например, следующий код на языке VBScript выполняет массовую загрузку данных из файла SampleXMLData.xml в таблицы базы данных в режиме транзакции. Свойство TempFilePath задано, чтобы задать путь для временных файлов, создаваемых в режиме транзакции.  
  
```  
set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
objBL.Transaction=True  
objBL.TempFilePath="\\Server\MyDir"  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
set objBL=Nothing  
```  
  
> [!NOTE]  
>  Путь к временному файлу должен быть общей папкой, доступной из учетной записи службы целевого экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и учетной записи, от которой запущено приложение массовой загрузки. Если вы не выполняете многозагрузку на локальном сервере, путь к временному файлу должен быть путем в формате UNC (например, \\ \servername\sharename).  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Создайте эту таблицу в базе данных **tempdb** :  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust (     CustomerID uniqueidentifier,   
          LastName  varchar(20));  
    GO  
    ```  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Добавьте в этот файл следующую схему XSD.  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:attribute name="CustomerID"  type="xsd:string" />  
         <xsd:attribute name="LastName" type="xsd:string" />  
       </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleXMLData.xml. Добавьте в этот файл следующий XML-документ.  
  
    ```  
    <ROOT>  
    <Customers CustomerID="6F9619FF-8B86-D011-B42D-00C04FC964FF"   
               LastName="Smith" />  
    </ROOT>  
    ```  
  
4.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем ValidateAndBulkload.vbs. Добавьте следующий код VBScript в этот файл. Измените строку соединения, указав соответствующие имена сервера и базы данных. Укажите соответствующий путь для файлов, указанных в качестве параметров метода Execute. Также укажите соответствующий путь для свойства TempFilePath.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Transaction=True  
    objBL.TempFilePath="\\server\folder"  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Выполните код VBScript.  
  
     Схема должна указывать соответствующий **тип данных SQL: DataType** для атрибута **CustomerID** , если значение для **CustomerID** указано в виде идентификатора GUID, включающего в себя фигурные скобки ({и}), например:  
  
    ```  
    <ROOT>  
    <Customers CustomerID="{6F9619FF-8B86-D011-B42D-00C04FC964FF}"   
               LastName="Smith" />  
    </ROOT>  
    ```  
  
     Обновленная схема:  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:attribute name="CustomerID"  type="xsd:string"   
                        sql:datatype="uniqueidentifier" />  
         <xsd:attribute name="LastName" type="xsd:string" />  
       </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
     Если указан **SQL: DataType** , определяющий тип столбца как **uniqueidentifier**, операция групповой загрузки удаляет фигурные скобки ({и}) из значения **CustomerID** перед вставкой в столбец.  
  
 Эквивалентная схема XDR:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
<ElementType name="ROOT" sql:is-constant="1">  
      <element type="Customers" />  
</ElementType>  
<ElementType name="Customers" sql:relation="Cust" >  
  <AttributeType name="CustomerID"  sql:datatype="uniqueidentifier" />  
  <AttributeType name="LastName"   />  
  
  <attribute type="CustomerID" />  
  <attribute type="LastName"   />  
</ElementType>  
</Schema>  
```  
  
## <a name="i-using-an-existing-database-connection-with-the-connectioncommand-property"></a>И. Использование существующего подключения к базе данных со свойством ConnectionCommand  
 Для массовой загрузки XML можно использовать существующее соединение ADO. Это может оказаться полезным в том случае, если массовая загрузка XML — лишь одна из многих операций, которые выполняются над источником данных.  
  
 Свойство ConnectionCommand позволяет использовать существующее соединение ADO с помощью объекта команды ADO. Это продемонстрировано в следующем примере на языке Visual Basic.  
  
```  
Private Sub Form_Load()  
Dim objBL As New SQLXMLBulkLoad4  
Dim objCmd As New ADODB.Command  
Dim objConn As New ADODB.Connection  
  
'Open a connection to an instance of SQL Server.  
objConn.Open "provider=SQLOLEDB;data source=(local);database=tempdb;integrated security=SSPI"  
'Ask the Command object to use the connection just established.  
Set objCmd.ActiveConnection = objConn  
  
'Tell Bulk Load to use the active command object that is using the Connection obj.  
objBL.ConnectionCommand = objCmd  
objBL.ErrorLogFile = "c:\error.log"  
objBL.CheckConstraints = True  
'The Transaction property must be set to True if you use ConnectionCommand.  
objBL.Transaction = True  
objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
Set objBL = Nothing  
End Sub  
```  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Создайте две таблицы в базе данных **tempdb** :  
  
    ```  
    USE tempdb;  
    CREATE TABLE Cust(  
                   CustomerID   varchar(5) PRIMARY KEY,  
                   CompanyName  varchar(30),  
                   City         varchar(20));  
    GO  
    CREATE TABLE CustOrder(  
                   CustomerID  varchar(5) references Cust (CustomerID),  
                   OrderID     varchar(5) PRIMARY KEY);  
    GO  
    ```  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Добавьте в этот файл следующую схему XSD.  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
    <xsd:annotation>  
      <xsd:appinfo>  
        <sql:relationship name="CustCustOrder"  
              parent="Cust"  
              parent-key="CustomerID"  
              child="CustOrder"  
              child-key="CustomerID" />  
      </xsd:appinfo>  
    </xsd:annotation>  
      <xsd:element name="ROOT" sql:is-constant="true" >  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element ref="Customers" />  
          </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
      <xsd:element name="Customers" sql:relation="Cust" >  
       <xsd:complexType>  
         <xsd:sequence>  
           <xsd:element name="CustomerID"  type="xsd:integer" />  
           <xsd:element name="CompanyName" type="xsd:string" />  
           <xsd:element name="City"        type="xsd:string" />  
           <xsd:element name="Order"   
                              sql:relation="CustOrder"  
                              sql:relationship="CustCustOrder" >  
             <xsd:complexType>  
              <xsd:attribute name="OrderID" type="xsd:integer" />  
              <xsd:attribute name="CustomerID" type="xsd:integer" />  
             </xsd:complexType>  
           </xsd:element>  
         </xsd:sequence>  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleXMLData.xml. Добавьте в этот файл следующий XML-документ.  
  
    ```  
    <ROOT>  
      <Customers>  
        <CustomerID>1111</CustomerID>  
        <CompanyName>Hanari Carnes</CompanyName>  
        <City>NY</City>  
        <Order OrderID="1" />  
        <Order OrderID="2" />  
      </Customers>  
  
      <Customers>  
        <CustomerID>1112</CustomerID>  
        <CompanyName>Toms Spezialitten</CompanyName>  
         <City>LA</City>  
        <Order OrderID="3" />  
      </Customers>  
      <Customers>  
        <CustomerID>1113</CustomerID>  
        <CompanyName>Victuailles en stock</CompanyName>  
        <Order OrderID="4" />  
    </Customers>  
    </ROOT>  
    ```  
  
4.  Создайте приложение на языке Visual Basic (стандартное EXE приложение) и предшествующий код. Добавьте эти ссылки в проект:  
  
    ```  
    Microsoft XML BulkLoad for SQL Server 4.0 Type Library  
    Microsoft ActiveX Data objects 2.6 Library  
    ```  
  
5.  Запустите приложение.  
  
 Эквивалентная схема XDR:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"   
        xmlns:dt="urn:schemas-microsoft-com:xml:datatypes"    
        xmlns:sql="urn:schemas-microsoft-com:xml-sql" >  
  
   <ElementType name="CustomerID" dt:type="int" />  
   <ElementType name="CompanyName" dt:type="string" />  
   <ElementType name="City" dt:type="string" />  
  
   <ElementType name="root" sql:is-constant="1">  
      <element type="Customers" />  
   </ElementType>  
  
   <ElementType name="Customers" sql:relation="Cust"  >  
      <element type="CustomerID" sql:field="CustomerID" />  
      <element type="CompanyName" sql:field="CompanyName" />  
      <element type="City" sql:field="City" />  
      <element type="Order" >  
         <sql:relationship  
                key-relation="Cust"  
                key="CustomerID"  
                foreign-key="CustomerID"  
                foreign-relation="CustOrder" />  
      </element>  
   </ElementType>  
    <ElementType name="Order" sql:relation="CustOrder" >  
      <AttributeType name="OrderID" />  
      <AttributeType name="CustomerID" />  
      <attribute type="OrderID" />  
      <attribute type="CustomerID" />  
    </ElementType>  
</Schema>  
```  
  
## <a name="j-bulk-loading-in-xml-data-type-columns"></a>К. Массовая загрузка XML-данных в столбцы типа данных xml  
 Если схема сопоставления задает столбец [типа данных XML](../../../t-sql/xml/xml-transact-sql.md) с помощью аннотации **SQL: datatype = "XML"** , то при XML-загрузке можно скопировать дочерние элементы XML для сопоставленного поля из исходного документа в этот столбец.  
  
 Рассмотрим следующую схему XSD, которая сопоставляет представление таблицы Production.ProductModel в образце базы данных AdventureWorks. В этой таблице поле CatalogDescription типа данных **XML** сопоставляется с **\<Desc>** элементом с помощью аннотаций **SQL: field** и **SQL: datatype = "XML"** .  
  
```  
<?xml version="1.0" encoding="utf-8" ?>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
           xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
           xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">   
  <xsd:element name="ProductModel"  sql:relation="Production.ProductModel" >  
    <xsd:complexType>  
      <xsd:sequence>  
        <xsd:element name="Name" type="xs:string"></xsd:element>  
        <xsd:element name="Desc" sql:field="CatalogDescription" sql:datatype="xml">  
        <xsd:complexType>  
          <xsd:sequence>  
            <xsd:element name="ProductDescription">  
              <xsd:complexType>  
                <xsd:sequence>  
                  <xsd:element name="Summary" type="xs:anyType"/>  
                </xsd:sequence>  
              </xsd:complexType>  
            </xsd:element>  
          </xsd:sequence>  
        </xsd:complexType>  
        </xsd:element>   
     </xsd:sequence>  
     <xsd:attribute name="ProductModelID" sql:field="ProductModelID" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Проверьте, что установлен образец базы данных AdventureWorks.  
  
2.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleSchema.xml. Скопируйте приведенную выше схему XSD, вставьте ее в файл и сохраните его.  
  
3.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем SampleXMLData.xml. Скопируйте следующий XML-документ, вставьте его в файл и сохраните его в той же папке, которая была использована в предыдущем шаге.  
  
    ```  
    <ProductModel ProductModelID="2005">  
        <Name>Mountain-100 (2005 model)</Name>  
        <Desc><?xml-stylesheet href="ProductDescription.xsl" type="text/xsl"?>  
            <p1:ProductDescription xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
                  xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
                  xmlns:wf="https://www.adventure-works.com/schemas/OtherFeatures"   
                  xmlns:html="http://www.w3.org/1999/xhtml"   
                  xmlns="">  
                <p1:Summary>  
                    <html:p>Our top-of-the-line competition mountain bike.   
          Performance-enhancing options include the innovative HL Frame,   
          super-smooth front suspension, and traction for all terrain.  
                            </html:p>  
                </p1:Summary>  
                <p1:Manufacturer>  
                    <p1:Name>AdventureWorks</p1:Name>  
                    <p1:Copyright>2002-2005</p1:Copyright>  
                    <p1:ProductURL>HTTP://www.Adventure-works.com</p1:ProductURL>  
                </p1:Manufacturer>  
                <p1:Features>These are the product highlights.   
                     <wm:Warranty>  
                        <wm:WarrantyPeriod>3 years</wm:WarrantyPeriod>  
                        <wm:Description>parts and labor</wm:Description>  
                    </wm:Warranty><wm:Maintenance>  
                        <wm:NoOfYears>10 years</wm:NoOfYears>  
                        <wm:Description>maintenance contract available through your dealer or any AdventureWorks retail store.</wm:Description>  
                    </wm:Maintenance><wf:wheel>High performance wheels.</wf:wheel><wf:saddle>  
                        <html:i>Anatomic design</html:i> and made from durable leather for a full-day of riding in comfort.</wf:saddle><wf:pedal>  
                        <html:b>Top-of-the-line</html:b> clipless pedals with adjustable tension.</wf:pedal><wf:BikeFrame>Each frame is hand-crafted in our Bothell facility to the optimum diameter   
          and wall-thickness required of a premium mountain frame.   
          The heat-treated welded aluminum frame has a larger diameter tube that absorbs the bumps.</wf:BikeFrame><wf:crankset> Triple crankset; alumunim crank arm; flawless shifting. </wf:crankset></p1:Features>  
                <!-- add one or more of these elements... one for each specific product in this product model -->  
                <p1:Picture>  
                    <p1:Angle>front</p1:Angle>  
                    <p1:Size>small</p1:Size>  
                    <p1:ProductPhotoID>118</p1:ProductPhotoID>  
                </p1:Picture>  
                <!-- add any tags in <specifications> -->  
                <p1:Specifications> These are the product specifications.  
                       <Material>Almuminum Alloy</Material><Color>Available in most colors</Color><ProductLine>Mountain bike</ProductLine><Style>Unisex</Style><RiderExperience>Advanced to Professional riders</RiderExperience></p1:Specifications>  
            </p1:ProductDescription>  
        </Desc>  
    </ProductModel>  
    ```  
  
4.  В текстовом редакторе или редакторе XML по своему выбору создайте файл и сохраните его под именем BulkloadXml.vbs. Скопируйте следующий код VBScript и вставьте его в файл. Сохраните в той же папке, которая была использована для предыдущих файлов XML-данных и схемы.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=MyServer;database=AdventureWorks;integrated security=SSPI"  
  
    Dim fso, sAppPath  
    Set fso = CreateObject("Scripting.FileSystemObject")   
    sAppPath = fso.GetFolder(".")   
  
    objBL.ErrorLogFile = sAppPath & "\error.log"  
  
    'Execute XML bulkload using file.  
    objBL.Execute "SampleSchema.xml", "SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
5.  Выполните скрипт BulkloadXml.vbs.  
  
  

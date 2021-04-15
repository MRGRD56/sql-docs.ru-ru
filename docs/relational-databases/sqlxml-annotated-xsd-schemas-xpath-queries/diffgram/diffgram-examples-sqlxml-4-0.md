---
title: Примеры DiffGram (SQLXML)
description: Просмотрите примеры дельтами в SQLXML 4,0, которые выполняют операции вставки, обновления и удаления в базе данных.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- DiffGrams [SQLXML], examples
- examples [SQLXML], DiffGram
- diffgr:parentID
- parentID annotation
ms.assetid: fc148583-dfd3-4efb-a413-f47b150b0975
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 02bdf0338e4393bd08add1ab1c8dcd4c0735b14a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491866"
---
# <a name="diffgram-examples-sqlxml-40"></a>Примеры дельт (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Примеры в данном разделе состоят из нескольких дельт (DiffGram), которые выполняют операции вставки, обновления и удаления в базе данных. При использовании этих примеров необходимо учитывать следующие моменты.  
  
-   Примеры используют две таблицы (Cust и Ord), которые необходимо создать, если требуется проверить примеры дельт:  
  
    ```  
    Cust(CustomerID, CompanyName, ContactName)  
    Ord(OrderID, CustomerID)  
    ```  
  
-   Большинство примеров в данном разделе используют следующую схему XSD:  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  
    <xsd:annotation>  
      <xsd:documentation>  
        Diffgram Customers/Orders Schema.  
      </xsd:documentation>  
      <xsd:appinfo>  
           <sql:relationship name="CustomersOrders"   
                      parent="Cust"  
                      parent-key="CustomerID"  
                      child-key="CustomerID"  
                      child="Ord"/>  
      </xsd:appinfo>  
    </xsd:annotation>  
  
    <xsd:element name="Customer" sql:relation="Cust">  
      <xsd:complexType>  
        <xsd:sequence>  
          <xsd:element name="CompanyName"    type="xsd:string"/>  
          <xsd:element name="ContactName"    type="xsd:string"/>  
           <xsd:element name="Order" sql:relation="Ord" sql:relationship="CustomersOrders">  
            <xsd:complexType>  
              <xsd:attribute name="OrderID" type="xsd:int" sql:field="OrderID"/>  
              <xsd:attribute name="CustomerID" type="xsd:string"/>  
            </xsd:complexType>  
          </xsd:element>  
        </xsd:sequence>  
        <xsd:attribute name="CustomerID" type="xsd:string" sql:field="CustomerID"/>  
      </xsd:complexType>  
    </xsd:element>  
  
    </xsd:schema>     
    ```  
  
     Сохраните данную схему под именем DiffGramSchema.xml в той же папке, где находятся остальные файлы, используемые в примерах.  
  
## <a name="a-deleting-a-record-by-using-a-diffgram"></a>A. Удаление записи с помощью дельты  
 Дельта в этом примере удаляет запись пользователя (с атрибутом CustomerID, имеющим значение ALFKI) из таблицы Cust и удаляет соответствующую запись заказа (с атрибутом OrderID со значением 1) из таблицы Ord.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
           xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
           xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance/>  
  
    <diffgr:before>  
        <Order diffgr:id="Order1"   
               msdata:rowOrder="0"   
               CustomerID="ALFKI"   
               OrderID="1"/>  
        <Customer diffgr:id="Customer1"   
                  msdata:rowOrder="0"   
                  CustomerID="ALFKI">  
           <CompanyName>Alfreds Futterkiste</CompanyName>  
           <ContactName>Maria Anders</ContactName>  
        </Customer>  
    </diffgr:before>  
    <msdata:errors/>  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 В **\<before>** блоке есть **\<Order>** элемент (**diffgr: ID = "Order1"**) и **\<Customer>** элемент (**diffgr: ID = "Customer1"**). Данные элементы представляют существующие записи в базе данных. **\<DataInstance>** Элемент не имеет соответствующих записей (с тем же **идентификатором diffgr: ID**). Это указывает на операцию удаления.  
  
#### <a name="to-test-the-diffgram"></a>Проверка дельты  
  
1.  Создайте эти таблицы в базе данных **tempdb** .  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Добавьте следующий образец данных:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Скопируйте приведенную выше дельту и вставьте ее в текстовый файл. Сохраните файл с именем MyDiffGram.xml в папке, которая использовалась на предыдущем шаге.  
  
4.  Скопируйте схему DiffGramSchema, приведенную ранее в этом разделе, и вставьте ее в текстовый файл. Сохраните файл с именем DiffGramSchema.xml.  
  
5.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить дельту.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="b-inserting-a-record-by-using-a-diffgram"></a>Б. Вставка записи с помощью дельты  
 В этом примере дельта вставляет по одной записи в таблицы Cust и Ord.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql"   
      sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
          xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
          xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
       <Customer diffgr:id="Customer1" msdata:rowOrder="0"    
                 diffgr:hasChanges="inserted" CustomerID="ALFKI">  
        <CompanyName>C3Company</CompanyName>  
        <ContactName>C3Contact</ContactName>  
        <Order diffgr:id="Order1"   
               msdata:rowOrder="0"  
               diffgr:hasChanges="inserted"   
               CustomerID="ALFKI" OrderID="1"/>  
      </Customer>  
    </DataInstance>  
  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 В этом DiffGram **\<before>** блок не задан (не определены существующие записи базы данных). Существуют два экземпляра записи (определяемые **\<Customer>** **\<Order>** элементами и в **\<DataInstance>** блоке), которые сопоставляются с таблицами Cust и намерены соответственно. Оба этих элемента указывают атрибут **diffgr: hasChanges** (**HasChanges = "inserted"**). Это указывает на операцию вставки. В этом DiffGram при указании **HasChanges = "Modified"** вы указываете, что хотите изменить несуществующую запись, что приведет к ошибке.  
  
#### <a name="to-test-the-diffgram"></a>Проверка дельты  
  
1.  Создайте эти таблицы в базе данных **tempdb** .  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Добавьте следующий образец данных:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Скопируйте приведенную выше дельту и вставьте ее в текстовый файл. Сохраните файл с именем MyDiffGram.xml в папке, которая использовалась на предыдущем шаге.  
  
4.  Скопируйте схему DiffGramSchema, приведенную ранее в этом разделе, и вставьте ее в текстовый файл. Сохраните файл с именем DiffGramSchema.xml.  
  
5.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить дельту.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="c-updating-an-existing-record-by-using-a-diffgram"></a>В. Обновление существующей записи с помощью дельты  
 В этом примере дельта обновляет CompanyName и ContactName для клиента ALFKI.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
           xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
           xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
      <Customer diffgr:id="Customer1"   
                msdata:rowOrder="0" diffgr:hasChanges="modified"   
                CustomerID="ALFKI">  
          <CompanyName>Bottom Dollar Markets</CompanyName>  
          <ContactName>Antonio Moreno</ContactName>  
      </Customer>  
    </DataInstance>  
  
    <diffgr:before>  
     <Customer diffgr:id="Customer1"   
               msdata:rowOrder="0"   
               CustomerID="ALFKI">  
        <CompanyName>Alfreds Futterkiste</CompanyName>  
        <ContactName>Maria Anders</ContactName>  
      </Customer>  
    </diffgr:before>  
  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 **\<before>** Блок включает **\<Customer>** элемент (**diffgr: ID = "Customer1"**). **\<DataInstance>** Блок включает соответствующий **\<Customer>** элемент с тем же **идентификатором**. **\<customer>** Элемент в **\<NewDataSet>** также указывает **diffgr: hasChanges = "Modified"**. Это указывает на операцию обновления, и запись клиента в таблице **cust** соответствующим образом обновляется. Обратите внимание, что если атрибут **diffgr: hasChanges** не указан, логика обработки DiffGram игнорирует этот элемент и обновления не выполняются.  
  
#### <a name="to-test-the-diffgram"></a>Проверка дельты  
  
1.  Создайте эти таблицы в базе данных **tempdb** .  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Добавьте следующий образец данных:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Скопируйте приведенную выше дельту и вставьте ее в текстовый файл. Сохраните файл с именем MyDiffGram.xml в папке, которая использовалась на предыдущем шаге.  
  
4.  Скопируйте схему DiffGramSchema, приведенную ранее в этом разделе, и вставьте ее в текстовый файл. Сохраните файл с именем DiffGramSchema.xml.  
  
5.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить дельту.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="d-inserting-updating-and-deleting-records-by-using-a-diffgram"></a>Г. Вставка, обновление и удаление записей с помощью дельты  
 В этом примере относительно сложная дельта используется для выполнения операций вставки, обновления и удаления.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql" sql:mapping-schema="DiffGramSchema.xml">  
  <diffgr:diffgram   
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"   
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
    <DataInstance>  
      <Customer diffgr:id="Customer2" msdata:rowOrder="1"   
                diffgr:hasChanges="modified"   
                CustomerID="ANATR">  
          <CompanyName>Bottom Dollar Markets</CompanyName>  
          <ContactName>Elizabeth Lincoln</ContactName>  
          <Order diffgr:id="Order2" msdata:rowOrder="1"   
               msdata:hiddenCustomerID="ANATR"   
               CustomerID="ANATR" OrderID="2"/>  
      </Customer>  
  
      <Customer diffgr:id="Customer3" msdata:rowOrder="2"   
                CustomerID="ANTON">  
         <CompanyName>Chop-suey Chinese</CompanyName>  
         <ContactName>Yang Wang</ContactName>  
         <Order diffgr:id="Order3" msdata:rowOrder="2"   
               msdata:hiddenCustomerID="ANTON"   
               CustomerID="ANTON" OrderID="3"/>  
      </Customer>  
      <Customer diffgr:id="Customer4" msdata:rowOrder="3"   
                diffgr:hasChanges="inserted"   
                CustomerID="AROUT">  
         <CompanyName>Around the Horn</CompanyName>  
         <ContactName>Thomas Hardy</ContactName>  
         <Order diffgr:id="Order4" msdata:rowOrder="3"   
                diffgr:hasChanges="inserted"   
                msdata:hiddenCustomerID="AROUT"   
               CustomerID="AROUT" OrderID="4"/>  
      </Customer>  
    </DataInstance>  
    <diffgr:before>  
      <Order diffgr:id="Order1" msdata:rowOrder="0"   
             msdata:hiddenCustomerID="ALFKI"   
             CustomerID="ALFKI" OrderID="1"/>  
      <Customer diffgr:id="Customer1" msdata:rowOrder="0"   
                CustomerID="ALFKI">  
        <CompanyName>Alfreds Futterkiste</CompanyName>  
        <ContactName>Maria Anders</ContactName>  
      </Customer>  
      <Customer diffgr:id="Customer2" msdata:rowOrder="1"   
                CustomerID="ANATR">  
        <CompanyName>Ana Trujillo Emparedados y helados</CompanyName>  
        <ContactName>Ana Trujillo</ContactName>  
      </Customer>  
    </diffgr:before>  
  </diffgr:diffgram>  
</ROOT>  
```  
  
 Логика работы с дельтами обрабатывает эту дельту следующим образом.  
  
-   В соответствии с логикой обработки DiffGram все элементы верхнего уровня в **\<before>** блоке сопоставлены с соответствующими таблицами, как описано в схеме сопоставления.  
  
-   **\<before>** Блок содержит **\<Order>** элемент (**дффгр: ID = "Order1"**) и **\<Customer>** элемент (**diffgr: ID = "Customer1"**), для которого нет соответствующего элемента в **\<DataInstance>** блоке (с таким же идентификатором). Это указывает на операцию удаления, во время которой записи удаляются из таблиц Cust и Ord.  
  
-   **\<before>** Блок содержит **\<Customer>** элемент (**diffgr: ID = "Customer2"**), для которого имеется соответствующий **\<Customer>** элемент в **\<DataInstance>** БЛОКе (с таким же идентификатором). Элемент в **\<DataInstance>** блоке указывает **diffgr: hasChanges = "Modified"**. Это операция обновления, в которой для АНАТР клиента сведения о CompanyName и ContactName обновляются в таблице Cust с использованием значений, указанных в **\<DataInstance>** блоке.  
  
-   **\<DataInstance>** Блок содержит **\<Customer>** элемент (**diffgr: ID = "Customer3"**) и **\<Order>** элемент (**diffgr: ID = "Order3"**). Ни один из этих элементов не указал атрибут **diffgr: hasChanges** . Таким образом, логика обработки дельт не учитывает эти элементы.  
  
-   **\<DataInstance>** Блок содержит **\<Customer>** элемент (**diffgr: ID = "Customer4"**) и **\<Order>** элемент (**diffgr: ID = "Order4"**), для которого нет соответствующих элементов в \<before> блоке. Эти элементы в **\<DataInstance>** блоке указывают **diffgr: hasChanges = "inserted"**. Таким образом, в таблицы Cust и Ord добавляется новая запись.  
  
#### <a name="to-test-the-diffgram"></a>Проверка дельты  
  
1.  Создайте следующие таблицы в базе данных **tempdb** .  
  
    ```  
    CREATE TABLE Cust(  
            CustomerID  nchar(5) Primary Key,  
            CompanyName nvarchar(40) NOT NULL ,  
            ContactName nvarchar(60) NULL)  
    GO  
  
    CREATE TABLE Ord(  
       OrderID    int Primary Key,  
       CustomerID nchar(5) Foreign Key REFERENCES Cust(CustomerID))  
    GO  
    ```  
  
2.  Добавьте следующий образец данных:  
  
    ```  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ALFKI', N'Alfreds Futterkiste', N'Maria Anders')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANATR', N'Ana Trujillo Emparedados y helados', N'Ana Trujillo')  
    INSERT INTO Cust(CustomerID, CompanyName, ContactName) VALUES  
         (N'ANTON', N'Antonio Moreno Taquería', N'Antonio Moreno')  
  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(1, N'ALFKI')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(2, N'ANATR')  
    INSERT INTO Ord(OrderID, CustomerID) VALUES(3, N'ANTON')  
    ```  
  
3.  Скопируйте приведенную выше дельту и вставьте ее в текстовый файл. Сохраните файл с именем MyDiffGram.xml в папке, которая использовалась на предыдущем шаге.  
  
4.  Скопируйте схему DiffGramSchema, приведенную ранее в этом разделе, и вставьте ее в текстовый файл. Сохраните файл с именем DiffGramSchema.xml.  
  
5.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить дельту.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
## <a name="e-applying-updates-by-using-a-diffgram-with-the-diffgrparentid-annotation"></a>Д. Применение обновлений с помощью дельты с заметкой diffgr:parentID  
 В этом примере показано, как заметка **ParentID** , указанная в **\<before>** блоке DiffGram, используется при применении обновлений.  
  
```  
<NewDataSet />  
<diffgr:before>  
   <Order diffgr:id="Order1" msdata:rowOrder="0" OrderID="2" />  
   <Order diffgr:id="Order3" msdata:rowOrder="2" OrderID="4" />  
  
   <OrderDetail diffgr:id="OrderDetail1"   
                diffgr:parentId="Order1"   
                msdata:rowOrder="0"   
                ProductID="13"   
                OrderID="2" />  
   <OrderDetail diffgr:id="OrderDetail3"   
                diffgr:parentId="Order3"  
                ProductID="77"  
                OrderID="4"/>  
</diffgr:before>  
</diffgr:diffgram>  
```  
  
 Этот DiffGram указывает операцию удаления, так как существует только **\<before>** блок. В DiffGram заметка **ParentID** используется для указания связи типа «родители-потомки» между заказами и сведениями о заказе. При удалении записей посредством SQLXML они удаляются из дочерней таблицы, указанной этой связью, а после этого из соответствующей родительской таблицы.  
  
  

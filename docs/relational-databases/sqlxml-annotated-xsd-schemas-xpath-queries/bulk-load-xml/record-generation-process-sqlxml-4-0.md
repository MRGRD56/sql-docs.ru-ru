---
title: Процесс создания записей (SQLXML)
description: Изучите подробные сведения о процессе создания записей при выполнении операции XML-загрузки в SQLXML 4,0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XML Bulk Load [SQLXML], record generation process
- node scopes [SQLXML]
- record subsets [SQLXML]
- scope [SQLXML]
- key ordering rules [SQLXML]
- record generation process for bulk loads [SQLXML]
- entering node scope [SQLXML]
- bulk load [SQLXML], record generation process
- leaving node scope [SQLXML]
- schema mapping [SQLXML]
ms.assetid: d8885bbe-6f15-4fb9-9684-ca7883cfe9ac
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cd81acac3c40baea509b46032316ffd36e502c64
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490628"
---
# <a name="record-generation-process-sqlxml-40"></a>Процесс создания записей (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Массовая загрузка XML обрабатывает входные XML-данные и готовит записи для соответствующих таблиц в Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Логика массовой загрузки XML определяет, когда формируется новая запись, какие значения дочерних элементов или атрибута копировать в поля записи, и когда запись завершена и готова к отправке в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для вставки.  
  
 Массовая загрузка XML не загружает все входные XML-данные в память и не формирует полные наборы записей перед отправкой данных в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Это объясняется тем, что входные XML-данные могут быть большим документом, и загрузка всего документа в память может быть дорогостоящей. Вместо этого массовая загрузка XML выполняет следующие действия.  
  
1.  Анализирует схему сопоставления и готовит необходимый план выполнения.  
  
2.  Применяет план выполнения к данным во входном потоке.  
  
 В связи с такой последовательной обработкой важно представить входные XML-данные определенным образом. Необходимо понимать, как массовая загрузка XML анализирует схему сопоставления и как происходит процесс создания записи. Это поможет предоставить схему сопоставления для массовой загрузки XML, которая даст нужные результаты.  
  
 Массовая загрузка XML обрабатывает общие заметки схемы сопоставления, в том числе сопоставления столбцов и таблиц (указанных явно с помощью заметок или неявно через сопоставление по умолчанию) и связи соединений.  
  
> [!NOTE]  
>  Предполагается, что пользователь знаком со схемами сопоставления XSD и XDR с заметками. Дополнительные сведения о схемах см. в статьях [Знакомство с заметками XSD-схем &#40;sqlxml 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md) или [схемой XDR с аннотацией &#40;не рекомендуется в SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
 Чтобы понять процесс создания записей, необходимо понимать следующие концепции.  
  
-   Область узла  
  
-   Правило создания записей  
  
-   Подмножество записей и правило упорядочения ключа  
  
-   Исключения из правила создания записей  
  
## <a name="scope-of-a-node"></a>Область узла  
 Узел (элемент или атрибут) в XML-документе переходит *в область* , когда при выполнении XML-загрузки в поток входных данных XML возникает ошибка. Для узла элемента открывающий тег элемента вводит элемент в область. Для узла атрибута имя атрибута вводит атрибут в область.  
  
 Узел выходит из области, когда в нем не остается данных: по закрывающему тегу (в случае узла элемента) или по окончанию значения атрибута (в случае узла атрибута).  
  
## <a name="record-generation-rule"></a>Правило создания записей  
 Когда узел (элемент или атрибут) входит в область, появляется возможность формирования записи из этого узла. Запись существует так же долго, как связанный узел в области. Когда узел выходит из области, массовая загрузка XML рассматривает сформированную запись как завершенную (с данными) и отправляет ее в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для вставки.  
  
 Например, рассмотрим следующий фрагмент схемы XSD:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="Customer" sql:relation="Customers" >  
   <xsd:complexType>  
     <xsd:attribute name="CustomerID" type="xsd:string" />  
     <xsd:attribute name="CompanyName" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
 Схема задает **\<Customer>** элемент с атрибутами **CustomerID** и **CompanyName** . Аннотация **SQL: relation** сопоставляет **\<Customer>** элемент с таблицей Customers.  
  
 Рассмотрим следующий фрагмент XML-документа:  
  
```  
<Customer CustomerID="1" CompanyName="xyz" />  
<Customer CustomerID="2" CompanyName="abc" />  
...  
```  
  
 Когда массовой загрузке XML предоставляется схема, описанная в предыдущих абзацах, и XML-данные в качестве входных данных, она обрабатывает узлы (элементы и атрибуты) в источнике данных следующим образом.  
  
-   Открывающий тег первого **\<Customer>** элемента приводит этот элемент в области. Этот узел сопоставляется с таблицей Customers. Поэтому массовая загрузка XML формирует запись для таблицы Customers.  
  
-   В схеме все атрибуты **\<Customer>** элемента сопоставляются со столбцами таблицы Customers. По мере входа этих атрибутов в область, массовая загрузка XML копирует их значения в запись клиента, уже сформированную родительской областью.  
  
-   Когда при выполнении групповой загрузки XML достигается закрывающий тег **\<Customer>** элемента, элемент выходит за пределы области. В результате массовая загрузка XML рассматривает запись как завершенную и отправляет ее в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 При выполнении операции XML-загрузки для каждого последующего элемента следует выполнить этот процесс **\<Customer>** .  
  
> [!IMPORTANT]  
>  В этой модели, поскольку запись вставляется при достижении закрывающего тега (или выхода узла из области), необходимо определить все данные, связанные с записью в области узла.  
  
## <a name="record-subset-and-the-key-ordering-rule"></a>Подмножество записей и правило упорядочения ключа  
 При указании схемы сопоставления, в которой используется **\<sql:relationship>** , термин подмножества ссылается на набор записей, создаваемых на внешней стороне связи. В следующем примере записи CustOrder находятся на внешней стороне, **\<sql:relationship>** .  
  
 Например, пусть база данных содержит следующие таблицы.  
  
-   Cust (CustomerID, CompanyName, City);  
  
-   CustOrder (CustomerID, OrderID).  
  
 Столбец CustomerID в таблице CustOrder является внешним ключом, под которым понимается первичный ключ CustomerID в таблице Cust.  
  
 Теперь рассмотрим представление XML, указанное в следующей схеме XSD с заметками. Эта схема использует **\<sql:relationship>** для указания связи между таблицами Cust и CustOrder.  
  
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
</xsd:schema>  
```  
  
 Ниже приведен образец XML-данных и шаги для создания рабочего образца.  
  
-   Когда **\<Customer>** узел элемента в XML-файле данных входит в область, при выполнении операции XML-загрузки создается запись для таблицы Cust. При выполнении операции XML-загрузки выполняется копирование необходимых значений столбцов (CustomerID, CompanyName и City) из **\<CustomerID>** , **\<CompanyName>** и **\<City>** дочерних элементов, вводимых в область.  
  
-   Когда **\<Order>** узел Element входит в область, при выполнении групповой загрузки XML создается запись для таблицы CustOrder. При выполнении операции XML-загрузки в эту запись копируется значение атрибута **OrderID** . Значение, необходимое для столбца CustomerID, получено из **\<CustomerID>** дочернего элемента **\<Customer>** элемента. При выполнении групповой загрузки XML данные, указанные в, используются **\<sql:relationship>** для получения значения внешнего ключа CustomerID для этой записи, если только атрибут **CustomerID** не был указан в **\<Order>** элементе. Общее правило заключается в том, что если дочерний элемент явно указывает значение атрибута внешнего ключа, при выполнении XML-загрузки с помощью указанного значения не получается значение из родительского элемента **\<sql:relationship>** . Поскольку данный **\<Order>** узел элемента выходит за пределы области, при выполнении операции XML-загрузки происходит отправка записи в, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] а затем обработка всех последующих **\<Order>** узлов элементов аналогичным образом.  
  
-   Наконец, **\<Customer>** узел Element выходит за пределы области. В это время массовая загрузка XML отправляет запись клиента в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Массовая загрузка XML продолжает этот процесс для всех последующих клиентов в потоке XML-данных.  
  
 Два замечания о схеме сопоставления.  
  
-   Если схема удовлетворяет правилу "вложенности" (например, все данные, связанные с клиентом и заказом, определяются в области связанных **\<Customer>** **\<Order>** узлов элементов и), то при выполнении такой загрузки происходит успешная операция.  
  
-   В описании **\<Customer>** элемента его дочерние элементы указываются в соответствующем порядке. В этом случае **\<CustomerID>** дочерний элемент указывается перед **\<Order>** дочерним элементом. Это означает, что во входном XML-файле данных **\<CustomerID>** значение элемента доступно как значение внешнего ключа, когда **\<Order>** элемент входит в область. Первыми указываются ключевые атрибуты — это «правило упорядочения ключа».  
  
     При указании **\<CustomerID>** дочернего элемента после **\<Order>** дочернего элемента значение недоступно, если **\<Order>** элемент входит в область. После того как **\</Order>** закрывающий тег считывается, запись для таблицы CustOrder считается завершенной и вставляется в таблицу CustOrder со ЗНАЧЕНИЕМ NULL для столбца CustomerID, что не является нужным результатом.  
  
#### <a name="to-create-a-working-sample"></a>Создание рабочего образца  
  
1.  Сохраните схему, приведенную в этом примере, в файле SampleSchema.xml.  
  
2.  Создайте следующие таблицы.  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int         PRIMARY KEY,  
                  CompanyName    varchar(20) NOT NULL,  
                  City           varchar(20) DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                 OrderID        int         PRIMARY KEY,  
                 CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID))  
    GO  
    ```  
  
3.  Сохраните следующий образец входных XML-данных в файле SampleXMLData.xml:  
  
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
  
4.  Чтобы выполнить массовую загрузку XML-данных, сохраните следующий пример на языке [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript) в файле BulkLoad.vbs и выполните его.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
## <a name="exceptions-to-the-record-generation-rule"></a>Исключения из правила создания записей  
 Массовая загрузка XML не формирует запись для узла, когда он входит в область, если этот узел типа IDREF или IDREFS. Необходимо убедиться, что в схеме приведено полное описание записи. Заметки **DT: Type = "NMTOKENS"** пропускаются, так как тип IDREFS игнорируется.  
  
 Например, рассмотрим следующую схему XSD, которая описывает **\<Customer>** элементы и **\<Order>** . **\<Customer>** Элемент включает атрибут **ORDERLIST** типа IDREFS. **\<sql:relationship>** Тег указывает связь «один ко многим» между клиентом и списком заказов.  
  
 Схема:  
  
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
  
  <xsd:element name="Customers" sql:relation="Cust" >  
   <xsd:complexType>  
    <xsd:attribute name="CustomerID" type="xsd:integer" />  
    <xsd:attribute name="CompanyName" type="xsd:string" />  
    <xsd:attribute name="City" type="xsd:string" />  
    <xsd:attribute name="OrderList"   
                       type="xsd:IDREFS"   
                       sql:relation="CustOrder"   
                       sql:field="OrderID"  
                       sql:relationship="CustCustOrder" >  
    </xsd:attribute>  
  </xsd:complexType>  
 </xsd:element>  
  
  <xsd:element name="Order" sql:relation="CustOrder" >  
   <xsd:complexType>  
    <xsd:attribute name="OrderID" type="xsd:string" />  
    <xsd:attribute name="CustomerID" type="xsd:integer" />  
    <xsd:attribute name="OrderDate" type="xsd:date" />  
  </xsd:complexType>  
 </xsd:element>  
</xsd:schema>  
```  
  
 Так как при выполнении групповой загрузки игнорируются узлы типа IDREFS, при переходе узла атрибута **OrderList** в область не создается никаких операций создания записей. Поэтому, если нужно упорядочить записи, добавленные в таблицу Orders, необходимо описать эти заказы в каком-то месте схемы. В этой схеме указание **\<Order>** элемента гарантирует, что при выполнении операции XML-загрузки в таблицу Orders добавляются записи о заказах. **\<Order>** Элемент описывает все атрибуты, необходимые для заполнения записи для таблицы CustOrder.  
  
 Необходимо убедиться, что значения **CustomerID** и **OrderID** в **\<Customer>** элементе соответствуют значениям в **\<Order>** элементе. Программист ответственен за обеспечение ссылочной целостности.  
  
#### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Создайте следующие таблицы.  
  
    ```  
    CREATE TABLE Cust (  
                  CustomerID     int          PRIMARY KEY,  
                  CompanyName    varchar(20)  NOT NULL,  
                  City           varchar(20)  DEFAULT 'Seattle')  
    GO  
    CREATE TABLE CustOrder (  
                  OrderID        varchar(10) PRIMARY KEY,  
                  CustomerID     int         FOREIGN KEY REFERENCES                                          Cust(CustomerID),  
                  OrderDate      datetime DEFAULT '2000-01-01')  
    GO  
    ```  
  
2.  Сохраните схему сопоставления, приведенную в этом примере, в файле SampleSchema.xml.  
  
3.  Сохраните следующий образец XML-данных в файле SampleXMLData.xml.  
  
    ```  
    <ROOT>  
      <Customers CustomerID="1111" CompanyName="Sean Chai" City="NY"  
                 OrderList="Ord1 Ord2" />  
      <Customers CustomerID="1112" CompanyName="Dont Know" City="LA"  
                 OrderList="Ord3 Ord4" />  
      <Order OrderID="Ord1" CustomerID="1111" OrderDate="1999-01-01" />  
      <Order OrderID="Ord2" CustomerID="1111" OrderDate="1999-02-01" />  
      <Order OrderID="Ord3" CustomerID="1112" OrderDate="1999-03-01" />  
      <Order OrderID="Ord4" CustomerID="1112" OrderDate="1999-04-01" />  
    </ROOT>  
    ```  
  
4.  Чтобы выполнить массовую загрузку XML-данных, сохраните этот пример на языке VBScript в файле Sample.vbs и выполните его.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints=True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
  

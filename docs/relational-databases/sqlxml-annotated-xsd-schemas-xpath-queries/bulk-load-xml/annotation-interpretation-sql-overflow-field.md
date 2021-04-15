---
title: 'SQL: overflow-поле (SQLXML)'
description: 'Узнайте, как использовать заметку "SQL: overflow-поле", чтобы указать столбец в качестве столбца переполнения, который будет получать все невостребованные данные из XML-документа.'
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- overflow-field annotation
- unconsumed data
- overflow data [SQLXML]
- sql:overflow-field
ms.assetid: f005182b-6151-432d-ab22-3bc025742cd3
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a79f03a282083477d0b648138c0a5ecb2c19a80
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490672"
---
# <a name="annotation-interpretation---sqloverflow-field"></a>Интерпретация заметки — sql:overflow-field
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  В схеме можно указать столбец как столбец переполнения, чтобы получить все невостребованные данные из XML-документа. Этот столбец указан в схеме с помощью заметки **SQL: overflow-поля** . Возможно иметь несколько столбцов переполнения.  
  
 Каждый раз, когда XML-узел (элемент или атрибут), для которого определена заметка **"SQL: переполнение поля** ", входит в область, столбец Overflow активируется и получает невостребованные данные. Когда узел выходит за границы области, столбец переполнения перестает быть активным, и процедура массовой загрузки XML делает активным предыдущее поле переполнения (если таковое имеется).  
  
 При хранении данных в столбце Overflow при выполнении групповой загрузки XML также сохраняются открывающий и закрывающий теги родительского элемента, для которого определено **поле SQL: overflow-field** .  
  
 Например, следующая схема описывает **\<Customers>** **\<CustOrder>** элементы и. Каждый из данных элементов определяет столбец переполнения:  
  
```  
<?xml version="1.0" ?>  
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
 <xsd:element name="ROOT" sql:is-constant="1">  
  <xsd:complexType>  
    <xsd:sequence>   
      <xsd:element name="Customers"   
                   sql:relation="Cust"  
                   sql:overflow-field="OverflowColumn">  
        <xsd:complexType>  
             <xsd:sequence>   
             <xsd:element name="CustomerID" type="xsd:integer"/>  
             <xsd:element name="CompanyName"  type="xsd:string"/>  
             <xsd:element name="City" type="xsd:string"/>  
             <xsd:element name="Order"  
                          sql:relation="CustOrder"  
                          sql:relationship="CustCustOrder"  
                          sql:overflow-field="OverflowColumn">  
               <xsd:complexType>  
                 <xsd:attribute name="OrderID"/>  
                 <xsd:attribute name="CustomerID"/>  
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
  
 В схеме **\<Customer>** элемент сопоставляется с таблицей Cust, а **\<Order>** элемент сопоставляется с таблицей CustOrder.  
  
 Оба **\<Customer>** элемента и **\<Order>** обозначают столбец Overflow. Таким словами, при выполнении операции XML-загрузки происходит сохранение всех неиспользованных дочерних элементов и атрибутов **\<Customer>** элемента в столбце Overflow таблицы Cust и всех невостребованных дочерних элементов и атрибутов **\<Order>** элемента в столбце Overflow таблицы CustOrder.  
  
### <a name="to-test-a-working-sample"></a>Проверка рабочего образца  
  
1.  Сохраните схему, приведенную в этом примере, в файле SampleSchema.xml.  
  
2.  Создайте следующие таблицы.  
  
    ```  
    CREATE TABLE Cust (  
              CustomerID     int         PRIMARY KEY,  
              CompanyName    varchar(20) NOT NULL,  
              City           varchar(20) DEFAULT 'Seattle',  
              OverflowColumn nvarchar(200))  
    GO  
    CREATE TABLE CustOrder (  
              OrderID        int         PRIMARY KEY,  
              CustomerID     int         FOREIGN KEY REFERENCES  
                                              Cust(CustomerID),  
              OverflowColumn nvarchar(200))  
    GO  
    ```  
  
3.  Сохраните следующий образец XML-данных в файле SampleXMLData.xml.  
  
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
       <City><![CDATA[LA]]> </City>  
       <xyz><address>111 Maple, Seattle</address></xyz>     
       <Order OrderID="3" />  
     </Customers>  
       <Customers>  
       <CustomerID>1113</CustomerID>  
       <CompanyName>Victuailles en stock</CompanyName>  
       <Order OrderID="4" />  
      </Customers>  
    </ROOT>  
    ```  
  
4.  Чтобы выполнить массовую загрузку XML-данных, сохраните этот пример на языке [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic Scripting Edition (VBScript) в файле Sample.vbs и выполните его.  
  
    ```  
    set objBL = CreateObject("SQLXMLBulkLoad.SQLXMLBulkload.4.0")  
    objBL.ConnectionString = "provider=SQLOLEDB;data source=localhost;database=tempdb;integrated security=SSPI"  
    objBL.ErrorLogFile = "c:\error.log"  
    objBL.CheckConstraints = True  
    objBL.Execute "c:\SampleSchema.xml", "c:\SampleXMLData.xml"  
    set objBL=Nothing  
    ```  
  
  

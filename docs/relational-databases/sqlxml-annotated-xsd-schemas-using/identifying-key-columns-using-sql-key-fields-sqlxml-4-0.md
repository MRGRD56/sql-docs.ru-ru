---
title: 'Указание ключевых столбцов с помощью SQL: Key-Fields (SQLXML)'
description: 'Узнайте, как обеспечить правильное вложение в результат запроса SQLXML 4,0, указав заметку SQL: Key-Fields в запросе XPath для определения ключевых столбцов.'
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nesting XML results
- proper nesting in results [SQLXML]
- sql:key-fields
- XSD schemas [SQLXML], key columns
- identifying key columns [SQLXML]
- annotated XSD schemas, key columns
- key columns [SQLXML]
- relationships [SQLXML], key columns
- hierarchical relationships [SQLXML]
- key-fields annotation
ms.assetid: 1a5ad868-8602-45c4-913d-6fbb837eebb0
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d5267f0ad5fdf26b256ec78028e1bbe6f9b07328
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492036"
---
# <a name="identifying-key-columns-using-sqlkey-fields-sqlxml-40"></a>Идентификация ключевых столбцов с использованием sql:key-fields (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  При указании запроса XPath к схеме XSD в большинстве случаев необходимы ключевые сведения, чтобы правильно организовать вложенность в результатах. Указание аннотации **SQL: Key-Fields** позволяет гарантировать создание соответствующей иерархии.  
  
> [!NOTE]  
>  Чтобы обеспечить правильное вложение, рекомендуется указать **SQL: Key-Fields** для элементов, которые сопоставляются с таблицами. Формируемый XML-код является зависимым от упорядочения базового результирующего набора. Если **SQL: Key-Fields** не указано, то созданный XML может быть неправильно сформирован.  
  
 Значение **SQL: Key-Fields** определяет столбцы, однозначно идентифицирующие строки в связи. Если для уникальной идентификации строки требуется более одного столбца, то значения столбцов разделяются пробелами.  
  
 Необходимо использовать заметку **SQL: Key-Fields** , если элемент содержит объект **\<sql:relationship>** , определенный между элементом и дочерним элементом, но не предоставляющий первичный ключ таблицы, указанный в родительском элементе.  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы на основе следующих примеров, необходимо выполнить определенные требования. Дополнительные сведения см. в разделе [требования для запуска примеров SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-producing-the-appropriate-nesting-when-sqlrelationship-does-not-provide-sufficient-information"></a>A. Создание подходящего вложения, если не \<sql:relationship> предоставляет достаточно сведений  
 В этом примере показано, где должны быть указаны **поля SQL: Key-Fields** .  
  
 Рассмотрим следующую схему. Схема задает иерархию между **\<Order>** элементами и, **\<Customer>** в которых **\<Order>** элемент является родительским, а **\<Customer>** элемент — дочерним.  
  
 **\<sql:relationship>** Тег используется для указания связи «родители-потомки». Он идентифицирует столбец CustomerID в таблице Sales.SalesOrderHeader как родительский ключ, который ссылается на дочерний ключ CustomerID таблицы Sales.Customer. Сведения, предоставленные в **\<sql:relationship>** , недостаточно для уникальной идентификации строк в родительской таблице (Sales. SalesOrderHeader). Таким образом, без аннотации **SQL: Key-Fields** создаваемая иерархия является неточной.  
  
 Если **SQL: Key-Fields** задано в **\<Order>** , заметка уникально определяет строки в родительской таблице (Sales. SalesOrderHeader), а ее дочерние элементы отображаются под родительским элементом.  
  
 Схема:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrdCust"  
        parent="Sales.SalesOrderHeader"  
        parent-key="CustomerID"  
        child="Sales.Customer"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"   
               sql:key-fields="SalesOrderID">  
   <xsd:complexType>  
     <xsd:sequence>  
     <xsd:element name="Customer" sql:relation="Sales.Customer"   
                       sql:relationship="OrdCust"  >  
       <xsd:complexType>  
         <xsd:attribute name="CustID" sql:field="CustomerID" />  
         <xsd:attribute name="SoldBy" sql:field="SalesPersonID" />  
       </xsd:complexType>  
     </xsd:element>  
     </xsd:sequence>  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name= "CustomerID" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Создание рабочего образца этой схемы  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл под именем KeyFields1.xml.  
  
2.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните файл под именем KeyFields1T.xml в том же каталоге, в котором был сохранен файл KeyFields1.xml. Запрос XPath в шаблоне возвращает все **\<Order>** элементы с идентификатором CustomerID меньше 3.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="KeyFields1.xml">  
            /Order[@CustomerID < 3]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу для схемы сопоставления (KeyFields1.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields1.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  

     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Ниже приведен частичный результирующий набор.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
    <Order SalesOrderID="43860" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="44501" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="45283" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    .....  
</ROOT>  
```  
  
### <a name="b-specifying-sqlkey-fields-to-produce-proper-nesting-in-the-result"></a>Б. Указание sql:key-fields для получения правильной вложенности в результате  
 В следующей схеме иерархия не указана с помощью **\<sql:relationship>** . Схема по-прежнему требует указания заметки **SQL: Key-Fields** для уникальной идентификации сотрудников в таблице HumanResources. Employee.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="HumanResources.Employee" sql:key-fields="EmployeeID" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="Title">  
          <xsd:complexType>  
            <xsd:simpleContent>  
              <xsd:extension base="xsd:string">  
                 <xsd:attribute name="EmployeeID" type="xsd:integer" />  
              </xsd:extension>  
            </xsd:simpleContent>  
          </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
   </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Создание рабочего образца этой схемы  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл под именем KeyFields2.xml.  
  
2.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните файл под именем KeyFields2T.xml в том же каталоге, в котором был сохранен файл KeyFields2.xml. Запрос XPath в шаблоне возвращает все **\<HumanResources.Employee>** элементы:  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="KeyFields2.xml">  
        /HumanResources.Employee  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу для схемы сопоставления (KeyFields2.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields2.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Результат:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <HumanResources.Employee>  
    <Title EmployeeID="1">Production Technician - WC60</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="2">Marketing Assistant</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="3">Engineering Manager</Title>   
  </HumanResources.Employee>  
  ...  
</ROOT>  
```  
  
  

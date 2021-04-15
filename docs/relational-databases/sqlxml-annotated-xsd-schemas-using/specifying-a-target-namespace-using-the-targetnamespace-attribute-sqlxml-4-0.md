---
title: Указание целевого пространства имен с targetNamespace (SQLXML)
description: Узнайте, как указать целевое пространство имен в схеме XSD с помощью атрибута targetNamespace в SQLXML 4,0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- namespaces [SQLXML], annotated XSD schemas
- targetNamespace attribute
- XSD schemas [SQLXML], target namespaces
- annotated XSD schemas, target namespaces
- xsd:targetNamespace
- attributeFormDefault attribute
- elementFormDefault attribute
- target namespaces [SQLXML]
ms.assetid: f3df9877-6672-4444-8245-2670063c9310
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 011b8e15caae5ea805aedb97223fe62c7640de3c
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490772"
---
# <a name="specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-40"></a>Задание целевого пространства имен с помощью атрибута targetNamespace (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  При написании XSD-схем можно использовать атрибут XSD **targetNamespace** , чтобы указать целевое пространство имен. В этом разделе описано, как работают атрибуты XSD **targetNamespace**, **elementFormDefault** и **attributeFormDefault** , как они влияют на создаваемый экземпляр XML и как задаются запросы XPath с пространствами имен.  
  
 Атрибут **xsd: targetNamespace** можно использовать для размещения элементов и атрибутов из пространства имен по умолчанию в другом пространстве имен. Можно указать, будут ли локально объявленные элементы и атрибуты схемы уточняться пространством имен, использоваться явно путем применения префикса либо использоваться неявно. Вы можете использовать атрибуты **elementFormDefault** и **attributeFormDefault** в элементе, **\<xsd:schema>** чтобы глобально указать квалификацию локальных элементов и атрибутов, или можно использовать атрибут **Form** для указания отдельных элементов и атрибутов по отдельности.  
  
## <a name="examples"></a>Примеры  
 Чтобы создать рабочие образцы на основе следующих примеров, необходимо выполнить определенные требования. Дополнительные сведения см. в разделе [требования для запуска примеров SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-a-target-namespace"></a>A. Указание целевого пространства имен  
 Следующая схема XSD задает целевое пространство имен с помощью атрибута **xsd: targetNamespace** . Схема также задает для значений атрибута **elementFormDefault** и **attributeFormDefault** значение **"неполное"** (для этих атрибутов по умолчанию). Это глобальное объявление и влияет на все локальные элементы ( **\<Order>** в схеме) и атрибуты (**CustomerID**, **ContactName** и **OrderID** в схеме).  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:CO="urn:MyNamespace"   
            targetNamespace="urn:MyNamespace" >  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustOrders"  
          parent="Sales.Customer"  
          parent-key="CustomerID"  
          child="Sales.SalesOrderHeader"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer"   
               sql:relation="Sales.Customer"   
               type="CO:CustomerType" />  
  
  <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Sales.SalesOrderHeader"  
                     sql:relationship="CustOrders"  
                     type="CO:OrderType" />  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
        <xsd:attribute name="SalesPersonID"  type="xsd:string" />  
  </xsd:complexType>  
  <xsd:complexType name="OrderType" >  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name="CustomerID" type="xsd:string" />  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 В схеме:  
  
-   Объявления типов **CustomerType** и **OrderType** являются глобальными и, следовательно, включаются в целевое пространство имен схемы. В результате, если на эти типы имеется ссылка в объявлении **\<Customer>** элемента и его **\<Order>** дочернего элемента, указывается префикс, связанный с целевым пространством имен.  
  
-   **\<Customer>** Элемент также включается в целевое пространство имен схемы, поскольку это глобальный элемент в схеме.  
  
 Выполните следующий XPath-запрос к схеме:  
  
```  
(/CO:Customer[@CustomerID=1)   
```  
  
 Этот XPath-запрос создает следующий экземпляр документа (показаны только несколько заказов):  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <y0:Customer xmlns:y0="urn:MyNamespace"   
      CustomerID="ALFKI" ContactName="Maria Anders">  
        <Order CustomerID="ALFKI" OrderID="10643" />   
        <Order CustomerID="ALFKI" OrderID="10692" />   
        ...  
  </y0:Customer>  
  </ROOT>  
```  
  
 Этот экземпляр документа определяет пространство имен urn: MyNamespace и связывает с ним префикс (y0). Префикс применяется только к **\<Customer>** глобальному элементу. (Элемент является глобальным, так как он объявлен как дочерний **\<xsd:schema>** элемент в схеме.)  
  
 Префикс не применяется к локальным элементам и атрибутам, так как в схеме значения атрибутов **elementFormDefault** и **attributeFormDefault** заданы как **неполные** . Обратите внимание, что **\<Order>** элемент является локальным, так как его объявление отображается в качестве дочернего элемента для **\<complexType>** элемента, определяющего **\<CustomerType>** элемент. Аналогичным образом атрибуты (**CustomerID**, **OrderID** и **ContactName**) являются локальными, а не глобальными.  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Создание рабочего образца этой схемы  
  
1.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл с именем targetNameSpace.xml.  
  
2.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните файл с именем targetNameSpace.xml в тот же каталог, куда уже был сохранен файл targetNamespace.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="targetNamespace.xml"  
                       xmlns:CO="urn:MyNamespace" >  
        /CO:Customer[@CustomerID=1]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Запрос XPath в шаблоне возвращает **\<Customer>** элемент для клиента с идентификатором CustomerID, равным 1. Обратите внимание, что в запросе XPath префикс пространства имен указан для элемента запроса, а не для атрибута. (Как указано в схеме, локальные атрибуты не определены).  
  
     Указанный для схемы сопоставления путь к каталогу (targetNamespace.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\MyDir\targetNamepsace.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Если схема задает атрибуты **elementFormDefault** и **attributeFormDefault** со значением **"квалифицированный"**, то в документе экземпляра будут определены все локальные элементы и атрибуты. Можно изменить предыдущую схему, чтобы включить эти атрибуты в **\<xsd:schema>** элемент, и снова выполнить шаблон. Так как атрибуты в экземпляре теперь также имеют квалификатор, XPath-запрос изменится так, чтобы включать префикс пространства имен.  
  
 Измененный XPath-запрос:  
  
```  
/CO:Customer[@CO:CustomerID=1]  
```  
  
 Возвращаемый XML-документ:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <y0:Customer xmlns:y0="urn:MyNamespace" CustomerID="1" SalesPersonID="280">  
      <Order SalesOrderID="43860" CustomerID="1" />   
      <Order SalesOrderID="44501" CustomerID="1" />   
      <Order SalesOrderID="45283" CustomerID="1" />   
      <Order SalesOrderID="46042" CustomerID="1" />   
   </y0:Customer>  
</ROOT>  
```  
  
  

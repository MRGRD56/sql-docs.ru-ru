---
title: Создание ссылки на встроенную коллекцию схем XML (sys) | Документация Майкрософт
description: Сведения о том, как ссылаться на встроенную коллекцию схем XML sys, предопределенную для каждой создаваемой базы данных.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- sys XML schema collections [SQL Server]
- schema collections [SQL Server], predefined
- predefined XML schema collections [SQL Server]
- XML schema collections [SQL Server], predefined
- built-in XML schema collections [SQL Server]
ms.assetid: 1e118303-5df0-4ee4-bd8d-14ced7544144
author: rothja
ms.author: jroth
ms.openlocfilehash: c061e18e4fb7224afe9a44813c4a8279e487f2d4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487795"
---
# <a name="reference-the-built-in-xml-schema-collection-sys"></a>Создание ссылки на встроенную коллекцию XML-схем (sys)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Любая созданная база данных содержит предопределенную коллекцию XML-схем **sys** в реляционной схеме **sys** . Она содержит эти предопределенные схемы, к которым можно получить доступ из любой другой созданной пользователем коллекции XML-схем. В языке XQuery имеют значение префиксы, используемые для предопределенных схем. К зарезервированным относится только префикс **xml** .  
  
```  
xml = http://www.w3.org/XML/1998/namespace  
xs = http://www.w3.org/2001/XMLSchema  
xsi = http://www.w3.org/2001/XMLSchema-instance  
fn = http://www.w3.org/2004/07/xpath-functions  
sqltypes = https://schemas.microsoft.com/sqlserver/2004/sqltypes  
xdt = http://www.w3.org/2004/07/xpath-datatypes  
(no prefix) = urn:schemas-microsoft-com:xml-sql  
(no prefix) = https://schemas.microsoft.com/sqlserver/2004/SOAP  
```  
  
 Заметим, что пространство имен **sqltypes** содержит компоненты, на которые возможны ссылки из любой определенной пользователем коллекции схем XML. Можно загрузить схему **sqltypes** с [сайта Майкрософт](https://go.microsoft.com/fwlink/?linkid=31850). К встроенным компонентам относятся:  
  
-   типы XSD;  
  
-   XML-атрибуты **lang**, **base** и **space**;  
  
-   компоненты пространства имен **sqltypes** .  
  
 Следующий запрос возвращает встроенные компоненты, на которые возможны ссылки из созданной пользователем коллекции XML-схем.  
  
```  
SELECT C.name, N.name, C.symbol_space_desc from sys.xml_schema_components C join sys.xml_schema_namespaces N  
on ((C.xml_namespace_id = N.xml_namespace_id) AND (C.xml_collection_id = N.xml_collection_id))  
join sys.xml_schema_collections SC  
on SC.xml_collection_id = C.xml_collection_id  
where ((C.xml_collection_id = 1) AND (C.name is not null) AND (C.scoping_xml_component_id is null)   
AND (SC.schema_id = 4))  
GO  
```  
  
 В следующем примере показано, как заданы ссылки на эти компоненты в пользовательской схеме. `CREATE XML SCHEMA COLLECTION` создает коллекцию схем XML, которая ссылается на тип `varchar` , определенный в пространстве имен `sqltypes` . В примере также производится ссылка на атрибут `lang` , определенный в пространстве имен `xml` .  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema   
   xmlns="http://www.w3.org/2001/XMLSchema"   
   targetNamespace="myNS"  
   xmlns:ns="myNS"  
   xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes" >   
   <import namespace="http://www.w3.org/XML/1998/namespace"/>  
   <import namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
   <element name="root">  
      <complexType>  
          <sequence>  
             <element name="a" type="string"/>  
             <element name="b" type="string"/>  
             <!-- varchar type is defined in the sys.sys collection and   
                  can be referenced in any user-defined schema -->  
             <element name="c" type="s:varchar"/>  
          </sequence>  
          <attribute name="att" type="int"/>  
          <!-- xml:lang attribute is defined in the sys.sys collection   
               and can be referenced in any user-defined schema -->  
          <attribute ref="xml:lang"/>  
      </complexType>  
    </element>  
 </schema>'  
GO  
 -- Cleanup  
DROP xml schema collection SC   
GO  
```  
  
 Обратите внимание на следующее.  
  
-   Нельзя модифицировать XML-схемы с данными пространствами имен в пользовательской коллекции XML-схем. Например, следующая коллекция XML-схем завершается неудачно, так как она добавляет компонент в защищенное пространство имен `sqltypes` :  
  
    ```  
    CREATE XML SCHEMA COLLECTION SC AS '  
    <schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace    
        ="https://schemas.microsoft.com/sqlserver/2004/sqltypes" >   
          <element name="root" type="string"/>  
    </schema>'  
    GO  
    ```  
  
-   Нельзя использовать коллекцию XML-схем `sys` для ввода столбцов, переменных или параметров `xml` . Например, следующий код возвращает ошибку:  
  
    ```  
    DECLARE @x xml (sys.sys)  
    ```  
  
-   Сериализация встроенных схем не поддерживается. Например, следующий код возвращает ошибку:  
  
    ```  
    SELECT XML_SCHEMA_NAMESPACE(N'sys',N'sys')  
    GO  
    ```  
  
 Следующий код является еще одним примером, в котором создается коллекция XML-схем, использующая тип данных `varchar`, определенный в пространстве имен `sqltypes`:  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"   
        targetNamespace="myNS" xmlns:ns="myNS"  
        xmlns:s="https://schemas.microsoft.com/sqlserver/2004/sqltypes">  
   <import     
     namespace="https://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
      <simpleType name="myType">  
            <restriction base="s:varchar">  
                  <maxLength value="20"/>  
            </restriction>  
      </simpleType>  
      <element name="root" type="ns:myType"/>  
</schema>'  
go  
```  
  
 Как показано ниже, можно создать переменную `XML`, обладающую типом данных, присвоить ей экземпляр XML и проверить, является ли значение типа элемента <`root`> типом данных `varchar`.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "https://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
data(/ns:root[1]) instance of sqltypes:varchar?')  
GO  
```  
  
 Выражение `instance of sqltypes:varchar?` возвращает значение TRUE, потому что значение элемента <`root`> имеет тип данных, производный от типа данных **varchar** в соответствии со схемой, связанной с переменной `@var`.  
  
## <a name="see-also"></a>См. также:  
 [Коллекции XML-схем (SQL Server)](../../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  

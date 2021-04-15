---
title: Запрет разрешения на коллекцию схем XML | Документация Майкрософт
description: Узнайте, как запретить разрешение на создание новой коллекции схем XML или на использование уже существующей.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- denying permissions [SQL Server], XML server collections
ms.assetid: e2b300b0-e734-4c43-a4da-c78e6e5d4fba
author: rothja
ms.author: jroth
ms.openlocfilehash: 0e428e81f45376bd81dd957812da35e313829c39
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490983"
---
# <a name="deny-permissions-on-an-xml-schema-collection"></a>Запрещает разрешения на коллекцию схем XML.
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Можно запретить разрешение либо на создание новой коллекции XML-схем, либо на использование уже существующей.  
  
## <a name="denying-permission-to-create-an-xml-schema-collection"></a>Запрет разрешений на создание коллекции XML-схем  
 Запретить разрешение на создание коллекции XML-схем можно одним из следующих способов.  
  
-   Запретите разрешение ALTER на реляционную схему.  
  
-   Запретите разрешение CONTROL на реляционную схему, чтобы запретить все разрешения на реляционную схему и на все содержащиеся в ней объекты.  
  
-   Запретите разрешение ALTER ANY SCHEMA на базу данных. В этом случае участник не может создать коллекцию XML-схемы в базе данных. Обратите внимание на то, что при запрете разрешений ALTER или CONTROL на базу данных происходит запрет всех разрешений на все объекты базы данных.  
  
## <a name="denying-permissions-on-an-xml-schema-collection-object"></a>Запрет разрешений на объект коллекции XML-схем  
 Далее приводятся разрешения, которые могут быть запрещены на существующие коллекции XML-схем, а также последствия таких запретов.  
  
-   Запрет разрешения ALTER запрещает участнику изменять содержание коллекции XML-схем.  
  
-   Запрет разрешения CONTROL запрещает участнику выполнять любые действия с коллекцией XML-схем.  
  
-   Запрет разрешения REFERENCES запрещает участнику вводить или ограничивать параметры и столбцы xml ввода при помощи коллекции XML-схем. При этом участнику запрещается ссылаться на эту коллекцию XML-схем из других коллекций XML-схем.  
  
-   Запрет разрешения VIEW DEFINITION запрещает участнику просматривать содержание коллекции XML-схем.  
  
-   Запрет разрешения EXECUTE запрещает участнику вставлять или обновлять значения в столбцах, переменные и параметры, типизированные или ограниченные коллекцией XML-схем. При этом участнику запрещается запрашивать значения в тех же столбцах типа xml и xml-переменных.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показан принцип работы разрешений на XML-схемы. В каждом примере создается соответствующая тестовая база данных, реляционные схемы и имена входа. Этим именам входа предоставляются необходимые разрешения на коллекции XML-схем. После завершения работы каждый пример выполняет необходимые действия по очистке.  
  
### <a name="a-preventing-a-user-from-creating-an-xml-schema-collection"></a>A. Запрет пользователю создавать коллекции схем XML  
 Одним из способов запретить пользователю создавать коллекции XML-схем является запрет разрешения ALTER на реляционную схему. Это показано в следующем примере.  
  
 В данном примере создается пользователь `TestLogin1`и база данных. Кроме того, в дополнение к схеме `dbo` в базе данных создается реляционная схема. Поначалу разрешение `CREATE XML SCHEMA` позволяет пользователю создавать коллекцию схем в любом месте базы данных. В примере запрещается разрешение `ALTER` для пользователя на одну из реляционных схем. При этом пользователю запрещается создавать коллекции XML-схем в этой реляционной схеме.  
  
```  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
-- Create another relational schema in the database.  
CREATE SCHEMA myOtherDBSchema  
GO  
CREATE USER TestLogin1  
GO  
-- For TestLogin1 to create/import XML schema collection, following  
-- permission needed.  
-- Database-level permissions  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
GO  
GRANT ALTER ANY SCHEMA TO TestLogin1  
GO  
-- Now TestLogin1 can import an XML schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
DROP XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection  
GO  
-- Now deny permission from TestLogin1 to alter myOtherDBSchema.  
setuser  
GO  
DENY ALTER ON SCHEMA::myOtherDBSchema TO TestLogin1  
GO  
-- Now TestLogin1 cannot create xml schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
-- Final cleanup  
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
GO  
```  
  
### <a name="b-denying-permissions-on-an-xml-schema-collection"></a>Б. Запрет разрешений на коллекцию схем XML  
 В следующем примере показано, как запретить разрешение для имени входа на существующую коллекцию XML-схем. В этом примере запрещается разрешение REFERENCES для тестового имени входа на существующую коллекцию XML-схем.  
  
 В данном примере создается пользователь `TestLogin1`и база данных. Кроме того, в дополнение к схеме `dbo` в базе данных создается реляционная схема. Поначалу разрешение `CREATE XML SCHEMA` позволяет пользователю создавать коллекцию схем в любом месте базы данных.  
  
 Разрешение `REFERENCES` на коллекцию XML-схем позволяет учетной записи `TestLogin1` использовать схему при создании типизированного столбца `xml` в таблице. Если разрешение `REFERENCES` коллекции XML-схем запрещено, то пользователь `TestLogin1` не может использовать ее.  
  
```  
CREATE LOGIN TestLogin1 WITH password='SQLSvrPwd1'  
GO  
CREATE DATABASE SampleDBForSchemaPermissions  
GO  
USE SampleDBForSchemaPermissions  
GO  
-- Create another relational schema in the database.  
CREATE SCHEMA myOtherDBSchema  
GO  
CREATE USER TestLogin1  
GO  
-- For TestLogin1 to create/import XML schema collection, the following  
-- permission is required.  
-- Database-level permissions  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
GO  
GRANT ALTER ANY SCHEMA TO TestLogin1  
GO  
-- Now TestLogin1 can import an XML schema collection.  
SETUSER 'TestLogin1'  
GO  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
GO  
-- Grant permission to TestLogin1 to create a table and reference the XML schema collection.  
SETUSER  
GO  
GRANT CREATE TABLE TO TestLogin1  
GO  
-- The user also needs REFERENCES permission to use the XML schema collection  
-- to create a typed XML column (REFERENCES permission on the schema   
-- collection is not needed).  
GRANT REFERENCES ON XML SCHEMA COLLECTION::myOtherDBSchema.myTestSchemaCollection   
TO TestLogin1  
GO  
  
--TestLogin1 can use the schema.  
CREATE TABLE T(i int, x xml (myOtherDBSchema.myTestSchemaCollection))  
GO  
-- Drop the table.  
DROP TABLE T  
GO  
-- Now deny REFERENCES permission to TestLogin1 on the schema created previously.  
SETUSER  
GO  
DENY REFERENCES ON XML SCHEMA COLLECTION::myOtherDBSchema.myTestSchemaCollection TO TestLogin1  
  
GO  
-- Now TestLogin1 cannot create xml schema collection  
SETUSER 'TestLogin1'  
GO  
-- Following statement fails. TestLogin1 does not have REFERENCES   
-- permission on the XML schema collection.  
CREATE TABLE T(i int, x xml (myOtherDBSchema.myTestSchemaCollection))  
GO  
  
-- Final cleanup  
SETUSER  
GO  
USE master  
GO  
DROP DATABASE SampleDBForSchemaPermissions  
GO  
DROP LOGIN TestLogin1  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Коллекции XML-схем (SQL Server)](../../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Требования и ограничения для коллекций XML-схем на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)   
 [DENY, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/deny-object-permissions-transact-sql.md)   
 [GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [Данные XML (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
  

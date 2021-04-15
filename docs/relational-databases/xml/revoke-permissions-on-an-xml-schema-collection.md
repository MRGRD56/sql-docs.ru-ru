---
title: Отзыв разрешения на коллекцию схем XML | Документация Майкрософт
description: Узнайте, как отменять разрешения на коллекцию схем XML.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- revoking permissions [SQL Server]
ms.assetid: 4e542b70-2d56-4a65-8a39-96a1ed477ca6
author: rothja
ms.author: jroth
ms.openlocfilehash: a6be58ed505e5816c0c7dc3dfeaf589c3248e21c
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487562"
---
# <a name="revoke-permissions-on-an-xml-schema-collection"></a>Отмена разрешений на коллекцию схем XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Разрешение на создание коллекции XML-схем можно отменить, выполнив одну из следующих операций:  
  
-   Отменить разрешение ALTER для реляционной схемы. Тогда участник не сможет создать коллекцию XML-схем в реляционной схеме. Однако участник сможет создавать коллекции XML-схем в других реляционных схемах той же базы данных.  
  
-   Отменить для участника разрешение ALTER ANY SCHEMA в базе данных. Тогда участник не сможет создать коллекцию XML-схем где-либо в базе данных.  
  
-   Отменить для участника разрешения CREATE XML SCHEMA COLLECTION или ALTER XML SCHEMA COLLECTION в базе данных. Это не позволит участнику осуществлять импортирование коллекции XML-схем в пределах базы данных. Отмена разрешения ALTER или CONTROL в базе данных приводит к тому же результату.  
  
## <a name="revoking-permissions-on-an-existing-xml-schema-collection-object"></a>Отмена разрешений на существующий объект коллекции XML-схем  
 Здесь представлены разрешения, которые можно отменить для коллекции XML-схем, и результаты такой отмены:  
  
-   Отмена разрешения ALTER лишает участника возможности изменять содержимое коллекции XML-схем.  
  
-   Отмена разрешения TAKE OWNERSHIP лишает участника возможности передавать право на владение коллекцией XML-схем.  
  
-   Отмена разрешения REFERENCES лишает участника возможности использовать коллекцию XML-схем для ввода или ограничения столбцов XML-типа в таблицах, представлениях и параметрах. А также отменяет разрешение ссылаться на эту коллекцию схем из других коллекций XML-схем.  
  
-   Отмена разрешения VIEW DEFINITION лишает участника возможности просмотра содержимого коллекции XML-схем.  
  
-   Отмена разрешения EXECUTE лишает участника возможности вставлять или обновлять значения в столбцах, переменных и параметрах, которые набраны или ограничены XML-коллекцией. Кроме того, отмена лишает права делать запрос столбцов, переменных или параметров типа **xml** .  
  
## <a name="examples"></a>Примеры  
 Представленные в следующих примерах сценарии отображают организацию разрешений на XML-схемы. В каждом примере создается соответствующая тестовая база данных, реляционные схемы и имена входа. Этим именам входа предоставляются необходимые разрешения на коллекции XML-схем. После завершения работы каждый пример выполняет необходимые действия по очистке.  
  
### <a name="a-revoking-permissions-to-create-an-xml-schema-collection"></a>A. Отмена разрешений на создание коллекции XML-схем  
 Этот пример создает имя входа и образец базы данных. Он также добавляет реляционную схему в базу данных. Изначально имени входа предоставляется разрешение ALTER на обе реляционные схемы и другие необходимые разрешения на создание коллекций XML-схем. Этот пример отменяет разрешение ALTER на одну из реляционных схем в базе данных. Это лишает имя входа возможности создания коллекции XML-схем.  
  
```  
setuser  
go  
create login TestLogin1 with password='SQLSvrPwd1'  
go  
create database SampleDBForSchemaPermissions  
go  
use SampleDBForSchemaPermissions  
go  
-- Create another relational schema in the db (in addition to dbo schema)  
CREATE SCHEMA myOtherDBSchema  
go  
CREATE USER TestLogin1  
go  
-- For TestLogin1 to create/import XML schema collection, following  
-- permission needed  
-- CREATE XML SCHEMA is a database level permission  
GRANT CREATE XML SCHEMA COLLECTION TO TestLogin1  
go  
GRANT ALTER ON SCHEMA::myOtherDBSchema TO TestLogin1  
go  
GRANT ALTER ON SCHEMA::dbo TO TestLogin1  
go  
-- Now TestLogin1 can import an XML schema collection in both relational schemas.  
setuser 'TestLogin1'  
go  
CREATE XML SCHEMA COLLECTION dbo.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
-- TestLogin1 can create XML schema collection in myOtherDBSchema relational schema  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
-- Let us drop XML schema collections from both relational schemas  
DROP XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection  
go  
DROP XML SCHEMA COLLECTION dbo.myTestSchemaCollection  
go  
-- now REVOKE permission from TestLogin1 to alter myOtherDBSchema  
setuser  
go  
REVOKE ALTER ON SCHEMA::myOtherDBSchema FROM TestLogin1  
go  
-- now TestLogin1 cannot create xml schema collection in myOtherDBSchema  
setuser 'TestLogin1'  
go  
CREATE XML SCHEMA COLLECTION myOtherDBSchema.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
  
-- TestLogin1 can still create XML schema collections in dbo  
-- It cannot create XML schema collections anywhere in the database  
-- if we REVOKE CREATE XML SCHEMA COLLECTION permission  
SETUSER  
go  
REVOKE CREATE XML SCHEMA COLLECTION FROM TestLogin1  
go  
  
setuser 'TestLogin1'  
go  
-- the following now should fail  
CREATE XML SCHEMA COLLECTION dbo.myTestSchemaCollection AS '<?xml version="1.0" encoding="UTF-8" ?>  
<xsd:schema targetNamespace="https://schemas.adventure-works.com/Additional/ContactInfo"   
            xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
elementFormDefault="qualified">  
<xsd:element name="telephone" type="xsd:string" />  
</xsd:schema>'  
go  
  
-- Final cleanup  
SETUSER  
go  
USE master  
go  
DROP DATABASE SampleDBForSchemaPermissions  
go  
DROP LOGIN TestLogin1  
Go  
```  
  
## <a name="see-also"></a>См. также:  
 [Данные XML (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)   
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Коллекции XML-схем (SQL Server)](../../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Требования и ограничения для коллекций схем XML на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  

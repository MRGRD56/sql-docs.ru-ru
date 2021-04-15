---
title: Директива TYPE в запросах FOR XML | Документация Майкрософт
description: Узнайте, как использовать директиву TYPE с предложением FOR XML, чтобы запросить возврат результата запроса в виде XML-данных.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, TYPE directive
- TYPE directive
ms.assetid: a3df6c30-1f25-45dc-b5a9-bd0e41921293
author: rothja
ms.author: jroth
ms.openlocfilehash: bc1c3897b4c82e32bd70bbd3f28948357ecc19a9
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487422"
---
# <a name="type-directive-in-for-xml-queries"></a>Директива TYPE в запросах FOR XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает тип данных [XML (Transact-SQL)](../../t-sql/xml/xml-transact-sql.md), что позволяет с помощью директивы TYPE запросить получение результата запроса FOR XML в виде типа данных **xml**. Это позволяет обрабатывать результат запроса FOR XML на сервере. Например, к нему можно применить инструкции на языке XQuery, присвоить его результат переменной типа **xml** или написать [вложенные запросы FOR XML](../../relational-databases/xml/use-nested-for-xml-queries.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает клиенту тип данных XML экземпляра, представляющий собой результат различных серверных конструкций, таких как запросы FOR XML, использующие директиву TYPE или тип данных **xml** для получения значений XML-данных экземпляра из столбцов таблицы и выходных параметров SQL. В коде клиентского приложения поставщик ADO.NET запрашивает эти XML-данные для отправки с сервера в двоичной кодировке. Однако при использовании предложения FOR XML без директивы TYPE XML-данные возвращаются как данные строкового типа. В любом случае поставщик клиента всегда будет иметь возможность обрабатывать XML-данные в любом из форматов. Обратите внимание на то, что предложение FOR XML верхнего уровня без директивы TYPE не может быть использовано с курсорами.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах показано использование директивы TYPE в запросах FOR XML.  
  
### <a name="retrieving-for-xml-query-results-as-xml-type"></a>Получение результатов запроса FOR XML в виде XML-данных  
 Следующий запрос используется для получения контактной информации о клиенте из таблицы `Contacts` . Поскольку в запросе `TYPE` указана директива `FOR XML`, результат возвращается в виде типа **xml** .  
  
```  
USE AdventureWorks2012;  
Go  
SELECT BusinessEntityID, FirstName, LastName  
FROM Person.Person  
ORDER BY BusinessEntityID  
FOR XML AUTO, TYPE;  
```  
  
 Частичный результат:  
  
 `<Person.Person BusinessEntityID="1" FirstName="Ken" LastName="Sánchez"/>`  
  
 `<Person.Person BusinessEntityID="2" FirstName="Terri" LastName="Duffy"/>`  
  
 `...`  
  
### <a name="assigning-for-xml-query-results-to-an-xml-type-variable"></a>Назначение результатов запроса FOR XML переменной типа xml  
 В следующем примере результат инструкции FOR XML присваивается переменной **типа** xml `@x`. Запрос извлекает информацию о контактах, например `BusinessEntityID`, `FirstName`, `LastName`, а также дополнительные телефонные номера из столбца `AdditionalContactInfo`**xml**`TYPE`. Поскольку в предложении `FOR XML` указана директива `TYPE` , результат возвращается в виде типа **xml** и присваивается переменной.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @x xml;  
SET @x = (  
   SELECT BusinessEntityID,   
          FirstName,   
          LastName,   
          AdditionalContactInfo.query('  
declare namespace aci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
              //act:telephoneNumber/act:number') as MorePhoneNumbers  
   FROM Person.Person  
   FOR XML AUTO, TYPE);  
SELECT @x;  
GO  
```  
  
### <a name="querying-results-of-a-for-xml-query"></a>Запрос к результатам запроса FOR XML  
 Запросы FOR XML возвращают XML-данные. Таким образом, можно применить методы типа **xml** , например **query()** и **value()** , к XML-данным, возвращенным запросами FOR XML.  
  
 В следующем запросе метод `query()` для типа данных **xml** используется с целью запроса к результатам запроса `FOR XML` . Дополнительные сведения см. в разделе [Метод query&#40;&#41;&#40;тип данных xml&#41;](../../t-sql/xml/query-method-xml-data-type.md).  
  
```  
USE AdventureWorks2012;  
GO  
SELECT (SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
DECLARE namespace aci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
DECLARE namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
 //act:telephoneNumber/act:number  
') AS PhoneNumbers  
FROM Person.Person  
FOR XML AUTO, TYPE).query('/Person.Person[1]');  
```  
  
 В следующем внутреннем запросе `SELECT ... FOR XML` возвращается результат типа **xml** , к которому внешняя инструкция `SELECT` применяет метод `query()` к типу **xml** . Обратите внимание на указанную директиву `TYPE` .  
  
 Результат:  
  
 `<Person.Person BusinessEntityID="1" FirstName="Ken" LastName="Sánchez">`  
  
 `<PhoneNumbers>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">111-111-1111</act:number>`  
  
 `<act:number xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">112-111-1111</act:number>`  
  
 `</PhoneNumbers>`  
  
 `</Person.Person>`  
  
 В следующем запросе метод `value()` для типа данных **xml** используется с целью извлечения значения из результирующего XML-документа, возвращенного запросом `SELECT...FOR XML` . Дополнительные сведения см. в разделе [Метод value (тип данных xml)](../../t-sql/xml/value-method-xml-data-type.md).  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @FirstPhoneFromAdditionalContactInfo varchar(40);  
SELECT @FirstPhoneFromAdditionalContactInfo =   
 ( SELECT BusinessEntityID, FirstName, LastName, AdditionalContactInfo.query('  
declare namespace aci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
   //act:telephoneNumber/act:number  
   ') AS PhoneNumbers  
   FROM Person.Person Contact  
   FOR XML AUTO, TYPE).value('  
declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
  /Contact[@BusinessEntityID="1"][1]/PhoneNumbers[1]/act:number[1]', 'varchar(40)'  
 )  
SELECT @FirstPhoneFromAdditionalContactInfo;  
```  
  
 Выражение пути XQuery в методе `value()` извлекает из контактных сведений первый номер телефона заказчика с идентификатором `BusinessEntityID` , равным `1`.  
  
> [!NOTE]  
>  Если директива TYPE не указана, результат запроса FOR XML возвращается в виде данных типа **nvarchar(max)** .  
  
### <a name="using-for-xml-query-results-in-insert-update-and-delete-transact-sql-dml"></a>Использование результатов запроса FOR XML в инструкциях INSERT, UPDATE и DELETE (Transact-SQL DML)  
 В следующем примере показано использование запросов FOR XML в инструкциях языка DML. В данном примере запрос `FOR XML` возвращает экземпляр типа **xml** . Инструкция `INSERT` вставляет этот экземпляр XML в таблицу.  
  
```  
CREATE TABLE T1(intCol int, XmlCol xml);  
GO  
INSERT INTO T1   
VALUES(1, '<Root><ProductDescription ProductModelID="1" /></Root>');  
GO  
  
CREATE TABLE T2(XmlCol xml)  
GO  
INSERT INTO T2(XmlCol)   
SELECT (SELECT XmlCol.query('/Root')   
        FROM T1   
        FOR XML AUTO,TYPE);   
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [FOR XML (SQL Server)](../../relational-databases/xml/for-xml-sql-server.md)  
  
  

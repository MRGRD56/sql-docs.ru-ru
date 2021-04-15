---
title: Клиентское и серверное форматирование XML (SQLXML)
description: Ознакомьтесь с общими различиями между форматированием XML на стороне клиента и сервером в SQLXML 4,0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- NESTED mode
- FOR XML clause, formatting
- client-side XML formatting
- server-side XPath
- server-side XML formatting
- AUTO mode
- client-side XPath
ms.assetid: f807ab7a-c5f8-4e61-9b00-23aebfabc47e
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1f85b8922a1f04972a0fc9ce05a0d7a173fa6c45
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490412"
---
# <a name="client-side-vs-server-side-xml-formatting-sqlxml-40"></a>Форматирование XML-кода на клиенте и на сервере (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  В этом разделе описываются основные различия между форматированием XML-кода в SQLXML на стороне клиента и на стороне сервера.  
  
## <a name="multiple-rowset-queries-not-supported-in-client-side-formatting"></a>Запросы к нескольким наборам строк не поддерживаются при форматировании на стороне клиента  
 Запросы, создающие несколько наборов строк, не поддерживаются при использовании форматирования XML-кода на стороне клиента. Например, предположим, что имеется виртуальный каталог, в котором определено форматирование на стороне клиента. Рассмотрим этот пример шаблона, который содержит две инструкции SELECT в **\<sql:query>** блоке:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query>  
     SELECT FirstName FROM Person.Contact FOR XML Nested;   
     SELECT LastName FROM Person.Contact FOR XML Nested    
  </sql:query>  
</ROOT>  
```  
  
 При выполнении данного шаблона в коде приложения возвращается ошибка, так как форматирование XML-кода на стороне клиента не поддерживает форматирование нескольких наборов строк. При указании запросов в двух отдельных **\<sql:query>** блоках будут получены нужные результаты.  
  
## <a name="timestamp-maps-differently-in-client--vs-server-side-formatting"></a>Различное сопоставление типа timestamp при форматировании на стороне клиента и на стороне сервера  
 При форматировании XML на стороне сервера столбец базы данных типа **timestamp** сопоставляется с типом XDR типа I8 (если в запросе указан параметр XMLDATA).  
  
 В формате XML на стороне клиента столбец базы данных типа **timestamp** сопоставляется либо с **URI** , либо с типом XDR **. base64** (в зависимости от того, указан ли в запросе параметр binary base64). Тип XDR в **bin. base64** полезен при использовании функций диаграмма обновления и Bulkload, так как этот тип преобразуется в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] тип **timestamp** . Таким образом, операции вставки, обновления или удаления выполняются успешно.  
  
## <a name="deep-variants-are-used-in-server-side-formatting"></a>При форматировании на стороне сервера используются глубокие типы VARIANT  
 При форматировании XML-кода на стороне сервера используются глубокие типы VARIANT. При форматировании XML-кода на стороне клиента подтипы VARIANT преобразуются в строку в Юникоде, а вложенные типы VARIANT не используются.  
  
## <a name="nested-mode-vs-auto-mode"></a>Режим NESTED и режим AUTO  
 Режим NESTED предложения FOR XML на стороне клиента аналогичен режиму AUTO предложения FOR XML на стороне сервера, за исключением следующих моментов.  
  
### <a name="when-you-query-views-using-auto-mode-on-the-server-side-the-view-name-is-returned-as-the-element-name-in-the-resulting-xml"></a>При представлении запроса с помощью режима AUTO на стороне сервера имя представления возвращается в виде имени элемента в результирующем XML-коде  
 Например, предположим, что в таблице Person. Contact в Адвентуреворксдатабасе создается следующее представление:  
  
```  
CREATE VIEW ContactView AS (SELECT ContactID as CID,  
                               FirstName  as FName,  
                               LastName  as LName  
                        FROM Person.Contact)  
```  
  
 Следующий шаблон указывает запрос к представлению ContactView, а также форматирование XML-кода на стороне сервера:  
  
```  
 <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="0">  
    SELECT *  
    FROM   ContactView  
    FOR XML AUTO  
  </sql:query>  
</ROOT>  
```  
  
 После применения этого шаблона возвращается следующий XML-код (Отображаются только частичные результаты.) Обратите внимание, что имена элементов — это имена представлений, в которых выполняется запрос.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <ContactView CID="1" FName="Gustavo" LName="Achong" />   
  <ContactView CID="2" FName="Catherine" LName="Abel" />   
...  
</ROOT>  
```  
  
 При форматировании XML-кода на стороне сервера в режиме NESTED имена базовой таблицы возвращаются в виде имен элементов в результирующем XML-коде. Например, следующий измененный шаблон выполняет ту же инструкцию SELECT, но форматирование XML выполняется на стороне клиента (т. е. **клиентский XML-файл** имеет значение true в шаблоне):  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="1">  
    SELECT *  
    FROM   ContactView  
    FOR XML NESTED  
  </sql:query>  
</ROOT>  
```  
  
 В результате применения этого шаблона создается следующий XML-код. Обратите внимание, что именем элемента в этом случае является имя базовой таблицы.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact CID="1" FName="Gustavo" LName="Achong" />   
  <Person.Contact CID="2" FName="Catherine" LName="Abel" />   
...  
</ROOT>  
```  
  
### <a name="when-you-use-auto-mode-of-the-server-side-for-xml-the-table-aliases-specified-in-the-query-are-returned-as-element-names-in-the-resulting-xml"></a>При использовании режима AUTO предложения FOR XML на стороне сервера, псевдонимы таблиц, указанные в запросе, возвращаются в виде имен элементов в результирующем XML-коде  
 Например, рассмотрим следующий шаблон.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="0">  
    SELECT FirstName as fname,  
           LastName as lname  
    FROM   Person.Contact C  
    FOR XML AUTO  
  </sql:query>  
</ROOT>  
```  
  
 В результате выполнения этого шаблона создается следующий XML-код.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <C fname="Gustavo" lname="Achong" />   
  <C fname="Catherine" lname="Abel" />   
...  
</ROOT>   
```  
  
 При использовании режима NESTED предложения FOR XML на стороне клиента, имена таблиц возвращаются в виде имен элементов в результирующем XML-коде. (Псевдонимы таблиц, указанные в запросе, не используются.) Например, рассмотрим следующий шаблон:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <sql:query client-side-xml="1">  
    SELECT FirstName as fname,  
           LastName as lname  
    FROM   Person.Contact C  
    FOR XML NESTED  
  </sql:query>  
</ROOT>  
```  
  
 В результате выполнения этого шаблона создается следующий XML-код.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Person.Contact fname="Gustavo" lname="Achong" />   
  <Person.Contact fname="Catherine" lname="Abel" />   
...  
</ROOT>  
```  
  
### <a name="if-you-have-a-query-that-returns-columns-as-dbobject-queries-you-cannot-use-aliases-for-these-columns"></a>При наличии запроса, возвращающего столбцы в виде запросов объектов базы данных, использовать псевдонимы для этих столбцов нельзя  
 Например, рассмотрим следующий шаблон, выполняющий запрос, который возвращает идентификатор и фотографию сотрудника.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
<sql:query client-side-xml="1">  
   SELECT ProductPhotoID, LargePhoto as P  
   FROM   Production.ProductPhoto  
   WHERE  ProductPhotoID=5  
   FOR XML NESTED, elements  
</sql:query>  
</ROOT>  
```  
  
 Выполнение этого шаблона возвращает столбец Photo в виде запроса объекта базы данных. В запросе объекта базы данных `@P` ссылается на имя несуществующего столбца.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Production.ProductPhoto>  
    <ProductPhotoID>5</ProductPhotoID>  
    <LargePhoto>dbobject/Production.ProductPhoto[@ProductPhotoID='5']/@P</LargePhoto>  
  </Production.ProductPhoto>  
</ROOT>  
```  
  
 Если форматирование XML выполняется на сервере (**Клиент-Side-XML = "0"**), можно использовать псевдоним для столбцов, возвращающих DBOBJECT запросы, в которых возвращаются фактические имена таблиц и столбцов (даже если указаны псевдонимы). Например, следующий шаблон выполняет запрос, и форматирование XML выполняется на сервере (параметр **Client-Side-XML** не указан, а параметр **выполнить на клиенте** не выбран для виртуального корневого каталога). Запрос также указывает режим AUTO (а не режим NESTED на стороне клиента).  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
<sql:query   
   SELECT ProductPhotoID, LargePhoto as P  
   FROM   Production.ProductPhoto  
   WHERE  ProductPhotoID=5  
   FOR XML AUTO, elements  
</sql:query>  
</ROOT>  
```  
  
 При применении этого шаблона возвращается следующий XML-документ (обратите внимание, что псевдонимы в запросе объектов базы данных для столбца LargePhoto не используются):  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Production.ProductPhoto>  
    <ProductPhotoID>5</ProductPhotoID>  
    <LargePhoto>dbobject/Production.ProductPhoto[@ProductPhotoID='5']/@LargePhoto</LargePhoto>  
  </Production.ProductPhoto>  
</ROOT>  
```  
  
### <a name="client-side-vs-server-side-xpath"></a>Запросы XPath на стороне клиента и на стороне сервера  
 Запросы XPath на стороне клиента и на стороне сервера работают аналогично, за исключением следующих моментов.  
  
-   Преобразования данных, применяемые при использовании запросов XPath на стороне клиента, отличаются от применяемых при использовании запросов XPath на стороне сервера. Запрос XPath на стороне клиента использует функцию CAST вместо CONVERT mode 126.  
  
-   При указании в шаблоне типа **Client-Side-XML = "0"** (false) выполняется запрос форматирования XML на стороне сервера. Таким образом, нельзя указать предложение FOR XML NESTED, так как сервер не распознает параметр NESTED. Это приводит к ошибке. Необходимо использовать режимы AUTO, RAW или EXPLICIT, которые сервер распознает.  
  
-   При указании **Client-Side-XML = "1"** (true) в шаблоне выполняется запрос форматирования XML на стороне клиента. В этом случае можно указать предложение FOR XML NESTED. Если для XML AUTO задано значение, то форматирование XML происходит на стороне сервера, хотя в шаблоне указан параметр **Client-Side-XML = "1"** .  
  
## <a name="see-also"></a>См. также  
 [Рекомендации по безопасности XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/for-xml-security-considerations-sqlxml-4-0.md)   
 [Форматирование XML на стороне клиента &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/formatting/client-side-xml-formatting-sqlxml-4-0.md)   
 [Форматирование XML на стороне сервера &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/formatting/server-side-xml-formatting-sqlxml-4-0.md)  
  
  

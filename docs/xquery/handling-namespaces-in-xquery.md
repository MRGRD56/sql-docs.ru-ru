---
title: Обработка пространств имен в XQuery | Документация Майкрософт
description: Просмотрите примеры обработки пространств имен в языке XQuery, которые содержат инструкции по объявлению новых пространств имен и.
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- declaring namespaces
- namespaces [XQuery]
- XQuery, namespaces
ms.assetid: 542b63da-4d3d-4ad5-acea-f577730688f1
author: rothja
ms.author: jroth
ms.openlocfilehash: acb1595ca2e7032d0363df3a1dd81740b7bcbf2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341884"
---
# <a name="handling-namespaces-in-xquery"></a>Поддержка пространств имен в XQuery
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  В этом разделе приведены примеры обработки пространств имен в запросах.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-declaring-a-namespace"></a>A. Объявление пространства имен  
 В результате следующего запроса будут выведены этапы производства конкретной модели продукта.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /AWMI:root/AWMI:Location[1]/AWMI:step  
    ') as x  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Частичный результат:  
  
```  
<AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>. </AWMI:step>  
...  
```  
  
 Обратите внимание, что ключевое слово **Namespace** используется для определения нового префикса пространства имен "AWMI:". Этот префикс затем должен применяться ко всем элементам запроса, которые попадают в область действия этого пространства имен.  
  
### <a name="b-declaring-a-default-namespace"></a>Б. Объявление пространства имен по умолчанию  
 В предыдущем запросе был определен префикс нового пространства имен. Этот префикс затем нужно было использовать в запросе для выборки нужных XML-структур. Однако можно объявить пространство имен по умолчанию, как это показано в следующем измененном запросе:  
  
```  
SELECT Instructions.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /root/Location[1]/step  
    ') as x  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Результат:  
  
```  
<step xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <material>aluminum sheet MS-2341</material> into the <tool>T-85A framing tool</tool>. </step>  
...  
```  
  
 Обратите внимание, что в этом примере определяемое пространство имен `"https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"` замещает пустое пространство имен по умолчанию. Поэтому больше не нужно указывать префикс нового пространства имен в выражении пути, использующемся в запросе. В именах элементов в результате также будет отсутствовать префикс пространства имен. Пространство имен по умолчанию применяется ко всем элементам, но не к их атрибутам.  
  
### <a name="c-using-namespaces-in-xml-construction"></a>В. Использование пространств имен при создании XML  
 При определении новые пространства имен получают область действия, распространяющуюся не только на запрос, но и на создание XML. Например, чтобы создать XML, можно определить новое пространство имен с помощью объявления `declare namespace ...`, а затем использовать это пространство имен со всеми элементами и атрибутами, которые должны будут появиться в результате запроса.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace myNS="uri:SomeNamespace";<myNS:Result>  
          { /ProductDescription/Summary }  
       </myNS:Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Результат:  
  
```  
  
      <myNS:Result xmlns:myNS="uri:SomeNamespace">  
  <Summary xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
     Our top-of-the-line competition mountain bike. Performance-enhancing   
     options include the innovative HL Frame, super-smooth front   
     suspension, and traction for all terrain.</p1:p>  
  </Summary>  
</myNS:Result>  
```  
  
 Можно также определять пространство имен явно, каждый раз, когда оно используется как часть конструкции XML, как это показано в следующем запросе:  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <myNS:Result xmlns:myNS="uri:SomeNamespace">  
          { /ProductDescription/Summary }  
       </myNS:Result>  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
### <a name="d-construction-using-default-namespaces"></a>Г. Создание с помощью пространств имен по умолчанию  
 Можно также определить пространства имен по умолчанию для использования в создании XML. Например, в следующем запросе показано, как можно указать пространство имен по умолчанию (URI: Соменамеспаце) \\ , используемое в качестве значения по умолчанию для создаваемых локально именованных элементов, таких как `<Result>` элемент.  
  
```  
SELECT CatalogDescription.query('  
      declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      declare default element namespace "uri:SomeNamespace";<Result>  
          { /PD:ProductDescription/PD:Summary }  
       </Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Результат:  
  
```  
  
      <Result xmlns="uri:SomeNamespace">  
  <PD:Summary xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         Our top-of-the-line competition mountain bike. Performance-  
         enhancing options include the innovative HL Frame, super-smooth   
         front suspension, and traction for all terrain.</p1:p>  
  </PD:Summary>  
</Result>  
```  
  
 Обратите внимание, что после замещения пустого пространства имен элементов по умолчанию все локальные элементы в создаваемом XML привязываются к новому пространству имен. Поэтому, если при создании XML нужна гибкость за счет использования пустого пространства имен, не замещайте пространство имен элементов по умолчанию.  
  
## <a name="see-also"></a>См. также:  
 [Добавление пространств имен в запросы с WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Данные XML (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)   
 [Справочник по языку XQuery (SQL Server)](../xquery/xquery-language-reference-sql-server.md)  
  
  

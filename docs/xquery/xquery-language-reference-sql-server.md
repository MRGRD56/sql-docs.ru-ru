---
title: Справочник по языку XQuery (SQL Server) | Документация Майкрософт
description: Узнайте о языке XQuery для SQL Server и просмотрите полную языковую ссылку.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
helpviewer_keywords:
- XQuery
- XQuery, about XQuery
- xml data type [SQL Server], XQuery
- XML [SQL Server], XQuery
- queries [XML in SQL Server], XQuery
ms.assetid: 8a69344f-2990-4357-8160-cb26aac95b91
author: rothja
ms.author: jroth
ms.openlocfilehash: 6fec2edcca6a5a73c8fcdb9bb0629b7b5ed2ff24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340464"
---
# <a name="xquery-language-reference-sql-server"></a>Справочник по языку XQuery (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[tsql](../includes/tsql-md.md)] поддерживает подмножество языка XQuery, используемого для запроса типа данных **XML** . Эта реализация XQuery совпадает с рабочим эскизом XQuery на июль 2004 г. Язык разрабатывается консорциумом World Wide Web Consortium (W3C) с участием всех основных поставщиков баз данных, а также корпорации Майкрософт. Так как спецификации W3C могут быть подвержены изменениям в будущем, перед тем как стать рекомендациями W3C, эта реализация может отличаться от конечной рекомендации. Данный подраздел охватывает семантику и синтаксис поднабора XQuery, который поддерживается в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Дополнительные сведения см. в [спецификации языка W3C XQuery 1,0](https://go.microsoft.com/fwlink/?LinkId=48846).  
  
 XQuery является языком, который может выполнять запросы к структурированным или полуструктурированным XML-данным. Благодаря поддержке типа данных **XML** , представленной в [!INCLUDE[ssDE](../includes/ssde-md.md)] , документы могут храниться в базе данных, а затем запрашиваться с помощью XQuery.  
  
 Язык XQuery основан на существующем языке запросов XPath с дополнительной улучшенной поддержкой итераций, результатов сортировки и возможности конструировать необходимые структуры XML. Язык XQuery работает с моделью данных XQuery. Это абстракция XML-документов и результатов XQuery, которые могут быть типизированными и нетипизированными. Сведения о типе основываются на типах, предоставляемых языком XML-схем W3C. Если нет доступных сведений по типам, XQuery считает данные нетипизированными. Это похоже на то, как XPath версии 1.0 обрабатывает XML.  
  
 Для запроса экземпляра XML, хранящегося в переменной или столбце типа **XML** , используются [методы типа данных XML](../t-sql/xml/xml-data-type-methods.md). Например, можно объявить переменную типа **XML** и запросить ее с помощью метода **query ()** типа данных **XML** .  
  
```sql
DECLARE @x xml  
SET @x = '<ROOT><a>111</a></ROOT>'  
SELECT @x.query('/ROOT/a')  
```  
  
 В следующем примере запрос задается для столбца Instructions типа **XML** в таблице ProductModel в базе данных AdventureWorks.  
  
```sql
SELECT Instructions.query('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') as Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 В языке XQuery есть объявление пространства имен, `declare namespace``AWMI=...` и выражение запроса `/AWMI:root/AWMI:Location[@LocationID=10]` .  
  
 Обратите внимание, что XQuery задается для столбца Instructions типа **XML** . [Метод query ()](../t-sql/xml/query-method-xml-data-type.md) типа данных XML используется для указания запроса XQuery.  
  
 В следующей таблице перечисляются соответствующие темы, которые могут помочь в понимании реализации XQuery в [!INCLUDE[ssDE](../includes/ssde-md.md)].  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Данные XML (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)|Описание поддержки типа данных **XML** в [!INCLUDE[ssDE](../includes/ssde-md.md)] методах и методов, которые можно использовать для этого типа данных. Тип данных **XML** формирует модель входных данных XQuery, на которой выполняются выражения XQuery.|  
|[Коллекции XML-схем (SQL Server)](../relational-databases/xml/xml-schema-collections-sql-server.md)|Описывается, как хранящиеся в базе данных экземпляры XML могут быть типизированы. Это означает, что коллекцию схем XML можно связать со столбцом типа **XML** . Все экземпляры, хранящиеся в столбце, проверяются и типизируются схемой в коллекции и предоставляют типизированные сведения для XQuery.|  
|||  
  
> [!NOTE]  
>  Организация этого раздела основана на спецификации рабочего эскиза XQuery корпорации World Wide Web Consortium (W3C). Некоторые диаграммы, приведенные в этом разделе, взяты из спецификации. В этом разделе сравнивается реализация Microsoft XQuery со спецификацией W3C, описывается, чем Microsoft XQuery отличается от W3C, и указывается, какие возможности W3C не поддерживаются. Спецификация W3C доступна по адресу [http://www.w3.org/TR/2004/WD-xquery-20040723](https://go.microsoft.com/fwlink/?LinkId=48846) .  
  
## <a name="in-this-section"></a>в этом разделе  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Основы языка XQuery](../xquery/xquery-basics.md)|Приводится общий обзор основных понятий XQuery, а также вычисления выражений (статический и динамический контекст), атомизации, действительного логического значения, системы типов XQuery, последовательного сравнения типов и обработки ошибок.|  
|[Выражения языка XQuery](../xquery/xquery-expressions.md)|Описываются основные выражения XQuery, выражения пути, выражения последовательности, логические выражения и выражения арифметического сравнения, конструкция XQuery, выражения FLWOR, условные и количественные выражения, а также различные выражения типов последовательностей.|  
|[Модули и журналы &#40;XQuery&#41;](../xquery/modules-and-prologs-xquery.md)|Введение в XQuery.|  
|[Функции XQuery для типа данных xml](../xquery/xquery-functions-against-the-xml-data-type.md)|Описывается список поддерживаемых функций XQuery.|  
|[Сравнение операторов XQuery с XML-данными](../xquery/xquery-operators-against-the-xml-data-type.md)|Описываются поддерживаемые операторы XQuery.|  
|[Дополнительные примеры запросов на языке XQuery к XML-данным](../xquery/additional-sample-xqueries-against-the-xml-data-type.md)|Приводятся дополнительные образцы запросов XQuery.|  
  
## <a name="see-also"></a>См. также:  
 [Данные XML (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)   
 [Коллекции XML-схем (SQL Server)](../relational-databases/xml/xml-schema-collections-sql-server.md)   
 [Примеры массового импорта и экспорта XML-документов (SQL Server)](../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)  
  
  

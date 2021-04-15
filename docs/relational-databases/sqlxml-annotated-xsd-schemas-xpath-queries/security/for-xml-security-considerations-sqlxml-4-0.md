---
title: Рекомендации по безопасности XML (SQLXML)
description: Дополнительные сведения о важных вопросах безопасности при указании для режима AUTO XML в SQLXML 4,0.
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- NESTED mode
- client-side XML formatting
- FOR XML clause, security
- server-side XML formatting
- AUTO mode
- security [SQLXML], FOR XML
ms.assetid: facba279-df93-475b-ad43-0043dc5bae03
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 129d634f3b717a219c7cd8af8028d1264ae1b9f4
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491640"
---
# <a name="for-xml-security-considerations-sqlxml-40"></a>Вопросы безопасности FOR XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Режим FOR XML AUTO создает иерархию XML, в которой имена элементов совпадают с именами таблиц, а имена атрибутов — с именами столбцов. Он предоставляет сведения о таблицах и столбцах базы данных. При использовании режима AUTO (форматирование на стороне сервера) сведения о базе данных можно скрыть, указав в запросе псевдонимы таблиц и столбцов. Эти псевдонимы возвращаются в результирующем XML-документе в виде имен элементов и атрибутов.  
  
 Например, следующий запрос указывает режим AUTO. Таким образом, на сервере выполняется форматирование XML-кода.  
  
```  
SELECT C.FirstName as F,C.LastName as L   
FROM Person.Contact C   
FOR XML AUTO  
```  
  
 В результирующем XML-документе псевдонимы используются для имен элементов и атрибутов:  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <C F="Nancy" L="Fuller" />   
  <CE F="Andrew" L="Peacock" />   
  <C F="Janet" L="Leverling" />   
  ...  
</root>  
```  
  
 При использовании режима NESTED (форматирование на стороне клиента) псевдонимы возвращаются только для атрибутов в результирующем XML-документе. Имена базовых таблиц всегда возвращаются в виде имен элементов. Например, следующий запрос указывает режим NESTED.  
  
```  
SELECT C.FirstName as F,C.LastName as L   
FROM Person.Contact C   
FOR XML AUTO  
```  
  
 В результирующем XML-документе имена базовых таблиц, возвращаемые в виде имен элементов и псевдонимов таблиц, не используются:  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Person.Contact F="Nancy" L="Fuller" />   
  <Person.Contact F="Andrew" L="Peacock" />   
  <Person.Contact F="Janet" L="Leverling" />   
       ...  
</root>  
```  
  
  

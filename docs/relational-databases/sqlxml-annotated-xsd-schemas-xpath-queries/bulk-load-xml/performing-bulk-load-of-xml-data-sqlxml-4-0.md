---
title: Выполнение групповой загрузки XML-данных (SQLXML)
description: Узнайте, как с помощью групповой загрузки XML в SQLXML 4,0 загружать частично структурированные XML-данные в Microsoft SQL Serverные таблицы.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XML Bulk Load [SQLXML]
- bulk load [SQLXML]
- data insertions [SQLXML]
- SQLXML, bulk loading
- XSD schemas [SQLXML], XML Bulk Load
- XDR schemas [SQLXML], XML Bulk Load
- inserting data
ms.assetid: 3708b493-322e-4f3c-9b27-441d0c0ee346
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 619279886e839784b4bf40a1dfc8d59dc3564272
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490632"
---
# <a name="performing-bulk-load-of-xml-data-sqlxml-40"></a>Выполнение массовой загрузки XML-данных (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Массовая загрузка XML является самостоятельным объектом COM, который позволяет загрузить частично структурированные XML-данные в таблицы Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>в этом разделе  
 [Общие сведения о групповой загрузке XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/introduction-to-xml-bulk-load-sqlxml-4-0.md)  
 Содержит основные сведения о массовой загрузке XML-данных при помощи программы XML Bulk Load. Разделы включают в себя описание потоковых XML-данных, а также синхронные и асинхронные операции массовой загрузки.  
  
 [Процесс создания записей &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/record-generation-process-sqlxml-4-0.md)  
 Содержит описание процесса и правил, посредством которых создаются записи для массовой загрузки XML.  
  
 [Интерпретация аннотации &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sqlxml-4-0.md)  
 Содержит описание того, как массовая загрузка XML интерпретирует заметки в схемах XSD и XDR.  
  
 [SQL Server модели объектов XML для групповой загрузки &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/sql-server-xml-bulk-load-object-model-sqlxml-4-0.md)  
 Описывает объект Склксмлбулклоад, его методы и свойства.  
  
 [Примеры групповой загрузки XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md)  
 Содержит пример кода, использующего массовую загрузку XML.  
  
 [Типы данных и поведение при выполнении групповой загрузки XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/data-types-and-xml-bulk-load-behavior-sqlxml-4-0.md)  
 Содержит описание поведения массовой загрузки XML при различных типах в XSD и XDR.  
  
 [Рекомендации и ограничения для групповой загрузки XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/guidelines-and-limitations-of-xml-bulk-load-sqlxml-4-0.md)  
 Содержит список проблем, которые необходимо учитывать при работе с массовой загрузкой XML.  
  
  

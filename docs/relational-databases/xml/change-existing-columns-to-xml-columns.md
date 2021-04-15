---
title: Замена существующих столбцов на столбцы XML | Документация Майкрософт
description: Узнайте, как с помощью инструкции ALTER TABLE можно изменить столбец строкового типа на столбец типа данных XML.
ms.custom: fresh2019may
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- tables [XML]
ms.assetid: 0d951424-9862-41fe-bd46-127f1c059bcb
author: rothja
ms.author: jroth
ms.openlocfilehash: 2cb83b4646b7aab950bbaf8bd3be2fd3fc8d44fb
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489009"
---
# <a name="change-existing-columns-to-xml-columns"></a>Замена существующих столбцов на XML-столбцы

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Инструкция ALTER TABLE поддерживает тип данных **xml** . Например, можно преобразовать столбец любого строкового типа в тип данных **xml** . Учтите, что в этом случае документы, содержащиеся в этом столбце, должны быть корректными. Также при изменении типа столбца со строкового на типизированный xml, содержащиеся в столбце документы должны проходить проверку по указанным XSD-схемам.  
  
```sql
CREATE TABLE T (Col1 int primary key, Col2 nvarchar(max));
GO  
INSERT INTO T   
  VALUES (1, '<Root><Product ProductID="1"/></Root>');
GO  
ALTER TABLE T   
  ALTER COLUMN Col2 xml;
```  
  
Можно изменить тип содержащихся в столбце `xml` данных с нетипизированного на типизированный XML. Пример:  
  
```sql
CREATE TABLE T (Col1 int primary key, Col2 xml);
GO  
INSERT INTO T   
  values (1, '<p1:ProductDescription ProductModelID="1"   
xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
            </p1:ProductDescription>');
GO   
-- Make it a typed xml column by specifying a schema collection.  
ALTER TABLE T   
  ALTER COLUMN Col2 xml (Production.ProductDescriptionSchemaCollection);
```  
  
> [!NOTE]  
> Cкрипт будет работать с базой данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , потому что коллекция XML-схем `Production.ProductDescriptionSchemaCollection`создана в виде части базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
 В предыдущем примере все сохраненные в столбце экземпляры проверяются на корректность и типизацию в соответствии с XSD-схемами из указанной коллекции. Если столбец содержит хотя бы один экземпляр XML, не проходящий проверку на соответствие указанной схеме, выполнение инструкции `ALTER TABLE` завершится ошибкой, и преобразование нетипизированного XML-столбца в типизированный будет невозможно.  
  
> [!NOTE]  
> Если таблица имеет большой размер, операция изменения столбца типа **xml** может требовать много ресурсов. Причиной этого является необходимость проверки каждого документа на корректность, а для типизированного XML — также и проверки соответствия схеме.  
  
Дополнительные сведения о типизированном XML см. в разделе [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  

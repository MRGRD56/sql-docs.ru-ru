---
title: 'SET SQL: инверсия атрибута в SQL: Relationship (SQLXML)'
description: 'Узнайте, как использовать атрибут SQL: инверсию в элементе SQL: relationship для указания связей между столбцами базы данных в операции диаграмма обновления.'
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:relationship
- bulk load [SQLXML]
- inverse attribute
- relationships [SQLXML], inverse orders
- relationship annotation
- XSD schemas [SQLXML], relationships
- annotated XSD schemas, relationships
- updategrams [SQLXML], relationships
- sql:inverse
ms.assetid: 08904cbd-9c86-493d-90c3-f5e1d13ce59d
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bcc10bf5d886679bf5048776154a2548c3bce2d2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490716"
---
# <a name="specifying-the-sqlinverse-attribute-on-sqlrelationship-sqlxml-40"></a>Задание значения атрибута sql:inverse для sql:relationship (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Атрибут **SQL: инверсия** полезен только в том случае, если схема XSD используется для групповой загрузки или диаграмма обновления. Атрибут **SQL: инверсия** может быть указан в **\<sql:relationship>** элементе. В диаграммах обновления их логика задействует схему при определении таблиц и столбцов, обновляемых операцией диаграммы обновления. Связи типа «родители-потомки», заданные в схеме, определяют порядок, в котором записи будут изменены (вставлены или удалены).  
  
 Если в схеме XSD связь «родители-потомки» задана в обратном порядке отношения «первичный ключ — внешний ключ» между соответствующими столбцами базы данных, операции вставки или удаления диаграммы обновления завершатся ошибкой из-за нарушения первичного ключа или внешнего ключа. В таких случаях задается атрибут **SQL: инверсия** (**SQL: Инверсия = "true"**) в **\<sql:relationship>** элементе, а логика диаграмма обновления обратная интерпретация связи типа "родители-потомки", заданной в схеме.  
  
 Атрибут **SQL: инверсия** принимает логическое значение (0 = false, 1 = true). Допустимые значения: 0, 1, true и false.  
  
 Рабочий пример с использованием аннотации **SQL: обратно** см. [в разделе Указание схемы сопоставления с заметками в диаграмма обновления](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
## <a name="see-also"></a>См. также  
 [Указание связей с помощью SQL: relationship &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md)  
  
  

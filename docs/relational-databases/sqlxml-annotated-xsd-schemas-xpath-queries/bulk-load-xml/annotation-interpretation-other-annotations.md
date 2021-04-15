---
title: Другие заметки (SQLXML)
description: Просмотрите список аннотаций SQLXML с описанием того, как каждая из них интерпретируется при выполнении групповой загрузки XML.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- url-encode annotation
- sql:key-fields
- use-cdata annotation
- sql:is-mapping-schema
- sql:url-encode
- sql:id-prefix
- sql:use-cdata
- key-fields annotation
- id-prefix annotation [SQLXML]
- is-mapping-schema annotation
ms.assetid: f7b4d37b-d6d3-4ac3-b2fd-a0b534a924e4
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d5d2270b60e8bbbcb4e63df0f2fcd29e78768de
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490740"
---
# <a name="annotation-interpretation---other-annotations"></a>Интерпретация заметки — прочие заметки
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Помимо заметок, описанных в предыдущих подразделах этого раздела, массовая загрузка XML интерпретирует ниже приведенные заметки.  
  
 **sql:id-prefix**  
 Если схема указывает префиксы к XML-данным, то перед передачей данных в Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]массовая загрузка XML удаляет эти префиксы.  
  
 **sql:use-cdata**  
 Массовая загрузка XML считывает текст, хранящийся в разделах CDATA, и отправляет его в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 **sql:url-encode**  
 Массовая загрузка XML не поддерживает эту заметку. Например, в выходных XML-данных нельзя указать URL-адрес и ожидать, что массовая загрузка XML считает данные из местоположения и сохранит их в базе данных.  
  
 **sql:is-mapping-schema**  
 Массовая загрузка XML не поддерживает ни эту заметку, ни **sql:id**.  
  
> [!NOTE]  
>  Массовая загрузка XML не поддерживает встроенные схемы сопоставления.  
  
 **sql:key-fields**  
 Массовая загрузка XML никогда не обрабатывает эту заметку.  
  
## <a name="see-also"></a>См. также  
 [Интерпретация аннотации &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/annotation-interpretation-sqlxml-4-0.md)  
  
  

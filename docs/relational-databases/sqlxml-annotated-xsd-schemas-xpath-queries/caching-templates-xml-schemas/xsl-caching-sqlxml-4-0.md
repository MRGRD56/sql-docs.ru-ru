---
title: Кэширование XSL (SQLXML)
description: Узнайте, как кэшировать таблицы стилей XSL и установить размер кэша XSL для повышения производительности запросов в SQLXML 4,0.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- registry keys [SQLXML]
- cache [SQLXML]
- XSL caching [SQLXML]
ms.assetid: 91994142-32f0-4d8d-a8cf-eb0d8b1f1999
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 39c5459d1ee7a3eaeedf8a395c4eec80d695f075
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491951"
---
# <a name="xsl-caching-sqlxml-40"></a>Кэширование XSL (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Кэширование таблиц стилей XSL повышает производительность. Если кэширование XSL включено, то после первого выполнения таблица стилей XSL остается в памяти. Это повышает производительность последующей обработки. Значение по умолчанию — ON.  
  
 Можно задать размер кэша XSL, добавив в реестр следующий раздел.  
  
```  
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML4\XSLCacheSize  
```  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoteRegistry](../../../includes/ssnoteregistry-md.md)]  
  
 Размер кэша XSL должен указываться исходя из доступного объема памяти и количества используемых таблиц стилей XSL. По умолчанию значение **XSLCacheSize** равно 31. Размер кэша можно увеличить, если доступ к XSL кажется медленным, или уменьшить при небольшом объеме памяти.  
  
 Чтобы обеспечить наилучшую производительность, рекомендуется указывать значение **XSLCacheSize** , превышающее число обычно используемых таблиц стилей XSL. Если значение **XSLCacheSize** меньше числа имеющихся таблиц стилей XSL, то производительность снижается по мере увеличения количества таблиц стилей XSL. Максимальное значение **XSLCacheSize** — 128.  
  
 При каждом использовании кэшированной таблицы стилей XSL проверяется время изменения файла XSL, чтобы определить необходимость его обновления. Это происходит потому, что копия на диске новее копии в кэше.  
  
## <a name="see-also"></a>См. также  
 [Кэширование шаблонов &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/template-caching-sqlxml-4-0.md)   
 [Кэширование схемы &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/schema-caching-sqlxml-4-0.md)  
  
  

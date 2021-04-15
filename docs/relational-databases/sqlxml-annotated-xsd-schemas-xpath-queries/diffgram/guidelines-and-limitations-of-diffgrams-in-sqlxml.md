---
description: Рекомендации и действующие ограничения SQLXML, связанные с дельтами
title: Рекомендации и действующие ограничения SQLXML, связанные с дельтами
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- DiffGrams [SQLXML], about DiffGrams
ms.assetid: cf8689c4-2a63-4d05-b202-21b5ff187d7f
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 737cbcef2cead359c4f4cfbab0a9d06e58405826
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491868"
---
# <a name="guidelines-and-limitations-of-diffgrams-in-sqlxml"></a>Рекомендации и действующие ограничения SQLXML, связанные с дельтами
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  При использовании дельт с SQLXML 4.0 учитывайте следующее.  
  
-   Типы больших двоичных объектов (BLOB), такие как **Text/ntext** и Images, не следует использовать в **\<diffgr:before>** блоке в при работе с дельтами, так как он будет включать их для использования в управлении параллелизмом. Это может вызвать проблемы в работе [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] из-за ограничений, налагаемых на сравнение для типов больших двоичных объектов. Например, ключевое слово LIKE используется в предложении WHERE для сравнения столбцов типа данных **Text** . Однако в случае с типами больших двоичных объектов, где размер данных превышает 8 КБ, сравнение завершится ошибкой.  
  
-   Специальные символы в данных типа **ntext** могут вызвать проблемы с SQLXML 4,0 из-за ограничений на сравнение типов больших двоичных объектов. Например, при использовании "[Serializable]" в **\<diffgr:before>** блоке DiffGram при проверке параллелизма столбца типа **ntext** произойдет сбой со следующим описанием ошибки SQLOLEDB:  
  
    ```  
    Empty update, no updatable rows found   Transaction aborted  
    ```  
  
  

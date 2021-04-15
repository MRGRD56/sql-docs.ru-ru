---
title: Рекомендации по безопасности схемы с заметками (SQLXML)
description: Сведения о рекомендациях по безопасности для использования схем с заметками в SQLXML 4,0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping schema [SQLXML], security
- annotated XDR schemas, security
- XDR schemas [SQLXML], security
- annotations [SQLXML]
- annotated XSD schemas, security
- SQLXML, annotated XSD schemas
- SQLXML, annotated XDR schemas
- security [SQLXML], annotated schemas
- XSD schemas [SQLXML], security
ms.assetid: 7d7e44dc-b6d3-4e0f-95c7-8f99930c94f2
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 755ca40c15bbd5af6accd7656575e5c755785d75
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491649"
---
# <a name="annotated-schema-security-considerations-sqlxml-40"></a>Основные понятия о безопасности схемы с заметками (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Далее приводятся рекомендации по обеспечению безопасности при использовании аннотированных схем.  
  
-   Избегайте использования сопоставления по умолчанию в схемах сопоставления.  Сопоставление по умолчанию предоставляет доступ к информации о базе данных (имена таблиц и столбцов) в результирующем XML-документе, поскольку по умолчанию имена элементов сопоставляются именам таблиц, а имена атрибутов — именам столбцов. Поэтому любой пользователь, увидевший XML-документ, получает доступ к информации о таблицах и столбцах базы данных, что представляет собой потенциальную угрозу безопасности.  Чтобы избежать этого риска, в схеме следует пользоваться произвольными именами элементов и атрибутов и явным образом сопоставлять их с таблицами и столбцами с помощью заметки.  Дополнительные сведения об использовании сопоставления по умолчанию при создании схем XSD см. [в разделе Сопоставление элементов и АТРИБУТОВ XSD по умолчанию с таблицами и столбцами &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md).  
  
-   Явное задание сопоставления с помощью заметок предоставляет информацию о базе данных (например, об именах таблиц и столбцов). Поэтому бывает нежелательно предоставлять публичный доступ к этим схемам.  
  
-   Выполнение определенных запросов, таких как указанные в схеме сопоставления с рекурсией (указанная с помощью аннотации **max-depth** с более высоким значением), может занять больше времени. При необходимости можно задать предельное время ожидания, задав свойство времени ожидания команды (в секундах). Пример:  
  
    ```  
    cn.Open "Provider=SQLOLEDB;Server=localhost;Database=tempdb;Integrated Security=SSPI;Command Properties='Command Time Out=50';"  
    ```  
  
## <a name="see-also"></a>См. также:  
 [Схемы XSD с заметками в SQLXML 4.0](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xsd-schemas-in-sqlxml-4-0.md)  
  
  

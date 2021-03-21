---
title: Поддержка распределенных запросов в наборах строк схемы | Документация Майкрософт
description: Интерфейс IDBSchemaRowset драйвера OLE DB Driver for SQL Server возвращает метаданные связанных серверов, чтобы обеспечить поддержку распределенных запросов.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DBPROPSET_SQLSERVERSESSION property
- schema rowsets [OLE DB]
- distributed queries [SQL Server], OLE DB Driver for SQL Server
- OLE DB, schema rowsets
- OLE DB rowsets, schema
- rowsets [OLE DB], schema
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3b687fb38097f7e227ddf6befec24aeae2486ba1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754184"
---
# <a name="schema-rowsets---distributed-query-support"></a>Наборы строк схемы — поддержка распределенных запросов
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Для поддержки распределенных запросов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] интерфейс **IDBSchemaRowset** драйвера OLE DB для SQL Server возвращает метаданные связанных серверов.  
  
 Если свойство SSPROP_QUOTEDCATALOGNAMES набора свойств DBPROPSET_SQLSERVERSESSION имеет значение VARIANT_TRUE, можно указать заключенный в кавычки идентификатор имени каталога (например, "my.catalog"). При ограничении вывода набора строк схемы по каталогу драйвер OLE DB для SQL Server распознает двухкомпонентное имя, которое содержит имя связанного сервера и имя каталога. Для наборов строк схемы в приведенной ниже таблице указание двухкомпонентного имени каталога с именем _связанный\_сервер_ **.** _каталог_ ограничивает выводимые данные подходящим каталогом именованного связанного сервера.  
  
|Набор строк схемы|Ограничение каталога|  
|-------------------|-------------------------|  
|DBSCHEMA_CATALOGS|CATALOG_NAME|  
|DBSCHEMA_COLUMNS|TABLE_CATALOG|  
|DBSCHEMA_PRIMARY_KEYS|TABLE_CATALOG|  
|DBSCHEMA_TABLES|TABLE_CATALOG|  
|DBSCHEMA_FOREIGN_KEYS|PK_TABLE_CATALOG FK_TABLE_CATALOG|  
|DBSCHEMA_INDEXES|TABLE_CATALOG|  
|DBSCHEMA_COLUMN_PRIVILEGES|TABLE_CATALOG|  
|DBSCHEMA_TABLE_PRIVILEGES|TABLE_CATALOG|  
  
> [!NOTE]  
>  Чтобы ограничить набор строк схемы всеми каталогами со связанного сервера, используйте синтаксис *связанный_сервер* (где разделитель-подчеркивание является частью спецификации имени). Этот синтаксис эквивалентен указанию значения NULL для ограничения имени каталога; он также используется, когда связанный сервер указывает на источник данных, который не поддерживает каталоги.  
 
 Драйвер OLE DB для SQL Server определяет набор строк схемы LINKEDSERVERS, который возвращает список источников данных OLE DB, зарегистрированных как связанные серверы.  
  
## <a name="see-also"></a>См. также:  
 [Поддержка набора строк схемы &#40;OLE DB&#41;](../../oledb/ole-db/schema-rowset-support-ole-db.md)   
 [Набор строк LINKEDSERVERS (OLE DB)](../../oledb/ole-db/schema-rowsets-linkedservers-rowset.md)  
  
  

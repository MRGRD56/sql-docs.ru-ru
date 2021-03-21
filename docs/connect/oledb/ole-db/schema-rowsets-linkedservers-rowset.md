---
title: Набор строк LINKEDSERVERS (OLE DB) | Документация Майкрософт
description: Набор строк LINKEDSERVERS перечисляет источники данных организации, которые могут применяться в распределенных запросах в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- LINKEDSERVERS rowset
- enumerating data sources [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 52c70c4034e67e1a6300cbdeb860862100cc0235
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754134"
---
# <a name="schema-rowsets---linkedservers-rowset"></a>Наборы строк схемы — набор строк LINKEDSERVERS
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Набор строк **LINKEDSERVERS** перечисляет источники данных организации, которые могут применяться в распределенных запросах [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Набор строк **LINKEDSERVERS** содержит следующие столбцы.  
  
|Имя столбца|Индикатор типа|Description|  
|-----------------|--------------------|-----------------|  
|SVR_NAME|DBTYPE_WSTR|Имя связанного сервера.|  
|SVR_PRODUCT|DBTYPE_WSTR|Имя производителя или другое имя, задающее тип хранилища данных, представленного именем связанного сервера.|  
|SVR_PROVIDERNAME|DBTYPE_WSTR|Понятное имя поставщика OLE DB, используемого для получения данных с этого сервера.|  
|SVR_DATASOURCE|DBTYPE_WSTR|Строка OLE DB DBPROP_INIT_DATASOURCE, используемая для получения источника данных от поставщика.|  
|SVR_PROVIDERSTRING|DBTYPE_WSTR|Значение OLE DB DBPROP_INIT_PROVIDERSTRING, используемое для получения источника данных от поставщика.|  
|SVR_LOCATION|DBTYPE_WSTR|Строка OLE DB DBPROP_INIT_LOCATION, используемая для получения источника данных от поставщика.|  
  
 Набор строк сортируется по столбцу SRV_NAME, и единственное ограничение поддерживается для столбца SRV_NAME.  
  
## <a name="see-also"></a>См. также:  
 [Поддержка набора строк схемы &#40;OLE DB&#41;](../../oledb/ole-db/schema-rowset-support-ole-db.md)  
  
  

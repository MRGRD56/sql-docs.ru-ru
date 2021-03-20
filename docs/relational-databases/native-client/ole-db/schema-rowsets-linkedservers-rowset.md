---
description: Наборы строк схемы — набор строк LINKEDSERVERS в SQL Server Native Client
title: Набор строк LINKEDSERVERS (OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- LINKEDSERVERS rowset
- enumerating data sources [OLE DB]
ms.assetid: 2633fd8a-65e7-498d-9aed-8e4b1cca2381
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7dd9331df262945d8f5d3962007cc05adda93bcd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743354"
---
# <a name="schema-rowsets---linkedservers-rowset-in-sql-server-native-client"></a>Наборы строк схемы — набор строк LINKEDSERVERS в SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

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
 [Поддержка набора строк схемы &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/schema-rowset-support-ole-db.md)  
  
  

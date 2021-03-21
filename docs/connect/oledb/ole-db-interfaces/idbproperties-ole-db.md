---
title: IDBProperties (драйвер OLE DB) | Документация Майкрософт
description: Узнайте об интерфейсе IDBProperties в OLE DB Driver for SQL Server, в том числе о методе IDBProperties::GetPropertyInfo.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8cb88bb3064dde4f997095cb8d39e61d26e0ab67
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752604"
---
# <a name="idbproperties-ole-db"></a>IDBProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Стандартная спецификация OLE DB допускает задание поставщиками VT_EMPTY для **DBPROPINFO::vValues**. Однако драйвер OLE DB для SQL Server всегда возвращает VT_EMPTY при вызове **IDBProperties::GetPropertyInfo** с **DBPROPSET_ROWSETALL** для извлечения свойств набора строк.  
  
## <a name="see-also"></a>См. также:  
 [Интерфейсы (OLE DB)](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md) 
  
  

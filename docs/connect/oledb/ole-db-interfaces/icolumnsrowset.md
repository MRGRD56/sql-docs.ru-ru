---
title: IColumnsRowset (драйвер OLE DB) | Документация Майкрософт
description: Столбец DBCOLUMN_BASETABLEINSTANCE в IColumnsRowset::GetColumnRowset зарезервирован для использования корпорацией Майкрософт в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7453fa75c29537a4609af618f09b8a234b96484b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752644"
---
# <a name="icolumnsrowset"></a>IColumnsRowset
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server добавляет столбец DBCOLUMN_BASETABLEINSTANCE в набор, возвращаемый методом IColumnsRowset::GetColumnRowset. Этот столбец имеет тип данных DBTYPE_I2 и зарезервирован корпорацией Майкрософт для будущего использования. В будущих версиях данные из этого столбца могут быть изменены.  
  
## <a name="see-also"></a>См. также:  
 [Интерфейсы (OLE DB)](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md) 
  
  

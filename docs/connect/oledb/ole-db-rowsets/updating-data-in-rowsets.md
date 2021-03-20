---
title: Обновление данных в наборах строк (драйвер OLE DB)
description: Узнайте, как драйвер OLE DB для SQL Server обновляет данные SQL Server, если объект-получатель обновляет изменяемый набор строк, содержащий эти данные.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- updating data [SQL Server]
- rowsets [OLE DB], updating data
- OLE DB Driver for SQL Server, rowsets
- OLE DB rowsets, updating data
- OLE DB Driver for SQL Server, data updates
- data updates [SQL Server], OLE DB
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a9bb3a04af6f75eda05a46da655a971bd60cc71
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755774"
---
# <a name="updating-data-in-rowsets"></a>Обновление данных в наборах строк
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Драйвер OLE DB для SQL Server обновляет данные [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], если потребитель обновляет изменяемый набор строк, содержащий эти данные. Набор строк, который можно изменять, создается, если потребитель запрашивает поддержку либо для интерфейса **IRowsetChange**, либо для интерфейса **IRowsetUpdate**.  
  
 Все изменяемые наборы строк OLE DB Driver for SQL Server клиента используют курсоры [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для поддержки набора строк. Свойство DBPROP_LOCKMODE набора строк изменяет поведение управления параллелизмом [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в курсорах, а также определяет поведение выборки строки из набора строк и создание ошибок целостности данных в наборах строк, которые можно обновлять.  
  
 OLE DB Driver for SQL Server поддерживает синхронизацию строк до или после обновления.  
  
> [!NOTE]  
>  Интерфейс IRowChange::SetColumns может установить значения одного или более именованных столбцов объекта-строки.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Обновление данных в курсорах SQL Server](../../oledb/ole-db-rowsets/updating-data-in-sql-server-cursors.md)  
  
-   [Повторная синхронизация строк](../../oledb/ole-db-rowsets/updating-data-in-rowsets-resynchronizing-rows.md)  
  
## <a name="see-also"></a>См. также:  
 [Наборы строк](../../oledb/ole-db-rowsets/rowsets.md)  
  
  

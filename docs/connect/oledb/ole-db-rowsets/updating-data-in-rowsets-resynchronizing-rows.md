---
title: Повторная синхронизация строк (драйвер OLE DB)
description: Для обновления данных в наборе строк OLE DB Driver for SQL Server поддерживает интерфейс IRowsetResynch только в наборах строк с поддержкой курсора SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- synchronization [OLE DB]
- IRowsetResynch interface
- resynchronizing rows
- data updates [SQL Server], OLE DB
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f1553c5f077f41212583bbfb47d0fad4a73621c6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755974"
---
# <a name="updating-data-in-rowsets---resynchronizing-rows"></a>Обновление данных в наборах строк — повторная синхронизация строк
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server поддерживает интерфейс **IRowsetResynch** только в наборах строк [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с поддержкой курсора. Интерфейс **IRowsetResynch** не предоставляется по требованию. Пользователь должен запросить этот интерфейс перед открытием набора строк.  
  
## <a name="see-also"></a>См. также:  
 [Обновление данных в наборах строк](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
  

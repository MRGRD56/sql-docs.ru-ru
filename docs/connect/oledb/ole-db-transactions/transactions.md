---
title: Транзакции (драйвер OLE DB)
description: Узнайте о поддержке локальных транзакций в OLE DB Driver for SQL Server. Используйте для распределенных транзакций координатор распределенных транзакций Microsoft.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- transactions [OLE DB]
- OLE DB Driver for SQL Server, transactions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: da8716e20f060742f04fb628820ddc4aa7e70576
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754194"
---
# <a name="transactions"></a>Транзакции
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server реализует поддержку только для локальных транзакций. Потребитель может использовать распределенные или координируемые транзакции с помощью координатора распределенных транзакций (Майкрософт) (MS DTC). Для потребителей, которым требуется управление транзакциями, охватывающее несколько сеансов, драйвер OLE DB для SQL Server может соединять транзакции, инициированные и обслуживаемые координатором MS DTC.  
  
 По умолчанию драйвер OLE DB для SQL Server использует режим автоматической фиксации транзакции, в котором каждое отдельное действие в сеансе потребителя составляет полную транзакцию для экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Режим автоматической фиксации драйвера OLE DB для SQL Server локальный, и транзакции с автоматической фиксацией никогда не принадлежат более чем одному сеансу.  
  
 Драйвер OLE DB для SQL Server предоставляет интерфейс **ITransactionLocal**, что позволяет потребителю использовать явно и неявно запускаемые транзакции в одном подключении к экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. OLE DB Driver for SQL Server не поддерживает вложенные локальные транзакции.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Поддержка локальных транзакций](../../oledb/ole-db-transactions/supporting-local-transactions.md)  
  
-   [Поддержка распределенных транзакций](../../oledb/ole-db-transactions/supporting-distributed-transactions.md)  
  
-   [Уровни изоляции (OLE DB)](../../oledb/ole-db-transactions/isolation-levels-ole-db.md)  
  
## <a name="see-also"></a>См. также:  
 [Программирование драйвера OLE DB для SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  

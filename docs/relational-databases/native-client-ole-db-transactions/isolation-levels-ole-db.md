---
description: Уровни изоляции (поставщик собственного клиента OLE DB)
title: Уровни изоляции (поставщик собственного клиента OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- isolation levels [OLE DB]
- transactions [OLE DB]
- SQL Server Native Client OLE DB provider, transactions
ms.assetid: d70ee72c-6e2a-4bcd-9456-4a697a866361
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23ab78551da0fa92d0d471f91ad5ce2147ae574f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743544"
---
# <a name="isolation-levels-native-client-ole-db-provider"></a>Уровни изоляции (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Клиенты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут управлять уровнями изоляции транзакций для соединения. Чтобы управлять уровнем изоляции транзакций, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребитель поставщика OLE DB собственного клиента использует:  
  
-   DBPROPSET_SESSION DBPROP_SESS_AUTOCOMMITISOLEVELS свойства для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] режима автоматической фиксации по умолчанию для поставщика собственного клиента OLE DB.  
  
     [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента для уровня по умолчанию имеет значение DBPROPVAL_TI_READCOMMITTED.  
  
-   Параметр *isoLevel* метода **ITransactionLocal::StartTransaction** для локальных транзакций с ручной фиксацией.  
  
-   Параметр *isoLevel* метода **ITransactionDispenser::BeginTransaction** для распределенных транзакций с координацией MS DTC.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] разрешает доступ только для чтения на уровне изоляции чтения «грязных» данных. Все другие уровни ограничивают параллелизм применением блокировок к объектам [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если клиент требует более высоких уровней параллелизма, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] применяет более строгие ограничения на параллельные обращения к данным. Чтобы обеспечить максимальный уровень одновременного доступа к данным, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребитель поставщика OLE DB собственного клиента должен управлять своими запросами для конкретных уровней параллелизма.  
  
> [!NOTE]  
>  В [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] появился уровень изоляции моментального снимка. Дополнительные сведения см. в разделе [Working with Snapshot Isolation](../../relational-databases/native-client/features/working-with-snapshot-isolation.md).  
  
## <a name="see-also"></a>См. также:  
 [Транзакции](../../relational-databases/native-client-ole-db-transactions/transactions.md)  
  
  

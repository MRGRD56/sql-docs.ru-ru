---
title: Результаты сообщения SQL Server (драйвер OLE DB)
description: Сведения об инструкциях Transact-SQL, которые не создают наборов строк OLE DB Driver for SQL Server и не возвращают количество строк, а также ожидаемые возвращаемые значения.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- errors [OLE DB], SQL Server message results
- OLE DB error handling, SQL Server message results
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dabbcef5f1d894f4d9a9c08230a13f60a269ce2f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741724"
---
# <a name="sql-server-message-results"></a>Результаты сообщения SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Следующие инструкции [!INCLUDE[tsql](../../../includes/tsql-md.md)] не создают при работе наборы строк OLE DB Driver for SQL Server и не возвращают количество обработанных строк:  
  
-   PRINT  
  
-   RAISERROR (со степенью серьезности 10 или ниже)  
  
-   DBCC  
  
-   SET SHOWPLAN  
  
-   SET STATISTICS  
  
 Эти инструкции либо возвращают одно или несколько информационных сообщений, либо дают указание [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] вернуть информационные сообщения вместо набора строк или результатов вычислений. При успешном выполнении OLE DB Driver for SQL Server возвращает значение S_OK, а сообщения становятся доступны потребителю OLE DB Driver for SQL Server.  
  
 OLE DB Driver for SQL Server возвращает значение S_OK и предоставляет одно или несколько информационных сообщений после выполнения многих инструкций [!INCLUDE[tsql](../../../includes/tsql-md.md)] или при выполнении потребителем функции-члена OLE DB Driver for SQL Server.  
  
Потребителю OLE DB Driver for SQL Server разрешена динамическая спецификация текста запроса. Потребитель должен проверять интерфейсы ошибок после _каждого_ выполнения функции-члена. Он должен всегда выполнять эти проверки: значение кода возврата, возвращается ли ссылка на интерфейс `IRowset` или `IMultipleResults`, а также количество обработанных строк.
  
## <a name="see-also"></a>См. также:  
 [ошибки](../../oledb/ole-db-errors/errors.md)  
  
  

---
description: Отсоединение от источника данных
title: Отключение от источника данных | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC data sources, connections
- data sources [SQL Server Native Client]
- disconnecting [ODBC]
- ODBC applications, disconnecting
- SQLDisconnect function
- ODBC applications, data sources
- connections [SQL Server Native Client]
- SQLFreeHandle function
- ODBC data sources, disconnecting
- SQL Server Native Client ODBC driver, data sources
- ODBC functions
- SQL Server Native Client ODBC driver, connections
ms.assetid: 65b0267d-b2ab-4a59-83f2-436d90cfbf79
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4864b004cf9bd885697aa2a36aac025ae0bf12a1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756344"
---
# <a name="disconnecting-from-a-data-source"></a>Отсоединение от источника данных
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Когда приложение завершило работу с источником данных, оно вызывает **SQLDisconnect**. **SQLDisconnect** освобождает все инструкции, выделенные для соединения, и отключает драйвер от источника данных. После отключения приложение может вызвать [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) , чтобы освободить маркер подключения. Перед выходом приложение также вызывает **SQLFreeHandle** , чтобы освободить обработчик среды.  
  
 После отсоединения приложение может повторно использовать выделенный дескриптор соединения, либо для соединения с другим источником данных, либо для повторного соединения с тем же. Для принятия решения о сохранении соединения или отсоединении и повторном соединении, разработчик приложения должен рассмотреть сравнительную стоимость каждого варианта. Соединение с источником данных и сохранение соединения в различных окружениях могут оказаться в разной степени затратными. Чтобы сделать выбор, следует также проанализировать вероятность и временные затраты других операций в том же источнике данных. Также приложению может потребоваться более одного соединения.  
  
## <a name="see-also"></a>См. также:  
 [Взаимодействие с SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  

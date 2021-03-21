---
description: Создание приложения драйвера. Асинхронный режим и команда SQLCancel
title: Асинхронный режим и SQLCancel | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- asynchronous operations [SQL Server Native Client]
- SQLCancel function
- SQLSetConnectAttr function
- SQL Server Native Client, asynchronous operations
- ODBC functions
- ODBC applications, asynchronous operations
- SQL Server Native Client ODBC driver, asynchronous mode
ms.assetid: f31702a2-df76-4589-ac3b-da5412c03dc2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d899640c5a5800246b191ebf0bfc3d94e790104c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755264"
---
# <a name="creating-a-driver-application---asynchronous-mode-and-sqlcancel"></a>Создание приложения драйвера. Асинхронный режим и команда SQLCancel
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Некоторые функции ODBC могут работать как синхронно, так и асинхронно. В приложении могут быть разрешены асинхронные операции как для дескриптора инструкции, так и для дескриптора соединения. Если параметр установлен для дескриптора соединения, он затрагивает все дескрипторы инструкции в этом дескрипторе соединения. Приложение использует следующие инструкции, чтобы включить или отключить асинхронный режим:  
  
```  
SQLSetConnectAttr(hdbc, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_ON, SQL_IS_INTEGER);  
SQLSetConnectAttr(hdbc, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_OFF, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_ON, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_OFF, SQL_IS_INTEGER);  
```  
  
 Когда приложение вызывает функцию ODBC в синхронном режиме, драйвер не возвращает управление приложению, пока не получит уведомление о том, что сервер завершил выполнение команды.  
  
 В асинхронном режиме драйвер возвращает управление приложению немедленно, даже перед отправкой команды серверу. Драйвер выставляет код возврата в значение SQL_STILL_EXECUTING. Приложение может выполнять другую работу.  
  
 Когда приложение проверяет завершение выполнения команды, делается тот же вызов функции с теми же параметрами для драйвера. Если драйвер еще не получил ответ от сервера, он опять вернет значение SQL_STILL_EXECUTING. Приложение должно проверять команду периодически до тех пор, пока код возврата не станет отличен от SQL_STILL_EXECUTING. Когда приложение получает любой другой код возврата, даже SQL_ERROR, оно может определить, что выполнение команды завершено.  
  
 Иногда команда остается необработанной долгое время. Если приложению нужно отменить команду, не дожидаясь ответа, это можно сделать, вызвав **SQLCancel** с тем же маркером инструкции, что и у необработанной команды. Это единственный момент, когда следует использовать **SQLCancel** . Некоторые программисты используют **SQLCancel** , когда они обрабатывали фрагменты в результирующем наборе и хотят отменить оставшуюся часть результирующего набора. [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) или [SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md) следует использовать для отмены оставшейся части необработанного результирующего набора, а не **SQLCancel**.  
  
## <a name="see-also"></a>См. также:  
 [Создание драйвера ODBC для собственного клиента SQL Server](../../../relational-databases/native-client/odbc/creating-a-driver-application.md)  
  
  

---
description: Неявные преобразования курсора (ODBC)
title: Неявные преобразования курсора (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC cursors, implicit cursor conversions
- implicit cursor conversions
- cursors [ODBC], implicit cursor conversions
ms.assetid: fe29a58d-8448-4512-9ffd-b414784ba338
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3be6e618fd4b5b34c7c0fbe4d51272ea339c6d66
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756254"
---
# <a name="implicit-cursor-conversions-odbc"></a>Неявные преобразования курсора (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Приложения могут запрашивать тип курсора через [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) , а затем выполнять инструкцию SQL, которая не поддерживается серверными курсорами запрошенного типа. Вызов **SQLExecute** или **SQLExecDirect** возвращает SQL_SUCCESS_WITH_INFO и **SQLGetDiagRec** возвращает:  
  
```  
szSqlState = "01S02", *pfNativeError = 0,  
szErrorMsg="[Microsoft][SQL Server Native Client] Cursor type changed"  
```  
  
 Приложение может определить, какой тип курсора теперь используется, вызвав **SQLGetStmtOption** в значение SQL_CURSOR_TYPE. Преобразование типа курсора применяется только к одной инструкции. Следующие **SQLExecDirect** или **SQLExecute** будут выполняться с использованием параметров курсора исходной инструкции.  
  
## <a name="see-also"></a>См. также:  
 [Сведения о программировании курсора &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-cursors/programming/cursor-programming-details-odbc.md)  
  
  

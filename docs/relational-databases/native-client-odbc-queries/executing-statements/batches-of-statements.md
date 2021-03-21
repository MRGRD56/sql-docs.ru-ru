---
description: Пакеты инструкций
title: Пакеты инструкций | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statements [ODBC], batches
- batches [ODBC]
- ODBC applications, statements
- multiple statements, batches
- SQLMoreResults function
- SQLExecDirect function
ms.assetid: 057d7c1c-1428-4780-9447-a002ea741188
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fcc872cbc1c1c342524abef7142467adb8c4fb2e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750784"
---
# <a name="batches-of-statements"></a>Пакеты инструкций
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Пакет [!INCLUDE[tsql](../../../includes/tsql-md.md)] инструкций содержит две или более инструкции, разделенные точкой с запятой (;), встроенными в одну строку, переданную в функцию **SQLExecDirect** или [SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md). Пример:  
  
```  
SQLExecDirect(hstmt,   
    "SELECT * FROM Authors; SELECT * FROM Titles",  
    SQL_NTS);  
```  
  
 Пакеты могут быть более эффективными, чем отправка инструкций по одной, так как они часто уменьшают требуемый сетевой трафик. Используйте [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) , чтобы перейти к следующему результирующему набору по завершении текущего результирующего набора.  
  
 Пакеты инструкций всегда можно использовать, если атрибуты курсора ODBC установлены по умолчанию (однопроходный курсор только для чтения), а размер набора строк равен 1.  
  
 Если инструкция выполняется в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], с помощью серверных курсоров, то серверный курсор неявно преобразуется в результирующий набор по умолчанию. **SQLExecDirect** или **SQLExecute** возвращают SQL_SUCCESS_WITH_INFO, а вызов **SQLGetDiagRec** возвращает:  
  
```  
szSqlState = "01S02", pfNativeError = 0  
szErrorMsg = "[Microsoft][SQL Server Native Server Native Client]Cursor type changed."  
```  
  
## <a name="see-also"></a>См. также:  
 [Исполнение инструкций &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  

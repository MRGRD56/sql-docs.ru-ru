---
description: Обработка инструкций, выдающих сообщения
title: Обработка инструкций, создающих сообщения | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- PRINT statement
- messages [ODBC], statements generating messages
- statements [ODBC], message generation
- errors [ODBC], statements generating messages
- SQL Server Native Client ODBC driver, errors
- STATISTICS IO option
- STATISTICS TIME option
- DBCC statements
- SQLExecute function
- RAISERROR statement
- SQLGetDiagRec function
- ODBC error handling, statements generating messages
- SQLExecDirect function
ms.assetid: 672ebdc5-7fa1-4ceb-8d52-fd25ef646654
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fc3ce91857b1ecb650df733958e4715a03893fd5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746134"
---
# <a name="processing-statements-that-generate-messages"></a>Обработка инструкций, выдающих сообщения
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Параметры STATISTICS TIME и STATISTICS IO инструкции SET языка [!INCLUDE[tsql](../../includes/tsql-md.md)] используются для получения сведений, помогающих при диагностике долго выполняющихся запросов. Предыдущие версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] также поддерживают параметр SHOWPLAN для анализа планов запросов. Приложение ODBC может установить эти параметры с помощью следующих инструкций:  
  
```  
SQLExecDirect(hstmt, "SET SHOWPLAN ON", SQL_NTS);  
SQLExecDirect(hstmt, "SET STATISTICS TIME ON", SQL_NTS90  
);  
SQLExecDirect(hstmt, "SET STATISTICS IO ON", SQL_NTS);  
```  
  
 Если параметр SET STATISTICS TIME или SET SHOWPLAN имеет значение ON, то **SQLExecute** и **SQLExecDirect** возвращают SQL_SUCCESS_WITH_INFO, и на этом этапе приложение может получить выходные данные инструкции Showplan или Statistics, вызвав **SQLGetDiagRec** , пока не вернет SQL_NO_DATA. Каждая строка данных SHOWPLAN возвращается в следующем формате.  
  
```  
szSqlState="01000", *pfNativeError=6223,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]   
              Table Scan"  
```  
  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версии 7.0 параметр SHOWPLAN заменен на SHOWPLAN_ALL и SHOWPLAN_TEXT, которые возвращают выходные данные в виде результирующего набора, а не набора сообщений.  
  
 Каждая строка STATISTICS TIME возвращается в следующем формате.  
  
```  
szSqlState="01000", *pfNativeError= 3613,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
   SQL Server Parse and Compile Time: cpu time = 0 ms."  
```  
  
 Выходные данные SET STATISTICS IO недоступны до завершения результирующего набора. Чтобы получить СТАТИСТИКУ ввода-вывода статистики, приложение вызывает **SQLGetDiagRec** во время **SQLFetch** или [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) возвращает SQL_NO_DATA. Выходные данные STATISTICS IO возвращаются в следующем формате.  
  
```  
szSqlState="01000", *pfNativeError= 3615,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Table: testshow  scan count 1,  logical reads: 1,  
   physical reads: 0."  
```  
  
## <a name="using-dbcc-statements"></a>Использование инструкций DBCC  
 Инструкции DBCC возвращают данные в виде сообщений, а не результирующих наборов. **SQLExecDirect** или **SQLExecute** возвращают SQL_SUCCESS_WITH_INFO, и приложение получает выходные данные, вызывая **SQLGetDiagRec** , пока не вернет SQL_NO_DATA.  
  
 Например, следующая инструкция возвращает значение SQL_SUCCESS_WITH_INFO.  
  
```  
SQLExecDirect(hstmt, "DBCC CHECKTABLE(Authors)", SQL_NTS);  
```  
  
 Вызовы **SQLGetDiagRec** возвращают:  
  
```  
szSqlState = "01000", *pfNativeError = 2536,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Checking authors"  
szSqlState = "01000", *pfNativeError = 2579,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   The total number of data pages in this table is 1."  
szSqlState = "01000", *pfNativeError = 7929,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Table has 23 data rows."  
szSqlState = "01000", *pfNativeError = 2528  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   DBCC execution completed. If DBCC printed error messages,  
   see your System Administrator."  
```  
  
## <a name="using-print-and-raiserror-statements"></a>Использование инструкций PRINT и RAISERROR  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] Инструкции PRINT и RAISERROR также возвращают данные путем вызова **SQLGetDiagRec**. Инструкции PRINT приводят к тому, что выполнение инструкции SQL возвращает SQL_SUCCESS_WITH_INFO, а последующий вызов **SQLGetDiagRec** возвращает значение *SQLSTATE* 01000. Инструкция RAISERROR со степенью серьезности 10 или ниже работает так же, как PRINT. Инструкция RAISERROR с уровнем серьезности 11 или выше приводит к тому, что оператор Execute возвращает SQL_ERROR, а последующий вызов **SQLGetDiagRec** возвращает *SQLSTATE* 42000. Например, следующая инструкция возвращает значение SQL_SUCCESS_WITH_INFO.  
  
```  
SQLExecDirect (hstmt, "PRINT  'Some message' ", SQL_NTS);  
```  
  
 Вызов **SQLGetDiagRec** возвращает:  
  
```  
szSQLState = "01000", *pfNative Error = 0,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Some message"  
```  
  
 Следующая инструкция возвращает значение SQL_SUCCESS_WITH_INFO.  
  
```  
SQLExecDirect (hstmt, "RAISERROR ('Sample error 1.', 10, -1)",  
   SQL_NTS)  
```  
  
 Вызов **SQLGetDiagRec** возвращает:  
  
```  
szSQLState = "01000", *pfNative Error = 50000,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Sample error 1."  
```  
  
 Следующая инструкция возвращает значение SQL_ERROR.  
  
```  
SQLExecDirect (hstmt, "RAISERROR ('Sample error 2.', 11, -1)", SQL_NTS)  
```  
  
 Вызов **SQLGetDiagRec** возвращает:  
  
```  
szSQLState = "42000", *pfNative Error = 50000,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Sample error 2."  
```  
  
 Время вызова **SQLGetDiagRec** является критическим, если выходные данные инструкций Print или RAISERROR включаются в результирующий набор. Вызов **SQLGetDiagRec** для получения выходных данных печати или RAISERROR должен выполняться сразу после инструкции, принимающей SQL_ERROR или SQL_SUCCESS_WITH_INFO. Это просто в случае выполнения отдельной инструкции SQL, как показано в примере выше. В этих случаях вызов **SQLExecDirect** или **SQLExecute** возвращает SQL_ERROR или SQL_SUCCESS_WITH_INFO и **SQLGetDiagRec** может быть вызван. При программировании циклов для обработки выходных данных пакета инструкций SQL или выполнении хранимых процедур [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возникают осложнения.  
  
 В этом случае [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает результирующий набор для каждой инструкции SELECT, выполненной в пакете или хранимой процедуре. Если пакет или процедура содержит инструкции PRINT или RAISERROR, то их выходные данные чередуются с результирующими наборами инструкций. Если первый оператор в пакете или процедуре является ПЕЧАТЬю или ИНСТРУКЦИей RAISERROR, то **SQLExecute** или **SQLExecDirect** возвращает SQL_SUCCESS_WITH_INFO или SQL_ERROR, и приложению необходимо вызвать **SQLGetDiagRec** до тех пор, пока не вернет SQL_NO_DATA для получения сведений о печати или инструкции RAISERROR.  
  
 Если инструкция PRINT или RAISERROR находится после инструкции SQL (например, инструкции SELECT), то сведения о печати или RAISERROR возвращаются, когда [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md)позиции в результирующем наборе, содержащем ошибку. **SQLMoreResults** возвращает SQL_SUCCESS_WITH_INFO или SQL_ERROR в зависимости от серьезности сообщения. Сообщения извлекаются путем вызова **SQLGetDiagRec** до тех пор, пока не будут возвращены SQL_NO_DATA.  
  
## <a name="see-also"></a>См. также:  
 [Обработка ошибок и сообщений](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  

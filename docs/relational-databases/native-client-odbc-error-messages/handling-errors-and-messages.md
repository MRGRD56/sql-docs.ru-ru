---
title: Обработка ошибок и сообщений | Документация Майкрософт
description: Сведения о том, какие диагностические данные возвращаются, когда приложение вызывает функцию ODBC, включая успех или неудачу и подробную информацию.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC error handling, about error handling
- errors [ODBC]
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], about messages
- ODBC error handling
- SQL_INVALID_HANDLE return code
- errors [ODBC], about error handling
- messages [ODBC]
ms.assetid: 74ea9630-e482-4a46-bb45-f5234f079b48
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 27633f14dcba7c61b86c4a734ffd9dd450b012fc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737484"
---
# <a name="handling-errors-and-messages"></a>Обработка ошибок и сообщений
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Когда приложение вызывает функцию ODBC, драйвер выполняет функцию и возвращает диагностическую информацию двумя способами: код возврата указывает на общий успех или сбой функции ODBC, а диагностические записи предоставляют подробные сведения о функции. Диагностическая запись состоит из записи заголовка и записи состояния. При успешном выполнении функции возвращается как минимум одна диагностическая запись, запись заголовка.  
  
 Диагностические сведения используются во время разработки для перехвата программных ошибок, например недопустимых дескрипторов и синтаксических ошибок в жестко запрограммированных инструкциях SQL. Она также используется во время выполнения для захвата ошибок и предупреждений времени выполнения, например усечения данных, нарушения правил и синтаксических ошибок в инструкциях SQL, введенных пользователем. Программная логика обычно основана на кодах возврата.  
  
 Например, после того, как приложение вызывает **SQLFetch** для получения строк в результирующем наборе, код возврата указывает, был ли достигнут конец результирующего набора (SQL_NO_DATA), если были возвращены какие-либо информационные сообщения (SQL_SUCCESS_WITH_INFO) или произошла ошибка (SQL_ERROR).  
  
 Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента возвращает что-либо, кроме SQL_SUCCESS, приложение может вызвать **SQLGetDiagRec** для получения любых информационных или сообщений об ошибках. Используйте **SQLGetDiagRec** для прокрутки набора сообщений вверх и вниз, если имеется более одного сообщения.  
  
 Код возврата SQL_INVALID_HANDLE всегда указывает на программную ошибку, поэтому не должен встречаться во время выполнения. Все другие коды возврата предоставляют сведения времени выполнения, хотя SQL_ERROR может означать программную ошибку.  
  
 Исходный [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный API, DB-Library для C, позволяет приложению устанавливать функции обработки ошибок и обработки сообщений обратного вызова, возвращающие ошибки или сообщения. Некоторые инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], например PRINT, RAISERROR, DBCC и SET, возвращают свои результаты функции обработки сообщений DB-Library, а не результирующему набору. Однако API-интерфейс ODBC не имеет такой возможности обратного вызова. Когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента обнаруживает сообщения, возвращенные из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , он устанавливает код возврата ODBC в SQL_SUCCESS_WITH_INFO или SQL_ERROR и возвращает сообщение в виде одной или нескольких диагностических записей. Поэтому приложение ODBC должно тщательно протестировать эти коды возврата и вызвать **SQLGetDiagRec** для получения данных сообщения.  
  
 Сведения об ошибках трассировки см. в статье [Отслеживание доступа к данным](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). См. сведения об улучшениях трассировки ошибок, добавленных в [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], в руководстве по [получению доступа к диагностическим сведениям в расширенном журнале событий](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Обработка инструкций, выдающих сообщения](../../relational-databases/native-client-odbc-error-messages/processing-statements-that-generate-messages.md)  
  
-   [Диагностические записи и поля](../../relational-databases/native-client-odbc-error-messages/diagnostic-records-and-fields.md)  
  
-   [Собственные коды ошибок](../../relational-databases/native-client-odbc-error-messages/native-error-numbers.md)  
  
-   [SQLSTATE &#40;коды ошибок ODBC&#41;](../../relational-databases/native-client-odbc-error-messages/sqlstate-odbc-error-codes.md)  
  
-   [сообщения об ошибках](../../relational-databases/native-client-odbc-error-messages/error-messages.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (ODBC)](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  

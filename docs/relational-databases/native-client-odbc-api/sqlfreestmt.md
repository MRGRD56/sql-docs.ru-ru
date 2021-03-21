---
description: Функция SQLFreeStmt
title: SQLFreeStmt | Документация Майкрософт
ms.custom: ''
ms.date: 11/23/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLFreeStmt function
ms.assetid: d9666d0b-3446-480e-bf1a-10b01213e411
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92cf1996fe7a4079307e448a80cb4d0f22f5227e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753294"
---
# <a name="sqlfreestmt"></a>Функция SQLFreeStmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Обычно   
      Не рекомендуется применять функцию **SQLFreeStmt** в ODBC 3.0 и более поздних версиях. Однако если приложению необходимо повторно использовать инструкцию, необходимо по-прежнему использовать **SQLFreeStmt** с параметрами **SQL_RESET_PARAMS** и **SQL_UNBIND** ). Вы также можете использовать [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md), [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md), [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md), [SQLSetDescField](../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)и [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) для замены или дублирования функции **SQLFreeStmt** и использовать их вместо них.  
  
 Как правило, более эффективно использовать инструкции, чем удалять их и выделять новые. Однако в некоторых ситуациях, например при повторном использовании инструкций, SQLFreeStmt все равно необходимо использовать.  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLFreeStmt](../../odbc/reference/syntax/sqlfreestmt-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

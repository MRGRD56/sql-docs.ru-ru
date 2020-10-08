---
description: SQLCancel
title: SQLCancel | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLCancel function
ms.assetid: d4c965ae-c1ac-4e9d-b4b9-32b561401106
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 86f50105acb4c506b6ecb937ff0f302f32b24bb3
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91811150"
---
# <a name="sqlcancel"></a>SQLCancel
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В разделе [SQLCancel](../../odbc/reference/syntax/sqlcancel-function.md) говорится, что в ODBC 2. x, если приложение вызывает **SQLCancel** , когда в инструкции не выполняется обработка, **SQLCancel** действует так же, как **SQLFreeStmt** с параметром **SQL_CLOSE** . Это поведение определено только для полноты, и приложения должны вызывать **SQLFreeStmt** или **SQLCloseCursor** для закрытия курсоров. Но даже если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственное клиентское приложение устанавливает версию API ODBC 3.5. x или более поздней версии, функция **SQLCancel** будет использовать поведение ODBC 2. x.  
  
## <a name="see-also"></a>См. также:  
 [SQLCancel](../../odbc/reference/syntax/sqlcancel-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

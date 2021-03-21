---
description: SQLGetCursorName
title: SQLGetCursorName | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetCursorName function
ms.assetid: 3a427a23-28ef-49aa-b9ec-6cab0914bdf3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 26b11982f53c2aba92b4323d3f621ce31387d63a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749134"
---
# <a name="sqlgetcursorname"></a>SQLGetCursorName
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Если приложение не указывает имени курсора, драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] формирует его для приложения после создании курсора. Приложение может использовать **SQLGetCursorName** , чтобы получить определенное драйвером имя курсора для позиционированных инструкций UPDATE и DELETE. Приложению не требуется вызывать **SQLSetCursorName** , чтобы использовать преимущества позиционированных инструкций по обработке данных.  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLGetCursorName](../../odbc/reference/syntax/sqlgetcursorname-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

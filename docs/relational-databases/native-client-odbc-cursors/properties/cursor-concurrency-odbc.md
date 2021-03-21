---
description: Параллелизм курсоров (ODBC)
title: Параллелизм курсоров (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- concurrency [ODBC]
- cursors [ODBC], concurrency
- ODBC cursors, concurrency
ms.assetid: 68228ece-cbf1-4f19-bfdc-053884c1af48
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ad84f6fcc0079ff5cceedb9c37e7b451fd5ba358
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756214"
---
# <a name="cursor-concurrency-odbc"></a>Параллелизм курсоров (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Операции курсора, как и типы курсора, зависят от параметров параллелизма, устанавливаемых приложением. Параметры параллелизма задаются с помощью параметра SQL_ATTR_CONCURRENCY [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). Существуют следующие типы параллелизма.  
  
-   Только для чтения (SQL_CONCUR_READONLY).  
  
-   Значения (SQL_CONCUR_VALUES).  
  
-   Версия строки (SQL_CONCUR_ROWVER).  
  
-   Блокировка (SQL_CONCUR_LOCK).  
  
## <a name="see-also"></a>См. также:  
 [Свойства курсора](../../../relational-databases/native-client-odbc-cursors/properties/cursor-properties.md)  
  
  

---
description: Поддержка разреженных столбцов (ODBC)
title: Поддержка разреженных столбцов (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 11ae959f-2fb6-4b85-ac5d-1476a82136d4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: abec37f7b5683e5cab713800700e4056bf852f2c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745534"
---
# <a name="sparse-columns-support-odbc"></a>Поддержка разреженных столбцов (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В этом разделе описывается поддержка разреженных столбцов драйвером ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Пример, демонстрирующий поддержку ODBC для разреженных столбцов, см. в разделе [вызов SQLColumns для таблицы с разреженными столбцами](../../../relational-databases/native-client-odbc-how-to/call-sqlcolumns-on-a-table-with-sparse-columns.md). Дополнительные сведения о разреженных столбцах см. [в разделе Поддержка разреженных столбцов в SQL Server Native Client](../../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md).  
  
## <a name="statement-metadata"></a>Метаданные инструкции  
 Поле дескриптора в дескрипторе параметра приложения (APD) и атрибут инструкции SQL_SOPT_SS_NAME_SCOPE принимают дополнительные значения SQL_SS_NAME_SCOPE_EXTENDED и SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET. Эти значения определяют, какие столбцы включаются в возвращаемый [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md)результирующий набор. Дополнительные сведения об атрибуте SQL_SOPT_SS_NAME_SCOPE см. в разделе [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Новый дескриптор строки реализации (IRD), доступное только для чтения поле SQL_CA_SS_IS_COLUMN_SET типа SQLSMALLINT, позволяет определить, имеет ли столбец значение XML **column_set** . SQL_CA_SS_IS_COLUMN_SET принимает значения SQL_TRUE and SQL_FALSE.  
  
## <a name="catalog-metadata"></a>Метаданные каталога  
 В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в результирующий набор для [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="odbc-function-support-for-sparse-columns"></a>Поддержка функций ODBC для разреженных столбцов  
 Следующие функции ODBC были обновлены для поддержки разреженных столбцов в собственном клиенте [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   [SQLColAttribute](../../../relational-databases/native-client-odbc-api/sqlcolattribute.md)  
  
-   [SQLColumns](../../../relational-databases/native-client-odbc-api/sqlcolumns.md)  
  
-   [SQLGetDescField](../../../relational-databases/native-client-odbc-api/sqlgetdescfield.md)  
  
-   [SQLSetDescField](../../../relational-databases/native-client-odbc-api/sqlsetdescfield.md)  
  
-   [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  

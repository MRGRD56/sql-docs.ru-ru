---
description: SQLDescribeCol
title: SQLDescribeCol | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDescribeCol function
ms.assetid: ffbf34c6-8268-434f-829a-82009a6cda59
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8b4962c3be438333c4791fc990cbaa5b3fd66569
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751404"
---
# <a name="sqldescribecol"></a>SQLDescribeCol
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Для выполненных инструкций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйверу ODBC для собственного клиента не требуется запрашивать сервер для описания столбцов в результирующем наборе. В этом случае **SQLDescribeCol** не вызывает обмен данными с сервером. Как и [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md)и [SQLNumResultCols](../../relational-databases/native-client-odbc-api/sqlnumresultcols.md), вызов **SQLDescribeCol** в подготовленных, но не выполненных инструкциях создает обмен данными между серверами.  
  
 Когда инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] или пакет инструкций возвращает несколько результирующих наборов строк, то столбец, на который ссылается исходный столбец, можно создать в отдельной таблице или сослаться на абсолютно другой столбец результирующего набора. **SQLDescribeCol** должен вызываться для каждого набора. При изменении результирующего набора приложение должно осуществить повторную привязку значений данных перед выборкой результатов строк. Дополнительные сведения об обработке запросов, возвращающих несколько результирующих наборов, см. в разделе [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md).  
  
 Когда несколько результирующих наборов формируется подготовленным пакетом инструкций SQL, атрибуты столбцов сообщаются только для первого результирующего набора.  
  
 Для типов данных больших значений значение, возвращаемое в *дататипептр* , SQL_VARCHAR, SQL_VARBINARY или SQL_NVARCHAR. Значение SQL_SS_LENGTH_UNLIMITED в *колумнсизептр* указывает, что размер не ограничен.  
  
 Улучшения в ядре СУБД, начиная с, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] позволяют SQLDescribeCol получать более точные описания ожидаемых результатов. Эти более точные результаты могут отличаться от значений, возвращаемых функцией SQLDescribeCol в предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Обнаружение метаданных](../../relational-databases/native-client/features/metadata-discovery.md).  
  
## <a name="sqldescribecol-support-for-enhanced-date-and-time-features"></a>Поддержка функцией SQLDescribeCol улучшенных возможностей работы с данными в формате даты-времени  
 Для типов даты-времени возвращаются следующие значения.  
  
| Атрибут | *дататипептр* | *колумнсизептр* | *деЦималдигитсптр* |  
| --------- | ------------- |---------------- | ------------------ |  
|DATETIME|SQL_TYPE_TIMESTAMP|23|3|  
|smalldatetime|SQL_TYPE_TIMESTAMP|16|0|  
|Дата|SQL_TYPE_DATE|10|0|  
|time|SQL_SS_TIME2|8, 10..16|0..7|  
|datetime2|SQL_TYPE_TIMESTAMP|19, 21..27|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|26, 28..34|0..7|  
  
 Дополнительные сведения см. в разделе [улучшения даты и времени &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqldescribecol-support-for-large-clr-udts"></a>Поддержка функцией SQLDescribeCol определяемых пользователем типов больших данных CLR  
 **SQLDescribeCol** поддерживает большие определяемые пользователем типы данных CLR (UDT). Дополнительные сведения см. в разделе [типы больших User-Defined CLR &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLDescribeCol](../../odbc/reference/syntax/sqldescribecol-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

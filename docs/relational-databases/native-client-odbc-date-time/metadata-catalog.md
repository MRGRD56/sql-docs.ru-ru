---
description: Метаданные — каталог
title: Метаданные каталога | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- metadata [ODBC]
- catalog metadata [ODBC]
ms.assetid: b82665be-8cb1-4ad3-ac15-2e590bdc1815
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 44c88fe888ac325393d87aa17074d27c4f8b02c0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756044"
---
# <a name="metadata---catalog"></a>Метаданные — каталог
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В этом разделе описываются метаданные столбца, возвращаемые **SQLColumns** и **SQLProcedureColumns**, а также метаданные типа данных, возвращаемые **SQLGetTypeInfo**.  
  
## <a name="remarks"></a>Remarks  
 Следующие значения столбца возвращаются для типов даты и времени с помощью **SQLColumns** и **SQLProcedureColumns**.  
  
|Тип параметра|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|--------------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|DATA_TYPE|SQL_TYPE_DATE|SQL_SS_TIME2|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_SS_TIMESTAMPOFFSET|  
|TYPE_NAME|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|COLUMN_SIZE|10|8, 10.16|16|23|19, 21..27|26, 28..34|  
|BUFFER_LENGTH|6|10|16|16|16|20|  
|DECIMAL_DIGITS|0|0..7|0|3|1.. 7|1.. 7|  
|SQL_DATA_TYPE|SQL_DATETIME|SQL_SS_TYPE_TIME2|SQL_DATETIME|SQL_DATETIME|SQL_DATETIME|SQL_SS_TYPE_TIMESTAMPOFFSET|  
|SQL_DATETIME_SUB|SQL_CODE_DATE|NULL|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|NULL|  
|CHAR_OCTET_LENGTH|NULL|NULL|NULL|NULL|NULL|NULL|  
|SS_DATA_TYPE|0|0|111|111|0|0|  
  
 Следующие значения столбца возвращаются для типов даты и времени с помощью **SQLGetTypeInfo**:  
  
|Тип параметра|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|--------------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|DATA_TYPE|SQL_TYPE_DATE|SQL_SS_TIME2|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_SS_TIMESTAMPOFFSET|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|'|'|'|'|'|'|  
|LITERAL_SUFFIX|'|'|'|'|'|'|  
|CREATE_PARAMS|NULL|scale|NULL|NULL|scale|scale|  
|NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|  
|CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|ДОСТУПНЫЙ ДЛЯ ПОИСКА|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|NULL|NULL|NULL|NULL|NULL|NULL|  
|FXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|AUTO_UNIQUE_VALUE|NULL|NULL|NULL|NULL|NULL|NULL|  
|LOCAL_TYPE_NAME|Дата|time|smalldatetime|DATETIME|datetime2|datetimeoffset|  
|MINIMUM_SCALE|0|0|0|3|0|0|  
|MAXIMUM_SCALE|0|7|0|3|7|7|  
|SQL_DATA_TYPE|SQL_DATETIME|SQL_SS_TIME2|SQL_DATETIME|SQL_DATETIME|SQL_DATETIME|SQL_SS_TYPE_TIMESTAMPOFFSET|  
|SQL_DATETIME_SUB|SQL_CODE_DATE|NULL|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|NULL|  
|NUM_PREC_RADIX|NULL|NULL|NULL|NULL|NULL|NULL|  
|INTERVAL_PRECISION|NULL|NULL|NULL|NULL|NULL|NULL|  
|USERTYPE|0|0|12|22|0|0|  
  
## <a name="see-also"></a>См. также:  
 [Метаданные &#40;ODBC&#41;](./date-and-time-improvements-odbc.md)  
  

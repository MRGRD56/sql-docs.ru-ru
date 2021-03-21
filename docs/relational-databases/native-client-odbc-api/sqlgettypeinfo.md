---
description: SQLGetTypeInfo
title: SQLGetTypeInfo | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetTypeInfo function
ms.assetid: 13b982c3-ae03-4155-bc0d-e225050703ce
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7d9fbf314deb253b76d33c2a597fa276de4e431
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749064"
---
# <a name="sqlgettypeinfo"></a>SQLGetTypeInfo
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента сообщает дополнительный столбец USERTYPE в результирующем наборе **SQLGetTypeInfo**. USERTYPE возвращает определение типа данных DB-Library. Этот столбец полезен разработчикам, которые переносят существующие приложения DB-Library в ODBC.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] рассматривает идентификатор как атрибут, а в ODBC он считается типом данных. Чтобы устранить это несоответствие, **SQLGetTypeInfo** возвращает типы данных: **интидентити**, **смаллинтидентити**, **тининтидентити**, **деЦималидентити** и **нумериЦидентити**. AUTO_UNIQUE_VALUE столбца результирующего набора **SQLGetTypeInfo** сообщает значение true для этих типов данных.  
  
 Для типа **varchar**, **nvarchar** и **varbinary** драйвер ODBC для собственного клиента по-своемуу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] отчету 8000, 4000 и 8000 соответственно для COLUMN_SIZE значения, хотя на самом деле он не ограничен. Это делается в целях обратной совместимости.  
  
 Для типа данных **XML** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента сообщает SQL_SS_LENGTH_UNLIMITED для COLUMN_SIZE, чтобы обозначить неограниченный размер.  
  
## <a name="sqlgettypeinfo-and-table-valued-parameters"></a>SQLGetTypeInfo и параметры, возвращающие табличные значения  
 Тип таблицы для возвращающих табличное значение параметров фактически является мета-типом, т. е. типом, используемым для определения других типов. Поэтому он не должен предоставляться через SQLGetTypeInfo. Для получения метаданных для табличных типов, используемых с возвращающими табличное значение параметрами, приложения должны использовать SQLTables, а не SQLGetTypeInfo.  
  
 Дополнительные сведения о получении метдата для возвращающих табличное значение параметров см. в разделе [атрибуты инструкций, влияющие на параметры Table-Valued](../../relational-databases/native-client-odbc-table-valued-parameters/statement-attributes-that-affect-table-valued-parameters.md).  
  
 Дополнительные сведения о возвращающих табличное значение параметрах см. в разделе [возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlgettypeinfo-support-for-enhanced-date-and-time-features"></a>Поддержка SQLGetTypeInfo для улучшенных функций даты-времени  
 Сведения о значениях, возвращаемых для типов даты-времени, см. в разделе [Catalog Metadata](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md).  
  
 Дополнительные общие сведения см. в разделе [улучшения даты и времени &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlgettypeinfo-support-for-large-clr-udts"></a>Поддержка SQLGetTypeInfo для больших определяемых пользователем типов данных среды CLR  
 **SQLGetTypeInfo** поддерживает большие определяемые пользователем типы данных CLR (UDT). Дополнительные сведения см. в разделе [типы больших User-Defined CLR &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLGetTypeInfo](../../odbc/reference/syntax/sqlgettypeinfo-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

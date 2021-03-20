---
description: SQLBindParameter
title: SQLBindParameter | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLBindParameter function
ms.assetid: c302c87a-e7f4-4d2b-a0a7-de42210174ac
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a2b6beb7697957cebcffb1f58c36a2902872ac35
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751414"
---
# <a name="sqlbindparameter"></a>SQLBindParameter
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLBindParameter** может устранять нагрузку при преобразовании данных при использовании для предоставления данных для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвера ODBC собственного клиента, что приводит к значительному повышению производительности для клиентских и серверных компонентов приложений. Другие преимущества — снижение потери точности при вставке или изменении приблизительных числовых типов данных.  
  
> [!NOTE]  
>  При вставке данных типа **char** и **WCHAR** в столбец Image используется размер передаваемых данных, в отличие от размера данных после преобразования в двоичный формат.  
  
 Если ODBC-драйвер для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сталкивается с ошибкой при обработке одного элемента в массиве параметров, драйвер продолжит выполнение инструкции для всех остальных элементов массива.  Если приложение выполнило привязку массива элементов параметров состояния, то строки параметров, которые вызвали ошибку, можно определить по массиву.  
  
 При использовании ODBC-драйвера для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] во время привязки входных параметров задавайте значение SQL_PARAM_INPUT. Значения SQL_PARAM_OUTPUT и SQL_PARAM_INPUT_OUTPUT следует задавать только для привязки параметров хранимой процедуры, для которых указано ключевое слово OUTPUT.  
  
 [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md) является ненадежным с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвером ODBC для собственного клиента, если элемент массива массива с привязанным параметром вызывает ошибку при выполнении инструкции. Атрибут инструкции ODBC SQL_ATTR_PARAMS_PROCESSED_PTR возвращает число строк, обработанных до возникновения ошибки. Затем при необходимости приложение может просмотреть массив состояний параметров, чтобы выяснить количество успешно выполненных инструкций.  
  
## <a name="binding-parameters-for-sql-character-types"></a>Привязка параметров для символьных типов SQL  
 Если переданный тип данных SQL имеет символьный тип, *ColumnSize* имеет размер в символах (а не байтах). Если длина строки данных в байтах превышает 8000, *ColumnSize* следует установить в значение **SQL_SS_LENGTH_UNLIMITED**, указывающее на отсутствие ограничения на размер типа SQL.  
  
 Например, если тип данных SQL — **SQL_WVARCHAR**, *ColumnSize* не должен превышать 4000. Если фактическая длина данных превышает 4000, то *ColumnSize* должен иметь значение **SQL_SS_LENGTH_UNLIMITED** , чтобы драйвер использовал **nvarchar (max)** .  
  
## <a name="sqlbindparameter-and-table-valued-parameters"></a>Параметр SQLBindParameter и возвращающие табличные значения параметры  
 Как и другие типы параметров, возвращающие табличные значения параметры связаны с помощью SQLBindParameter.  
  
 После привязки возвращающего табличное значение параметра его столбцы также оказываются привязанными. Чтобы привязать столбцы, вызовите [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) , чтобы задать SQL_SOPT_SS_PARAM_FOCUS порядковому номеру возвращающего табличное значение параметра. Затем вызовите SQLBindParameter для каждого столбца в возвращающем табличное значение параметре. Для возвращения к высокоуровневой привязке параметров задайте для SQL_SOPT_SS_PARAM_FOCUS значение 0.  
  
 Дополнительные сведения о сопоставлении параметров с полями дескрипторов для возвращающих табличное значение параметров см. в разделе [Binding и Передача данных Table-Valued Parameters и Values](../../relational-databases/native-client-odbc-table-valued-parameters/binding-and-data-transfer-of-table-valued-parameters-and-column-values.md).  
  
 Дополнительные сведения о возвращающих табличное значение параметрах см. в разделе [возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlbindparameter-support-for-enhanced-date-and-time-features"></a>Поддержка метода SQLBindParameter для улучшенных функций даты-времени  
 Значения параметров типов даты-времени преобразуются, как описано в статье [преобразования из C в SQL](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-c-to-sql.md). Обратите внимание, что параметры Type **time** и **DateTimeOffset** должны иметь значение *ValueType* , заданное как **SQL_C_DEFAULT** или **SQL_C_BINARY** , если используются соответствующие структуры (**SQL_SS_TIME2_STRUCT** и **SQL_SS_TIMESTAMPOFFSET_STRUCT**).  
  
 Дополнительные сведения см. в разделе [улучшения даты и времени &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlbindparameter-support-for-large-clr-udts"></a>Поддержка метода SQLBindParameter для больших определяемых пользователем типов в среде CLR  
 **SQLBindParameter** поддерживает большие определяемые пользователем типы данных CLR (UDT). Дополнительные сведения см. в разделе [типы больших User-Defined CLR &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Сведения о реализации API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [Функция SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md)  
  

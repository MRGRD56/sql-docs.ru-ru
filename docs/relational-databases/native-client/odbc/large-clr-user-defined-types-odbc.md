---
description: Определяемые пользователем типы данных больших значений CLR (ODBC)
title: Большие типы User-Defined CLR (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, large user-defined types
- large user-defined types [ODBC]
ms.assetid: ddce337e-bb6e-4a30-b7cc-4969bb1520a9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f1e55ddcd51072688d4544a41b415f42bb79fd58
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748350"
---
# <a name="large-clr-user-defined-types-odbc"></a>Определяемые пользователем типы данных больших значений CLR (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В этом разделе обсуждаются изменения ODBC в собственном клиенте SQL Server для поддержки определяемых пользователем типов данных CLR.  
  
 Пример, демонстрирующий поддержку ODBC для больших определяемых пользователем типов CLR, см. в разделе [Поддержка больших определяемых пользователем типов](../../../relational-databases/native-client-odbc-how-to/support-for-large-udts.md).  
  
 Дополнительные сведения о поддержке больших определяемых пользователем типов CLR в SQL Server Native Client см. в разделе [типы больших User-Defined CLR](../../../relational-databases/native-client/features/large-clr-user-defined-types.md).  
  
## <a name="data-format"></a>Формат данных  
 Собственный клиент SQL Server использует значение SQL_SS_LENGTH_UNLIMITED для обозначения того, что размер столбца больше чем 8000 байт для типов больших объектов. Начиная с SQL Server 2008, при размере столбца больше 8000 байт для определяемых пользователем типов данных больших значений CLR используется такое же значение.  
  
 Значения определяемых пользователем типов представляются в виде массивов байт. Поддерживается преобразование данных в шестнадцатеричные строки и из шестнадцатеричных строк. Литеральные значения представляются в виде шестнадцатеричных строк с префиксом «0x».  
  
 В следующей таблице показано сопоставление типов данных в параметрах и результирующих наборах.  
  
|Тип данных SQL Server|Тип данных SQL|Значение|  
|--------------------------|-------------------|-----------|  
|определяемый пользователем тип среды CLR|SQL_SS_UDT|-151 (sqlncli.h)|  
  
 В следующей таблице обсуждается соответствующая структура и тип ODBC C. По сути, определяемый пользователем тип данных CLR является типом **varbinary** с дополнительными метаданными.  
  
|Тип данных SQL|Организация памяти|Тип данных C|Значение (sqlext.h)|  
|-------------------|-------------------|-----------------|------------------------|  
|SQL_SS_UDT|SQLCHAR * (неподписанный знак \* )|SQL_C_BINARY|SQL_BINARY (-2)|  
  
## <a name="descriptor-fields-for-parameters"></a>Поля дескрипторов для параметров  
 Сведения, возвращаются в поля дескриптора параметра реализации (IPD) следующим образом.  
  
|Поле дескриптора|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|  
|----------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_LOCAL_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_SCALE|0|0|  
|SQL_DESC_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_UNSIGNED|SQL_TRUE|SQL_TRUE|  
|SQL_CA_SS_UDT_CATALOG_NAME|Имя каталога, содержащего определяемый пользователем тип.|Имя каталога, содержащего определяемый пользователем тип.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|Имя схемы, содержащей определяемый пользователем тип.|Имя схемы, содержащей определяемый пользователем тип.|  
|SQL_CA_SS_UDT_TYPE_NAME|Имя определяемого пользователем типа.|Имя определяемого пользователем типа.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|Полное имя определяемого пользователем типа.|Полное имя определяемого пользователем типа.|  
  
 Для параметров определяемого пользователем типа параметр SQL_CA_SS_UDT_TYPE_NAME всегда должен быть задан через **SQLSetDescField**. Значения SQL_CA_SS_UDT_CATALOG_NAME и SQL_CA_SS_UDT_SCHEMA_NAME не обязательны.  
  
 Если определяемый пользователем тип и таблица определяются в одной базе данных, но с разными схемами, необходимо установить SQL_CA_SS_UDT_SCHEMA_NAME.  
  
 Если определяемый пользователем тип и таблица определяются в разных базах данных, необходимо установить SQL_CA_SS_UDT_CATALOG_NAME и SQL_CA_SS_UDT_SCHEMA_NAME.  
  
 Если в установках для SQL_CA_SS_UDT_TYPE_NAME, SQL_CA_SS_UDT_CATALOG_NAME или SQL_CA_SS_UDT_SCHEMA_NAME существуют какие-либо ошибки или пропуски, то создается запись диагностики с кодом SQLSTATE HY000 и специфичный для сервера текст сообщения.  
  
## <a name="descriptor-fields-for-results"></a>Поля дескрипторов для результатов  
 Сведения, возвращаются в поля дескриптора строки реализации (IRD) следующим образом.  
  
|Поле дескриптора|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|  
|----------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_CONCISE_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|0|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_DISPLAY_SIZE|2 *n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|  
|SQL_DESC_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_LITERAL_PREFIX|"0x"|"0x"|  
|SQL_DESC_LITERAL_SUFFIX|""|""|  
|SQL_DESC_LOCAL_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_PRECISION|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SQL_DESC_SCALE|0|0|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|SQL_PRED_NONE|  
|SQL_DESC_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DESC_TYPE_NAME|"udt"|"udt"|  
|SQL_DESC_UNSIGNED|SQL_TRUE|SQL_TRUE|  
|SQL_CA_SS_UDT_CATALOG_NAME|Имя каталога, содержащего определяемый пользователем тип.|Имя каталога, содержащего определяемый пользователем тип.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|Имя схемы, содержащей определяемый пользователем тип.|Имя схемы, содержащей определяемый пользователем тип.|  
|SQL_CA_SS_UDT_TYPE_NAME|Имя определяемого пользователем типа.|Имя определяемого пользователем типа.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|Полное имя определяемого пользователем типа.|Полное имя определяемого пользователем типа.|  
  
## <a name="column-metadata-returned-by-sqlcolumns-and-sqlprocedurecolumns-catalog-metadata"></a>Метаданные столбца, возвращаемые функциями SQLColumns и SQLProcedureColumns (метаданные каталога)  
 Для определяемых пользователем типов возвращаются следующие значения столбца.  
  
|Имя столбца|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|  
|-----------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|DATA_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|TYPE_NAME|Имя определяемого пользователем типа.|Имя определяемого пользователем типа.|  
|COLUMN_SIZE|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|BUFFER_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|DECIMAL_DIGITS|NULL|NULL|  
|SQL_DATA_TYPE|SQL_SS_UDT|SQL_SS_UDT|  
|SQL_DATETIME_SUB|NULL|NULL|  
|CHAR_OCTET_LENGTH|*n*|SQL_SS_LENGTH_UNLIMITED (0)|  
|SS_UDT_CATALOG_NAME|Имя каталога, содержащего определяемый пользователем тип.|Имя каталога, содержащего определяемый пользователем тип.|  
|SS_UDT_SCHEMA_NAME|Имя схемы, содержащей определяемый пользователем тип.|Имя схемы, содержащей определяемый пользователем тип.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|Полное имя определяемого пользователем типа.|Полное имя определяемого пользователем типа.|  
  
 Три последних столбца зависят от драйвера. Они добавляются после любых столбцов, определенных ODBC, но перед любыми существующими для каждого драйвера столбцами результирующего набора SQLColumns или SQLProcedureColumns.  
  
 SQLGetTypeInfo не возвращает ни одной строки для отдельных определяемых пользователем типов или для универсального типа "UDT".  
  
## <a name="bindings-and-conversions"></a>Привязки и преобразования  
 Поддерживаются следующие преобразования типов данных SQL в типы данных C.  
  
|Прямое и обратное преобразование:|SQL_SS_UDT|  
|-----------------------------|------------------|  
|SQL_C_WCHAR|Поддерживается|  
|SQL_C_BINARY|Поддерживается|  
|SQL_C_CHAR|Поддерживается|  
  
 \* Двоичные данные преобразуются в шестнадцатеричную строку.  
  
 Поддерживаются следующие преобразования типов данных C в типы данных SQL.  
  
|Прямое и обратное преобразование:|SQL_SS_UDT|  
|-----------------------------|------------------|  
|SQL_C_WCHAR|Поддерживается|  
|SQL_C_BINARY|Поддерживается|  
|SQL_C_CHAR|Поддерживается|  
  
 \* Выполняется преобразование шестнадцатеричной строки в двоичные данные.  
  
## <a name="sql_variant-support-for-udts"></a>Поддержка SQL_VARIANT для определяемых пользователем типов  
 Определяемые пользователем типы не поддерживаются в столбцах SQL_VARIANT.  
  
## <a name="bcp-support-for-udts"></a>Поддержка программы bcp для определяемых пользователем типов  
 Значения определяемых пользователем типов можно импортировать или экспортировать только в виде символьных или двоичных значений.  
  
## <a name="downlevel-client-behavior-for-udts"></a>Поведение клиента нижнего уровня для определяемых пользователем типов  
 Определяемые пользователем типы проходят сопоставление типов с клиентами низкого уровня, как показано далее.  
  
|Версия сервера|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|  
|--------------------|-------------------------------------------------------------------|----------------------------------------------------------|  
|SQL Server 2005|**UDT**|**varbinary(max)**|  
|SQL Server 2008 и более поздние версии|**UDT**|**UDT**|  
  
## <a name="odbc-functions-supporting-large-clr-udts"></a>Функции ODBC, поддерживающие определяемые пользователем типы больших данных CLR  
 В этом разделе обсуждаются изменения в функциях ODBC собственного клиента SQL Server, касающиеся поддержки определяемых пользователем типов больших данных CLR.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 Значения столбцов результатов определяемого пользователем типа преобразуются из типов SQL в C, как описано в разделе "привязки и преобразования" ранее в этом разделе.  
  
### <a name="sqlbindparameter"></a>SQLBindParameter  
 Для определяемых пользователем типов необходимы следующие значения.  
  
|Тип данных SQL|*ParameterType*|*колумнсизептр*|*деЦималдигитсптр*|  
|-------------------|---------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Значения, возвращаемые для определяемых пользователем типов, описаны в подразделе «Поля дескрипторов для результатов» ранее в этом разделе.  
  
### <a name="sqlcolumns"></a>SQLColumns  
 Значения, возвращаемые для определяемых пользователем типов, описаны в подразделе «Метаданные столбца, возвращаемые функциями SQLColumns и SQLProcedureColumns (метаданные каталога)» ранее в этом разделе.  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Для определяемых пользователем типов возвращаются следующие значения.  
  
|Тип данных SQL|*дататипептр*|*колумнсизептр*|*деЦималдигитсптр*|  
|-------------------|-------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqldescribeparam"></a>SQLDescribeParam  
 Для определяемых пользователем типов возвращаются следующие значения.  
  
|Тип данных SQL|*дататипептр*|*колумнсизептр*|*деЦималдигитсптр*|  
|-------------------|-------------------|---------------------|------------------------|  
|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT|*n*|0|  
|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|SQL_SS_UDT|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlfetch"></a>SQLFetch  
 Значения столбцов результатов определяемого пользователем типа преобразуются из типов SQL в C, как описано в разделе "привязки и преобразования" ранее в этом разделе.  
  
### <a name="sqlfetchscroll"></a>SQLFetchScroll  
 Значения столбцов результатов определяемого пользователем типа преобразуются из типов SQL в C, как описано в разделе "привязки и преобразования" ранее в этом разделе.  
  
### <a name="sqlgetdata"></a>SQLGetData  
 Значения столбцов результатов определяемого пользователем типа преобразуются из типов SQL в C, как описано в разделе "привязки и преобразования" ранее в этом разделе.  
  
### <a name="sqlgetdescfield"></a>SQLGetDescField  
 Поля дескрипторов, доступные с новыми типами, описаны в подразделах «Поля дескрипторов для параметров» и «Поля дескрипторов для результатов» ранее в этом разделе.  
  
### <a name="sqlgetdescrec"></a>SQLGetDescRec  
 Для определяемых пользователем типов возвращаются следующие значения.  
  
|Тип данных SQL|Тип|Подтип|Длина|Точность|Масштабирование|  
|-------------------|----------|-------------|------------|---------------|-----------|  
|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT|0|*n*|n|0|  
|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|SQL_SS_UDT|0|SQL_SS_LENGTH_UNLIMITED (0)|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlgettypeinfo"></a>SQLGetTypeInfo  
 Значения, возвращаемые для определяемых пользователем типов, описаны в подразделе «Метаданные, возвращаемые функциями SQLColumns и SQLProcedureColumns (метаданные каталога)» ранее в этом разделе.  
  
### <a name="sqlprocedurecolumns"></a>SQLProcedureColumns  
 Значения, возвращаемые для определяемых пользователем типов, описаны в подразделе «Метаданные, возвращаемые функциями SQLColumns и SQLProcedureColumns (метаданные каталога)» ранее в этом разделе.  
  
### <a name="sqlputdata"></a>SQLPutData  
 Значения параметров определяемого пользователем типа преобразуются из C в типы SQL, как описано в разделе "привязки и преобразования" ранее в этом разделе.  
  
### <a name="sqlsetdescfield"></a>SQLSetDescField  
 Поля дескрипторов, доступные с новыми типами, описаны в подразделах «Поля дескрипторов для параметров» и «Поля дескрипторов для результатов» ранее в этом разделе.  
  
### <a name="sqlsetdescrec"></a>SQLSetDescRec  
 Для определяемых пользователем типов разрешены следующие значения.  
  
|Тип данных SQL|Тип|Подтип|Длина|Точность|Масштабирование|  
|-------------------|----------|-------------|------------|---------------|-----------|  
|SQL_SS_UDT<br /><br /> (длина не более 8 000 байт)|SQL_SS_UDT|0|*n*|*n*|0|  
|SQL_SS_UDT<br /><br /> (длина более 8 000 байт)|SQL_SS_UDT|0|SQL_SS_LENGTH_UNLIMITED (0)|SQL_SS_LENGTH_UNLIMITED (0)|0|  
  
### <a name="sqlspecialcolumns"></a>SQLSpecialColumns  
 Значения, возвращаемые для столбцов DATA_TYPE, TYPE_NAME, COLUMN_SIZE, BUFFER_LENGTH и DECIMAL_DIGTS определяемых пользователем типов, описаны в подразделе «Метаданные, возвращаемые функциями SQLColumns и SQLProcedureColumns (метаданные каталога)» ранее в этом разделе.  
  
## <a name="see-also"></a>См. также:  
 [Большие определяемые пользователем типы данных CLR](../../../relational-databases/native-client/features/large-clr-user-defined-types.md)  
  
  

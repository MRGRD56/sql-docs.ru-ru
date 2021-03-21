---
description: SQLColAttribute
title: SQLColAttribute | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLColAttribute function
ms.assetid: a5387d9e-a243-4cfe-b786-7fad5842b1d6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4d62e13e7bed632ffed99ef0eab8ddf5cbb02a59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755094"
---
# <a name="sqlcolattribute"></a>SQLColAttribute
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLColAttribute** можно использовать для получения атрибута столбца результирующего набора для подготовленных или выполненных инструкций ODBC. Вызов **SQLColAttribute** для подготовленных инструкций вызывает обмен данными с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента получает данные столбца результирующего набора в ходе выполнения инструкции, поэтому вызов **SQLColAttribute** после завершения **SQLExecute** или **SQLExecDirect** не требует циклического обмена данными с сервером.  
  
> [!NOTE]  
>  Атрибуты ODBC идентификаторов столбца доступны не во всех результирующих наборах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Идентификатор поля|Описание|  
|----------------------|-----------------|  
|SQL_COLUMN_TABLE_NAME|Можно использовать для результирующих наборов, полученных от инструкций, которые формируют серверные курсоры, или для выполненных инструкций SELECT, содержащих предложение FOR BROWSE.|  
|SQL_DESC_BASE_COLUMN_NAME|Можно использовать для результирующих наборов, полученных от инструкций, которые формируют серверные курсоры, или для выполненных инструкций SELECT, содержащих предложение FOR BROWSE.|  
|SQL_DESC_BASE_TABLE_NAME|Можно использовать для результирующих наборов, полученных от инструкций, которые формируют серверные курсоры, или для выполненных инструкций SELECT, содержащих предложение FOR BROWSE.|  
|SQL_DESC_CATALOG_NAME|имя базы данных. Можно использовать для результирующих наборов, полученных от инструкций, которые формируют серверные курсоры, или для выполненных инструкций SELECT, содержащих предложение FOR BROWSE.|  
|SQL_DESC_LABEL|Можно использовать для всех результирующих наборов. Значение идентично значению поля SQL_DESC_NAME.<br /><br /> Длина поля равна нулю только в том случае, если столбец является результатом выражения, не содержащего назначения метки.|  
|SQL_DESC_NAME|Можно использовать для всех результирующих наборов. Значение идентично значению поля SQL_DESC_LABEL.<br /><br /> Длина поля равна нулю только в том случае, если столбец является результатом выражения, не содержащего назначения метки.|  
|SQL_DESC_SCHEMA_NAME|Имя владельца. Можно использовать для результирующих наборов, полученных от инструкций, которые формируют серверные курсоры, или для выполненных инструкций SELECT, содержащих предложение FOR BROWSE.<br /><br /> Доступно, только если имя владельца задано для столбца в инструкции SELECT.|  
|SQL_DESC_TABLE_NAME|Можно использовать для результирующих наборов, полученных от инструкций, которые формируют серверные курсоры, или для выполненных инструкций SELECT, содержащих предложение FOR BROWSE.|  
|SQL_DESC_UNNAMED|SQL_NAMED для всех столбцов в результирующем наборе, за исключением случаев, когда столбец является результатом выражения, не содержащего назначения метки в составе выражения. Если SQL_DESC_UNNAMED возвращает SQL_UNNAMED, все атрибуты ODBC идентификаторов столбцов содержат строки нулевой длины для этого столбца.|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Драйвер ODBC для собственного клиента использует инструкцию SET FMTONLY, чтобы уменьшить нагрузку на сервер при вызове **SQLColAttribute** для подготовленных, но невыполненных инструкций.  
  
 Для типов больших значений **SQLColAttribute** будет возвращать следующие значения:  
  
|Идентификатор поля|Описание изменения|  
|----------------------|---------------------------|  
|SQL_DESC_DISPLAY_SIZE|Максимальное число символов, необходимых для отображения данных столбца. Для столбцов типов больших значений возвращается значение SQL_SS_LENGTH_UNLIMITED.|  
|SQL_DESC_LENGTH|Возвращает в результирующем наборе фактическую длину столбца. Для столбцов типов больших значений возвращается значение SQL_SS_LENGTH_UNLIMITED.|  
|SQL_DESC_OCTET_LENGTH|Возвращает максимальную длину столбца с типом больших значений. SQL_SS_LENGTH_UNLIMITED используется для указания неограниченного размера.|  
|SQL_DESC_PRECISION|Возвращает значение SQL_SS_LENGTH_UNLIMITED для столбцов с типом больших значений.|  
|SQL_DESC_TYPE|Возвращает SQL_VARCHAR, SQL_WVARCHAR и SQL_VARBINARY для типов больших значений.|  
|SQL_DESC_TYPE_NAME|Возвращает типы данных varchar, varbinary и nvarchar для типов больших значений.|  
  
 Для всех версий атрибуты столбцов возвращаются только для первого результирующего набора, если несколько результирующих наборов формируются готовым пакетом инструкций SQL.  
  
 Следующие атрибуты столбцов являются расширениями, предоставляемыми [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвером ODBC для собственного клиента. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента возвращает все значения в параметре *нумерикаттрптр* . Возвращаются значения с типом SDWORD (signed long) за исключением SQL_CA_SS_COMPUTE_BYLIST, которое представляет собой указатель на массив WORD.  
  
|Идентификатор поля|Возвращенное значение|  
|----------------------|--------------------|  
|SQL_CA_SS_COLUMN_HIDDEN*|Значение TRUE, если столбец является частью скрытого первичного ключа, созданного для поддержки инструкции Transact-SQL SELECT, содержащей FOR BROWSE.|  
|SQL_CA_SS_COLUMN_ID|Порядковый номер результирующего столбца предложения COMPUTE в текущей инструкции Transact-SQL SELECT.|  
|SQL_CA_SS_COLUMN_KEY*|Значение TRUE, если столбец является частью первичного ключа строки, а инструкция Transact-SQL SELECT содержит FOR BROWSE.|  
|SQL_CA_SS_COLUMN_OP|Целочисленное значение, задающее статистический оператор, ответственный за значение в столбце предложения COMPUTE. Определения целочисленных значений находятся в файле sqlncli.h.|  
|SQL_CA_SS_COLUMN_ORDER|Порядковый номер столбца в предложении ORDER BY инструкции ODBC или Transact-SQL SELECT.|  
|SQL_CA_SS_COLUMN_SIZE|Максимальная длина в байтах, необходимая для привязки значения данных, полученных из столбца, к переменной SQL_C_BINARY.|  
|SQL_CA_SS_COLUMN_SSTYPE|Собственный тип данных, хранящихся в столбце SQL Server. Определения значений типов находятся в файле sqlncli.h.|  
|SQL_CA_SS_COLUMN_UTYPE|Базовый тип данных определяемого пользователем типа данных столбца SQL Server. Определения значений типов находятся в файле sqlncli.h.|  
|SQL_CA_SS_COLUMN_VARYLEN|Значение TRUE, если длина данных столбца может быть разной, в противном случае FALSE.|  
|SQL_CA_SS_COMPUTE_BYLIST|Указатель на массив WORD (unsigned short), задающий столбцы, используемые во фразе BY предложения COMPUTE. Если в предложении COMPUTE не задана фраза BY, возвращается указатель NULL.<br /><br /> Первый элемент массива содержит счетчик столбцов BY. Дополнительные элементы являются порядковыми номерами столбцов.|  
|SQL_CA_SS_COMPUTE_ID|*компутеид* строки, которая является результатом предложения COMPUTE в текущей инструкции TRANSACT-SQL SELECT.|  
|SQL_CA_SS_NUM_COMPUTES|Количество предложений COMPUTE, заданных в текущей инструкции Transact-SQL SELECT.|  
|SQL_CA_SS_NUM_ORDERS|Количество столбцов в предложении ORDER BY инструкции ODBC или Transact-SQL SELECT.|  
  
 \*   Доступный атрибут инструкции If SQL_SOPT_SS_HIDDEN_COLUMNS имеет значение SQL_HC_ON.  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Добавлены связанные с драйвером поля дескриптора для предоставления дополнительных сведений для обозначения имени коллекции XML-схем, имени схемы и имени каталога соответственно. При наличии в этих свойствах неалфавитных символов использование кавычек или escape-символа не требуется. Эти новые поля дескриптора приведены в следующей таблице:  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_CATALOG_NAME|CharacterAttributePtr|Имя каталога, в котором определено имя коллекции схем XML. Если обнаружить имя каталога невозможно, то эта переменная содержит пустую строку.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_SS_XML_SCHEMACOLLECTION_CATALOG_NAME IRD, предназначенного для чтения и записи.|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_SCHEMA_NAM E|CharacterAttributePtr|Имя схемы, в которой определено имя коллекции схем XML. Если обнаружить имя схемы невозможно, то эта переменная содержит пустую строку.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_SS_XML_SCHEMACOLLECTION_SCHEMA_NAME IRD, предназначенного для чтения и записи.|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_NAME|CharacterAttributePtr|Имя коллекции схем XML. Если обнаружить имя невозможно, то эта переменная содержит пустую строку.<br /><br /> Эти сведения возвращаются из поля записи SQL_DESC_SS_XML_SCHEMACOLLECTION_NAME IRD, предназначенного для чтения и записи.|  
  
 Кроме того, в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] представлены новые поля дескриптора для получения дополнительных сведений для столбца результирующего набора с определяемым пользователем типом данных (UDT) и для параметра UDT хранимой процедуры или параметризированного запроса. При наличии в этих свойствах неалфавитных символов использование кавычек или escape-символа не требуется. Эти новые поля дескриптора приведены в следующей таблице:  
  
|Имя столбца|Тип|Описание|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_UDT_CATALOG_NAME|CharacterAttributePtr|Имя каталога, содержащего определяемый пользователем тип.|  
|SQL_CA_SS_UDT_SCHEMA_NAME|CharacterAttributePtr|Имя схемы, содержащей определяемый пользователем тип.|  
|SQL_CA_SS_UDT_TYPE_NAME|CharacterAttributePtr|Имя определяемого пользователем типа.|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|CharacterAttributePtr|Полное имя сборки определяемого пользователем типа.|  
  
 Идентификатор существующего дескриптора поля SQL_DESC_TYPE_NAME используется для указания имени определяемого пользователем типа данных. Поле SQL_DESC_TYPE для столбца определяемого пользователем типа содержит значение SQL_SS_UDT.  
  
## <a name="sqlcolattribute-support-for-enhanced-date-and-time-features"></a>Поддержка SQLColAttribute для усовершенствованных функций даты-времени  
 Значения, возвращаемые для типов даты и времени, см. в подразделе «сведения, возвращаемые в полях IRD» раздела [метаданные параметров и результатов](../../relational-databases/native-client-odbc-date-time/metadata-parameter-and-result.md).  
  
 Дополнительные сведения см. в разделе [улучшения даты и времени &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlcolattribute-support-for-large-clr-udts"></a>Поддержка SQLColAttribute для больших определяемых пользователем типов CLR  
 **SQLColAttribute** поддерживает большие определяемые пользователем типы данных CLR (UDT). Дополнительные сведения см. в разделе [типы больших User-Defined CLR &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlcolattribute-support-for-sparse-columns"></a>Поддержка SQLColAttribute для разреженных столбцов  
 SQLColAttribute запрашивает новое поле дескриптора строки реализации (IRD), SQL_CA_SS_IS_COLUMN_SET, чтобы определить, является ли столбец **column_setным** столбцом.  
  
 Дополнительные сведения см. в разделе [Поддержка разреженных столбцов &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLColAttribute](../../odbc/reference/syntax/sqlcolattribute-function.md)   
 [Сведения о реализации API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)  
  

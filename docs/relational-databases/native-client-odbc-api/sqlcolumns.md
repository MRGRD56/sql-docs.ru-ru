---
description: SQLColumns
title: SQLColumns | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLColumns function
ms.assetid: 69d3af44-8196-43ab-8037-cdd06207b171
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 086904d3456ba5160813c731ca84db249692b441
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755064"
---
# <a name="sqlcolumns"></a>SQLColumns
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLColumns** возвращает значение, SQL_SUCCESS, существуют ли значения для параметров *CatalogName*, *TableName* или *ColumnName* . Функция **SQLFetch** возвращает значение SQL_NO_DATA, если в этих параметрах заданы недопустимые значения.  
  
> [!NOTE]  
>  Для типов больших значений все параметры длины будут возвращены со значением SQL_SS_LENGTH_UNLIMITED.  
  
 **SQLColumns** может выполняться на статическом серверном курсоре. Попытка выполнить **SQLColumns** для обновляемого (динамического или ключевого набора ключей) курсора возвратит SQL_SUCCESS_WITH_INFO, указывающее, что тип курсора был изменен.  
  
 Драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает выдачу сведений о таблицах, находящихся на связанных серверах, принимая двухкомпонентное имя в параметре *CatalogName* : *Имя_Связанного_Сервера.Имя_Каталога*.  
  
 Для ODBC 2. приложения *x* , не использующие подстановочные знаки в *TableName*, **SQLColumns** возвращает сведения о таблицах, имена которых совпадают с *TableName* и принадлежат текущему пользователю. Если текущий пользователь не владеет таблицей, имя которой соответствует параметру *TableName* , **SQLColumns** возвращает сведения о любых таблицах, принадлежащих другим пользователям, где имя таблицы совпадает с параметром *TableName* . Для ODBC 2. приложения *x* , использующие подстановочные знаки, **SQLColumns** возвращает все таблицы, имена которых совпадают с *TableName*. Для ODBC 3. *x* Applications **SQLColumns** возвращает все таблицы, имена которых совпадают с *TableName* , независимо от владельца или от того, используются ли подстановочные знаки.  
  
 В следующей таблице перечислены столбцы, возвращаемые в результирующем наборе.  
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|DATA_TYPE|Возвращает SQL_VARCHAR, SQL_VARBINARY или SQL_WVARCHAR для типов данных **varchar (max)** .|  
|TYPE_NAME|Возвращает "varchar", "varbinary" или "nvarchar" для типов данных **varchar (max)**, **varbinary (max)** и **nvarchar (max)** .|  
|COLUMN_SIZE|Возвращает SQL_SS_LENGTH_UNLIMITED для типов данных **varchar (max)** , указывающих на то, что размер столбца не ограничен.|  
|BUFFER_LENGTH|Возвращает SQL_SS_LENGTH_UNLIMITED для типов данных **varchar (max)** , указывающих, что размер буфера не ограничен.|  
|SQL_DATA_TYPE|Возвращает SQL_VARCHAR, SQL_VARBINARY или SQL_WVARCHAR для типов данных **varchar (max)** .|  
|CHAR_OCTET_LENGTH|Возвращает максимальную длину символьного или двоичного столбца. Возвращает значение 0, если размер не ограничен.|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|Возвращает имя каталога, в котором определено имя коллекции схем XML. Если обнаружить имя каталога невозможно, то эта переменная содержит пустую строку.|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|Возвращает имя схемы, в которой определено имя коллекции схем XML. Если обнаружить имя схемы невозможно, то эта переменная содержит пустую строку.|  
|SS_XML_SCHEMACOLLECTION_NAME|Возвращает имя коллекции схем XML. Если обнаружить имя невозможно, то эта переменная содержит пустую строку.|  
|SS_UDT_CATALOG_NAME|Имя каталога, содержащего определяемый пользователем тип.|  
|SS_UDT_SCHEMA_NAME|Имя схемы, содержащей определяемый пользователем тип.|  
|SS_UDT_ASSEMBLY_TYPE_NAME|Имя сборки определяемого пользователем типа.|  
  
 Для определяемых пользователем типов существующий столбец TYPE_NAME используется для указания имени определяемого пользователем типа; Поэтому для него не следует добавлять дополнительный столбец в результирующий набор **SQLColumns** или [SQLProcedureColumns](../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md). DATA_TYPE для столбца или параметра определяемого пользователем типа имеет значение SQL_SS_UDT.  
  
 Для параметров определяемого пользователем типа можно использовать новые, зависящие от драйвера дескрипторы, определенные выше, для получения или задания дополнительных свойств метаданных определяемого пользователем типа, если сервер должен вернуть или запросить данные сведения.  
  
 Когда клиент подключается к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и вызывает SQLColumns, использование значений NULL или подстановочных знаков для входного параметра каталога не приведет к возврату сведений из других каталогов. Вместо этого будут возвращены сведения только о текущем каталоге. Клиент может сначала вызвать SQLTables, чтобы определить, в каком каталоге находится нужная таблица. Затем клиент может использовать это значение каталога для входного параметра каталога в его вызове SQLColumns для получения сведений о столбцах в этой таблице.  
  
## <a name="sqlcolumns-and-table-valued-parameters"></a>Функция SQLColumns и возвращающие табличное значение параметры  
 Результирующий набор, возвращаемый функцией SQLColumns, зависит от значения параметра SQL_SOPT_SS_NAME_SCOPE. Дополнительные сведения см. в разделе [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). Следующие столбцы добавлены для возвращающих табличное значение параметров.  
  
|Имя столбца|Тип данных|Содержимое|  
|-----------------|---------------|--------------|  
|SS_IS_COMPUTED|Smallint|Если столбец является вычисляемым, то для него в TABLE_TYPE это значение равно SQL_TRUE. В противном случае — SQL_FALSE.|  
|SS_IS_IDENTITY|Smallint|SQL_TRUE, если столбец является столбцом идентификаторов. В противном случае — SQL_FALSE.|  
  
 Дополнительные сведения о возвращающих табличное значение параметрах см. в разделе [возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlcolumns-support-for-enhanced-date-and-time-features"></a>Поддержка функцией SQLColumns улучшенных возможностей работы с датой и временем  
 Сведения о значениях, возвращаемых для типов даты и времени, см. в разделе [метаданные каталога](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md).  
  
 Дополнительные сведения см. в разделе [улучшения даты и времени &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlcolumns-support-for-large-clr-udts"></a>Поддержка функцией SQLColumns определяемых пользователем типов больших данных CLR  
 **SQLColumns** поддерживает большие определяемые пользователем типы данных CLR (UDT). Дополнительные сведения см. в разделе [типы больших User-Defined CLR &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlcolumns-support-for-sparse-columns"></a>Поддержка функцией SQLColumns разреженных столбцов  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]В результирующий набор для SQLColumns были добавлены два указанных столбца:  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|SS_IS_SPARSE|**Smallint**|Если столбец является разреженным, то значение равно SQL_TRUE. В противном случае — SQL_FALSE.|  
|SS_IS_COLUMN_SET|**Smallint**|Если столбец является столбцом **column_set** , это SQL_TRUE. в противном случае SQL_FALSE.|  
  
 В соответствии со спецификацией ODBC SS_IS_SPARSE и SS_IS_COLUMN_SET отображаются перед всеми столбцами, которые были добавлены в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] более ранние версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , чем, и после всех столбцов, заданные самим ODBC.  
  
 Результирующий набор, возвращаемый функцией SQLColumns, зависит от значения параметра SQL_SOPT_SS_NAME_SCOPE. Дополнительные сведения см. в разделе [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Дополнительные сведения о разреженных столбцах в ODBC см. в разделе [Поддержка разреженных столбцов &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLColumns](../../odbc/reference/syntax/sqlcolumns-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

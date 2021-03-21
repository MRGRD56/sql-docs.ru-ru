---
description: SQLSetStmtAttr
title: SQLSetStmtAttr | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetStmtAttr function
ms.assetid: 799c80fd-c561-4912-8562-9229076dfd19
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9005450f9dcb700a08b70820e75ddc8d3f83d90
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754864"
---
# <a name="sqlsetstmtattr"></a>SQLSetStmtAttr
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Драйвер ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает смешанную (динамическую и с набором ключей) модель курсора. Попытки установить размер набора ключей с помощью атрибута SQL_ATTR_KEYSET_SIZE завершатся неудачей, если задаваемое значение не равно 0.  
  
 Приложение задает SQL_ATTR_ROW_ARRAY_SIZE для всех инструкций, чтобы объявить число строк, возвращаемых при вызове функции **SQLFetch** или [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) . Для инструкций, задающих серверный курсор, драйвер использует атрибут SQL_ATTR_ROW_ARRAY_SIZE, чтобы задать размер блока строк, которые создаются на сервере в ответ на запрос на выборку со стороны курсора. В пределах размера блока динамического курсора членство в строке и порядок не меняются, если уровень изоляции транзакции достаточен для обеспечения повторяемого чтения фиксированных транзакций. Вне блока, задаваемого этой величиной, курсор полностью динамичен. Размер блока серверного курсора полностью динамичен и может быть изменен в любой момент в процессе обработки выборки.  
  
## <a name="sqlsetstmtattr-and-table-valued-parameters"></a>Функция SQLSetStmtAttr и возвращающие табличные значения параметры  
 SQLSetStmtAttr можно использовать для установки SQL_SOPT_SS_PARAM_FOCUS в дескрипторе параметра приложения (APD) перед доступом к полям дескриптора для столбцов возвращающего табличное значение параметра.  
  
 Если предпринимается попытка задать SQL_SOPT_SS_PARAM_FOCUS порядковому номеру параметра, который не является возвращающим табличное значение параметром, SQLSetStmtAttr возвращает SQL_ERROR и запись диагностики, созданную с помощью SQLSTATE = HY024, и сообщение "Недопустимый атрибут value". Если возвращается значение SQL_ERROR, то атрибут SQL_SOPT_SS_PARAM_FOCUS не меняется.  
  
 Установка атрибута SQL_SOPT_SS_PARAM_FOCUS в значение 0 восстанавливает доступ к записям дескриптора для параметров.  
  
 SQLSetStmtAttr также можно использовать для установки SQL_SOPT_SS_NAME_SCOPE. Дополнительные сведения см. в подразделе «SQL_SOPT_SS_NAME_SCOPE» далее в этом разделе.  
  
 Дополнительные сведения см. в разделе [метаданные возвращающего табличное значение параметра для подготовленных инструкций](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameter-metadata-for-prepared-statements.md).  
  
 Дополнительные сведения о возвращающих табличное значение параметрах см. в разделе [возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlsetstmtattr-support-for-sparse-columns"></a>Поддержка разреженных столбцов функцией SQLSetStmtAttr  
 SQLSetStmtAttr можно использовать для установки SQL_SOPT_SS_NAME_SCOPE. Дополнительные сведения см. в подразделе SQL_SOPT_SS_NAME_SCOPE далее в этом разделе. Дополнительные сведения о разреженных столбцах см. в разделе [Поддержка разреженных столбцов &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="statement-attributes"></a>Атрибуты инструкции  
 Драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает следующие атрибуты инструкций, специфичные для драйвера.  
  
### <a name="sql_sopt_ss_cursor_options"></a>SQL_SOPT_SS_CURSOR_OPTIONS  
 Атрибут SQL_SOPT_SS_CURSOR указывает, будет ли драйвер использовать для курсоров специфичные для данного драйвера параметры настройки производительности. [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) не допускается, если заданы эти параметры. Значение по умолчанию равно SQL_CO_OFF. Значение аргумента *ValuePtr* имеет тип SQLLEN.  
  
|Значение *ValuePtr*|Описание|  
|----------------------|-----------------|  
|SQL_CO_OFF|По умолчанию. Отключает быстрые однопроходные курсоры, только для чтения и автовыборки, включает **SQLGetData** для однопроходных курсоров только для чтения. Если атрибут SQL_SOPT_SS_CURSOR_OPTIONS имеет значение SQL_CO_OFF, тип курсора не изменится. То есть быстрый однопроходный курсор останется быстрым однопроходным курсором. Чтобы изменить тип курсора, приложение должно установить другой тип курсора с помощью **SQLSetStmtAttr**/SQL_ATTR_CURSOR_TYPE.|  
|SQL_CO_FFO|Включает быстрые однопроходные курсоры только для чтения, отключает **SQLGetData** на однопроходных курсорах только для чтения.|  
|SQL_CO_AF|Включает параметр автоматической выборки для курсора любого типа. Если этот параметр задан для обработчика инструкции, **SQLExecute** или **SQLExecDirect** создаст неявный **SQLFetchScroll** (SQL_FIRST). Курсор открыт, и первый набор строк возвращен за одно обращение к серверу.|  
|SQL_CO_FFO_AF|Включает быстрые однопроходные курсоры с параметром автоматической выборки. Эквивалентно одновременному заданию SQL_CO_AF и SQL_CO_FFO.|  
  
 Если эти параметры заданы, сервер автоматически закрывает курсор, когда обнаруживает, что была получена последняя строка. Приложение по-прежнему должно вызывать [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) (SQL_CLOSE) или [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md), но драйверу не нужно отсылать уведомление о закрытии на сервер.  
  
 Если список выбора содержит столбец **Text**, **ntext** или **Image** , то однопроходный курсор преобразуется в динамический курсор, а **SQLGetData** — разрешено.  
  
### <a name="sql_sopt_ss_defer_prepare"></a>SQL_SOPT_SS_DEFER_PREPARE  
 Атрибут SQL_SOPT_SS_DEFER_PREPARE определяет, была ли инструкция подготовлена немедленно или отложена до выполнения **SQLExecute**, [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) или [SQLDescribeParam](../../relational-databases/native-client-odbc-api/sqldescribeparam.md) . В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 и более ранних версиях это свойство не учитывается (подготовка не откладывается). Значение аргумента *ValuePtr* имеет тип SQLLEN.  
  
|Значение *ValuePtr*|Описание|  
|----------------------|-----------------|  
|SQL_DP_ON|По умолчанию. После вызова [функции SQLPrepare](../../odbc/reference/syntax/sqlprepare-function.md)подготовка инструкции откладывается до вызова **SQLExecute** или выполнения операции метасвойства (**SQLDescribeCol** или **SQLDescribeParam**).|  
|SQL_DP_OFF|Инструкция подготовлена сразу после выполнения **SQLPrepare** .|  
  
### <a name="sql_sopt_ss_regionalize"></a>SQL_SOPT_SS_REGIONALIZE  
 Атрибут SQL_SOPT_SS_REGIONALIZE используется для задания преобразования данных на уровне инструкции. Атрибут указывает драйверу, что следует использовать настройки локали на клиенте при преобразовании данных в денежном формате, формате даты и времени в символьные данные. Преобразование проводится только из собственных типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в строки символов.  
  
 Значение аргумента *ValuePtr* имеет тип SQLLEN.  
  
|Значение *ValuePtr*|Описание|  
|----------------------|-----------------|  
|SQL_RE_OFF|По умолчанию. Драйвер ODBC не использует настройки локали на клиенте при преобразовании данных в денежном формате, формате даты и времени в строковые данные.|  
|SQL_RE_ON|Драйвер ODBC использует настройки локали на клиенте при преобразовании данных в денежном формате, формате даты и времени в символьные данные.|  
  
 Региональные параметры преобразований применяются к типам данных валюты, чисел и даты-времени. Параметры преобразований применяются только к выходным преобразованиям, когда значения валюты, числовые значения, значения даты или времени преобразуются в символьные строки.  
  
> [!NOTE]  
>  Когда атрибут инструкции SQL_SOPT_SS_REGIONALIZE включен, драйвер использует настройки локали из реестра текущего пользователя. Драйвер не учитывает языковой стандарт текущего потока, если приложение устанавливает его, например, путем вызова **сетсреадлокале**.  
  
 Изменение режима работы источника данных с региональными параметрами может привести к ошибке приложения. Изменение этого значения может отрицательно повлиять на работу приложения, анализирующего строки даты и предполагающего, что строки даты будут выводиться в виде, определенном ODBC.  
  
### <a name="sql_sopt_ss_textptr_logging"></a>SQL_SOPT_SS_TEXTPTR_LOGGING  
 Атрибут SQL_SOPT_SS_TEXTPTR_LOGGING переключает ведение журнала операций над столбцами, содержащими **текстовые** или **графические** данные. Значение аргумента *ValuePtr* имеет тип SQLLEN.  
  
|Значение *ValuePtr*|Описание|  
|----------------------|-----------------|  
|SQL_TL_OFF|Отключает ведение журнала операций, выполняемых с данными типа **Text** и **Image** .|  
|SQL_TL_ON|По умолчанию. Включает ведение журнала операций, выполняемых с данными типа **Text** и **Image** .|  
  
### <a name="sql_sopt_ss_hidden_columns"></a>SQL_SOPT_SS_HIDDEN_COLUMNS  
 В результирующем наборе атрибут SQL_SOPT_SS_HIDDEN_COLUMNS предоставляет столбцы, скрытые в инструкции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SELECT FOR BROWSE. По умолчанию драйвер не предоставляет доступ к этим столбцам. Значение аргумента *ValuePtr* имеет тип SQLLEN.  
  
|Значение *ValuePtr*|Описание|  
|----------------------|-----------------|  
|SQL_HC_OFF|По умолчанию. Столбцы FOR BROWSE в результирующем наборе скрыты.|  
|SQL_HC_ON|Обеспечивает доступ к столбцам FOR BROWSE.|  
  
### <a name="sql_sopt_ss_querynotification_msgtext"></a>Атрибут SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT  
 Атрибут SQL_SOPT_SS_QUERYNOTIFICATION_MSGTEXT возвращает текст сообщения в ответ на запрошенное уведомление о запросе.  
  
### <a name="sql_sopt_ss_querynotification_options"></a>SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS  
 Атрибут SQL_SOPT_SS_QUERYNOTIFICATION_OPTIONS задает параметры для запроса уведомления о запросе. Эти параметры заданы в строке с конструкцией `name=value`, как показано ниже. За создание службы и считывание уведомлений из очереди отвечает приложение.  
  
 Строка параметров уведомлений запросов имеет следующий синтаксис.  
  
 `service=<service-name>[;(local database=<database>|broker instance=<broker instance>)]`  
  
 Пример:  
  
 `service=mySSBService;local database=mydb`  
  
### <a name="sql_sopt_ss_querynotification_timeout"></a>SQL_SOPT_SS_QUERYNOTIFICATION_TIMEOUT  
 Атрибут SQL_SOPT_SS_QUERYNOTIFICATION_TIMEOUT задает время в секундах, в течение которого уведомление о запросе будет активным. Значение по умолчанию составляет 432 000 секунд (5 суток). Значение аргумента *ValuePtr* имеет тип SQLLEN.  
  
### <a name="sql_sopt_ss_param_focus"></a>SQL_SOPT_SS_PARAM_FOCUS  
 Атрибут SQL_SOPT_SS_PARAM_FOCUS задает фокус для последующих вызовов SQLBindParameter, SQLGetDescField, SQLSetDescField, SQLGetDescRec и SQLSetDescRec.  
  
 Атрибут SQL_SOPT_SS_PARAM_FOCUS принадлежит к типу SQLULEN.  
  
 По умолчанию он имеет значение 0; это означает, что вызовы относятся к параметрам, соответствующим маркерам параметров в инструкции SQL. Если задать для атрибута значение, равное порядковому номеру возвращающего табличное значение параметра, вызовы будут относиться к столбцам этого параметра. Если для параметра задано значение, которое не является номером параметра с табличным значением, то эти вызовы возвращают ошибку IM020: "фокус параметра не ссылается на параметр, возвращающий табличное значение".  
  
### <a name="sql_sopt_ss_name_scope"></a>SQL_SOPT_SS_NAME_SCOPE  
 Атрибут SQL_SOPT_SS_NAME_SCOPE задает область действия имени для последующих вызовов функций работы с каталогами. Результирующий набор, возвращаемый функцией SQLColumns, зависит от значения параметра SQL_SOPT_SS_NAME_SCOPE.  
  
 Атрибут SQL_SOPT_SS_NAME_SCOPE принадлежит к типу SQLULEN.  
  
|Значение *ValuePtr*|Описание|  
|----------------------|-----------------|  
|SQL_SS_NAME_SCOPE_TABLE|По умолчанию.<br /><br /> При использовании возвращающих табличное значение параметров этот атрибут указывает, что нужно возвратить метаданные реально существующих таблиц.<br /><br /> При использовании функции разреженных столбцов SQLColumns возвращает только столбцы, не являющиеся членами разреженного **column_set**.|  
|SQL_SS_NAME_SCOPE_TABLE_TYPE|Указывает, что приложению требуются метаданные для табличного типа, а не для реально существующих таблиц (функции работы с каталогами должны возвращать метаданные для табличных типов). Затем приложение передает TYPE_NAME возвращающего табличное значение параметра в виде параметра *TableName* .|  
|SQL_SS_NAME_SCOPE_EXTENDED|При использовании функции разреженных столбцов SQLColumns возвращает все столбцы независимо от **column_set** членства.|  
|SQL_SS_NAME_SCOPE_SPARSE_COLUMN_SET|При использовании функции разреженных столбцов SQLColumns возвращает только те столбцы, которые являются элементами разреженного **column_set**.|  
|SQL_SS_NAME_SCOPE_DEFAULT|Равен атрибуту SQL_SS_NAME_SCOPE_TABLE.|  
  
 SS_TYPE_CATALOG_NAME и SS_TYPE_SCHEMA_NAME используются с параметрами *CatalogName* и *SchemaName* соответственно, чтобы найти каталог и схему для возвращающего табличное значение параметра. Когда приложение закончит получать метаданные для возвращающего табличное значение параметра, оно должно вновь присвоить SQL_SOPT_SS_NAME_SCOPE значение по умолчанию SQL_SS_NAME_SCOPE_TABLE.  
  
 Если SQL_SOPT_SS_NAME_SCOPE имеет значение SQL_SS_NAME_SCOPE_TABLE, то запросы к связанным серверам завершаются ошибкой. Вызовы SQLColumns или SQLPrimaryKeys с каталогом, содержащим серверный компонент, завершатся ошибкой.  
  
 При попытке задать для атрибута SQL_SOPT_SS_NAME_SCOPE недопустимое значение будет возвращено значение SQL_ERROR и создана диагностическая запись с параметром SQLSTATE HY024 и сообщением «Недопустимое значение атрибута».  
  
 Если функция каталога, отличная от then SQLTables, SQLColumns или SQLPrimaryKeys, вызывается, когда SQL_SOPT_SS_NAME_SCOPE имеет значение, отличное от SQL_SS_NAME_SCOPE_TABLE, возвращается SQL_ERROR. Создается диагностическая запись с параметром SQLSTATE HY010 и сообщением «Ошибочная последовательность функций (значение атрибута SQL_SOPT_SS_NAME_SCOPE не равно SQL_SS_NAME_SCOPE_TABLE)».  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLGetStmtAttr](../../odbc/reference/syntax/sqlgetstmtattr-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

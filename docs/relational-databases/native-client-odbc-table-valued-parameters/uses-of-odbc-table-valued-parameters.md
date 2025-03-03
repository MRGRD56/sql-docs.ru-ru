---
description: Сценарии использования возвращающих табличное значение параметров ODBC
title: Использование параметров Table-Valued ODBC | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), scenarios
- ODBC, table-valued parameters
ms.assetid: f1b73932-4570-4a8a-baa0-0f229d9c32ee
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0694bb36991fd0d9b8d7c9b652f6d7eaa93998a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755024"
---
# <a name="uses-of-odbc-table-valued-parameters"></a>Сценарии использования возвращающих табличное значение параметров ODBC
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В данном разделе обсуждаются основные пользовательские сценарии для использования возвращающих табличное значение параметров ODBC.  
  
-   Возвращающий табличное значение параметр с многострочными буферами с полной привязкой (отправка данных в виде возвращающего табличное значение параметра со всеми значениями в памяти)  
  
-   Возвращающий табличное значение параметр с поддержкой потоковой работы со строками (отправка данных в виде возвращающего табличное значение параметра с использованием данных времени выполнения).  
  
-   Получение метаданных возвращающих табличное значение параметров из системного каталога  
  
-   Получение метаданных возвращающих табличное значение параметров для подготовленной инструкции  
  
## <a name="table-valued-parameter-with-fully-bound-multirow-buffers-send-data-as-a-tvp-with-all-values-in-memory"></a>Возвращающий табличное значение параметр с многострочными буферами с полной привязкой (отправка данных в виде возвращающего табличное значение параметра со всеми значениями в памяти)  
 При использовании с многострочными буферами с полной привязкой все значения параметров доступны в памяти. Например, это характерно для транзакции OLTP, в которой возвращающие табличное значение параметры могут быть упакованы в одну хранимую процедуру. Без возвращающих табличное значение параметров для этого потребовалось бы динамическое создание сложного пакета с несколькими инструкциями или несколько обращений к серверу.  
  
 Возвращающий табличное значение параметр привязывается с помощью [SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md) вместе с другими параметрами. После привязки всех параметров приложение задает атрибут фокуса параметра, SQL_SOPT_SS_PARAM_FOCUS, для каждого возвращающего табличное значение параметра и вызывает SQLBindParameter для столбцов возвращающего табличное значение параметра.  
  
 Тип сервера для возвращающего табличное значение параметра является новым типом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], SQL_SS_TABLE. Типом привязки C для типа SQL_SS_TABLE должен быть всегда SQL_C_DEFAULT. Никакие данные для параметра, привязанного к возвращающему табличное значение параметру, не передаются; они используются, чтобы передавать метаданные таблицы и управлять передачей данных в столбцах, составляющих возвращающий табличное значение параметр.  
  
 Длина возвращающего табличное значение параметра устанавливается в значение количества строк, отправленных серверу. Параметр *ColumnSize* объекта SQLBindParameter для возвращающего табличное значение параметра указывает максимальное количество строк, которые могут быть отправлены. это размер массива буферов столбцов. *Параметервалуептр* — это буфер параметров для возвращающего табличное значение параметра в SQLBindParameter. *Параметервалуептр* и связанные с ним *BufferLength* используются для передачи имени типа возвращающего табличное значение параметра, если это необходимо. Имя типа не требуется для вызова хранимой процедуры, но требуется для инструкций SQL.  
  
 Если в вызове SQLBindParameter указано имя типа возвращающего табличное значение параметра, оно всегда должно быть указано в Юникоде даже в приложениях, созданных как приложения ANSI. При указании имени типа возвращающего табличное значение параметра с помощью SQLSetDescField можно использовать литерал, который соответствует способу построения приложения. Диспетчер драйвера ODBC выполнит все необходимые преобразования данных в Юникод.  
  
 Метаданные для возвращающих табличные значения параметров и столбцов возвращающих табличное значение параметров можно манипулировать по отдельности и явно с помощью SQLGetDescRec, SQLSetDescRec, SQLGetDescField и SQLSetDescField. Однако перегрузка SQLBindParameter обычно более удобна и не требует явного доступа к дескриптору в большинстве случаев. Этот подход согласуется с определением SQLBindParameter для других типов данных, за исключением того, что для возвращающего табличное значение параметра затронутые поля дескриптора немного отличаются.  
  
 Иногда приложение использует возвращающий табличное значение параметр с динамическими инструкциями SQL, при этом имя типа возвращающего табличное значение параметра должно быть указано. Если это так, а параметр, возвращающий табличное значение, не определен в текущей схеме по умолчанию для соединения, SQL_CA_SS_TYPE_CATALOG_NAME и SQL_CA_SS_TYPE_SCHEMA_NAME должны быть заданы с помощью SQLSetDescField. Так как определения табличного типа и возвращающие табличное значение параметры должны находиться в одной базе данных, значение SQL_CA_SS_TYPE_CATALOG_NAME не должно быть установлено, если приложение использует возвращающие табличное значение параметры. В противном случае SQLSetDescField сообщит об ошибке.  
  
 Пример кода для этого сценария приведен в процедуре, `demo_fixed_TVP_binding` [используемой Table-Valued параметрами &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md).  
  
## <a name="table-valued-parameter-with-row-streaming-send-data-as-a-tvp-using-data-at-execution"></a>Возвращающий табличное значение параметр с поддержкой потоковой работы со строками (отправка данных в виде возвращающего табличное значение параметра с использованием данных времени выполнения).  
 В данном сценарии приложение передает строки драйверу, когда он их запрашивает, и они передаются потоком на сервер. Это помогает избежать буферизации всех строк в памяти. Это типично для массовой вставки или обновления сценариев. Возвращающие табличное значение параметры обеспечивают показатель производительности где-то между массивами параметров и массовым копированием. То есть возвращающие табличное значение параметры почти так же легко программировать, как и массивы параметров, но они дают большую гибкость на сервере.  
  
 Возвращающий табличное значение параметр и его столбцы привязаны, как описано в предыдущем разделе «Возвращающий табличное значение параметр с многострочными буферами с полной привязкой», но признак длины возвращающего табличное значение параметра установлен в значение SQL_DATA_AT_EXEC. Драйвер реагирует на SQLExecute или Склексекутедирект обычным способом для параметров выполнения данных, то есть путем возвращения SQL_NEED_DATA. Когда драйвер готов принять данные для возвращающего табличное значение параметра, метод SQLParamData возвращает значения *параметервалуептр* в SQLBindParameter.  
  
 Приложение использует SQLPutData для возвращающего табличное значение параметра, чтобы указать доступность данных для составных столбцов возвращающего табличное значение параметра. При вызове SQLPutData для возвращающего табличное значение параметра *датаптр* должен быть равен null, а *StrLen_Or_Ind* должен быть равен 0 или быть числом, меньшим или равным размеру массива, заданному для буферов параметров, возвращающих табличное значение (параметр *ColumnSize* метода SQLBindParameter). 0 обозначает, что в возвращающем табличное значение параметре больше нет строк и драйвер продолжит обработку со следующего фактического параметра процедуры. Если значение *StrLen_Or_Ind* не равно 0, драйвер будет обрабатывать составные столбцы возвращающего табличное значение параметра так же, как параметры, связанные с параметрами, не являющиеся табличными. в каждом столбце возвращающего табличное значение параметра можно указать его фактическую длину данных, SQL_NULL_DATA или указать данные во время выполнения через буфер длины или индикатора. Значения столбцов возвращающего табличное значение параметра могут передаваться повторными вызовами SQLPutData в обычном режиме, когда символьное или двоичное значения передаются в части.  
  
 После того как все столбцы возвращающего табличное значение параметра были обработаны, драйвер снова обращается к возвращающему табличное значение параметру для обработки следующих строк данных возвращающего табличное значение параметра. Поэтому для возвращающих табличное значение параметров с данными времени выполнения драйвер не выполняет обычный последовательный просмотр привязанных параметров. Связанный возвращающий табличное значение параметр будет опрашиваться до тех пор, пока SQLPutData не вызовется с *StrLen_or_IndPtr* равно 0, когда драйвер пропускает столбцы возвращающего табличное значение параметра и переходит к следующему реальному параметру хранимой процедуры.  Когда SQLPutData передает значение индикатора, большее или равное 1, драйвер обрабатывает столбцы и строки возвращающих табличные значения параметров последовательно до тех пор, пока они не поступают со значениями для всех привязанных строк и столбцов. Затем драйвер возвращается к возвращающему табличное значение параметру. Между получением маркера для возвращающего табличное значение параметра из метод SQLParamData и вызовом SQLPutData (хстмт, NULL, n) для возвращающего табличное значение параметра приложение должно установить возвращающий табличное значение параметр, составляющий данные столбцов и содержимое буфера индикаторов для следующей строки или строк, которые будут переданы на сервер.  
  
 Пример кода для этого сценария приведен в подпрограмме, `demo_variable_TVP_binding` [используемой Table-Valued параметрами &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md).  
  
## <a name="retrieving-table-valued-parameter-metadata-from-the-system-catalog"></a>Получение метаданных возвращающих табличное значение параметров из системного каталога  
 Когда приложение вызывает SQLProcedureColumns для процедуры, которая содержит параметры возвращающего табличное значение параметра, DATA_TYPE возвращается как SQL_SS_TABLE, а TYPE_NAME — имя табличного типа для возвращающего табличное значение параметра. В результирующий набор, возвращаемый функцией SQLProcedureColumns, добавляются два дополнительных столбца. SS_TYPE_CATALOG_NAME возвращает имя каталога, в котором определен табличный тип параметра table-value, и SS_TYPE_SCHEMA_NAME возвращает имя схемы, в которой определен тип таблицы параметра table-value. В соответствии со спецификацией ODBC SS_TYPE_CATALOG_NAME и SS_TYPE_SCHEMA_NAME применяются до всех столбцов драйвера, которые были добавлены в предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], и после всех столбцов, применяемых ODBC.  
  
 Новые столбцы будут заполнены не только для возвращающих табличное значение параметров, но и для параметров определенного пользователем типа данных CLR. Существующие схема и столбцы каталога параметров определяемого пользователем типа будут все равно заполнены, но наличие общих схемы и столбцов каталога для типов данных, для которых они требуются, упростит разработку приложения в будущем. (Следует отметить, что коллекции схем XML несколько отличаются и не включаются в это изменение).  
  
 Приложение использует SQLTables для определения имен табличных типов так же, как и для постоянных таблиц, системных таблиц и представлений. Новый табличный тип TABLE TYPE вводится, чтобы приложение могло определить табличный тип, связанный с возвращающими табличное значение параметрами. Табличные типы и обычные таблицы используют различные пространства имен. Это значит, что можно использовать одно и то же имя как для табличного типа, так и для существующей таблицы. Для обработки этой ситуации был введен новый атрибут инструкции SQL_SOPT_SS_NAME_SCOPE. Этот атрибут указывает, должны ли SQLTables и другие функции каталога, принимающие имя таблицы в качестве параметра, интерпретировать имя таблицы как имя фактической таблицы или имя табличного типа.  
  
 Приложение использует SQLColumns, чтобы определить столбцы для табличного типа так же, как и для постоянных таблиц, но сначала необходимо задать SQL_SOPT_SS_NAME_SCOPE, чтобы указать, что она работает с табличными типами, а не с реальными таблицами. SQLPrimaryKeys также можно использовать с табличными типами, опять же используя SQL_SOPT_SS_NAME_SCOPE.  
  
 Пример кода для этого сценария приведен в подпрограмме, `demo_metadata_from_catalog_APIs` [используемой Table-Valued параметрами &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md).  
  
## <a name="retrieving-table-valued-parameter-metadata-for-a-prepared-statement"></a>Получение метаданных возвращающих табличное значение параметров для подготовленной инструкции  
 В этом сценарии приложение использует Склнумпараметерс и SQLDescribeParam для получения метаданных для возвращающих табличное значение параметров.  
  
 IPD-поле атрибута SQL_CA_SS_TYPE_NAME используется для получения имени типа для возвращающего табличное значение параметра. IPD-поля атрибутов SQL_CA_SS_TYPE_SCHEMA_NAME и SQL_CA_SS_TYPE_CATALOG_NAME используются соответственно для получения каталога и схемы этого параметра.  
  
 Определения табличных типов и возвращающие табличное значение параметры должны находиться в одной базе данных. SQLSetDescField будет сообщать об ошибке, если приложение задает SQL_CA_SS_TYPE_CATALOG_NAME при использовании возвращающих табличное значение параметров.  
  
 Атрибуты SQL_CA_SS_TYPE_CATALOG_NAME и SQL_CA_SS_TYPE_SCHEMA_NAME могут также использоваться для получения каталога и схемы, связанных с параметрами определяемых пользователем типов данных CLR. Атрибуты SQL_CA_SS_TYPE_CATALOG_NAME и SQL_CA_SS_TYPE_SCHEMA_NAME представляют собой альтернативу существующим атрибутам типов в схеме каталогов для определяемых пользователем типов данных CLR.  
  
 Приложение использует SQLColumns для получения метаданных столбца для возвращающего табличное значение параметра в этом сценарии, так как SQLDescribeParam не возвращает метаданные для столбцов возвращающего табличное значение параметра столбца.  
  
 Пример кода для этого варианта использования — в подпрограмме, `demo_metadata_from_prepared_statement` [используемой Table-Valued параметрами &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/use-table-valued-parameters-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  

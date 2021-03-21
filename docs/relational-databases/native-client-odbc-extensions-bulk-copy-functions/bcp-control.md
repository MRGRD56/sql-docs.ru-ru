---
description: bcp_control
title: bcp_control | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_control
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_control function
ms.assetid: 32187282-1385-4c52-9134-09f061eb44f5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 335dd32c4d3ba62c9aae09cc38adb4383d1f71fd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749584"
---
# <a name="bcp_control"></a>bcp_control
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Изменяет значения по умолчанию различных параметров управления для массового копирования между файлом и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
RETCODE bcp_control (  
        HDBC hdbc,  
        INT eOption,  
        void* iValue);  
```  
  
## <a name="arguments"></a>Аргументы  
 *hdbc*  
 Дескриптор соединения ODBC с поддержкой массового копирования.  
  
 *eOption*  
 Принимает одно из следующих значений.  
  
 BCPABORT  
 Останавливает текущую операцию массового копирования. Вызовите **bcp_control** с *eOption* бкпаборт из другого потока, чтобы прерывать выполнение операции с массовым копированием. Параметр *iValue* игнорируется.  
  
 BCPBATCH  
 Число строк в пакете. Значение по умолчанию — 0, что указывает либо все строки в таблице при извлечении данных, либо все строки в пользовательском файле данных, когда данные копируются в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если задать для этого параметра значение меньше 1, то значению BCPBATCH будет установлено значение по умолчанию.  
  
 BCPDELAYREADFMT  
 Логическое значение, если установлено значение true, вызовет [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) чтение во время выполнения. Если значение равно false (по умолчанию), bcp_readfmt немедленно прочитает файл форматирования. Если BCPDELAYREADFMT имеет значение true и вы вызываете bcp_columns или bcp_setcolfmt, возникнет ошибка последовательности.  
  
 Ошибка последовательности также возникает при вызове `bcp_control(hdbc,` BCPDELAYREADFMT `, (void *)FALSE)` после вызова `bcp_control(hdbc,` BCPDELAYREADFMT `, (void *)TRUE)` и bcp_writefmt.  
  
 Дополнительные сведения см. в разделе [Обнаружение метаданных](../../relational-databases/native-client/features/metadata-discovery.md).  
  
 BCPFILECP  
 *iValue* содержит номер кодовой страницы для файла данных. Можно указать номер кодовой страницы, например 1252 или 850, либо одно из следующих значений.  
  
 BCPFILE_ACP: данные в файле хранятся в кодовой странице Microsoft Windows® клиента.  
  
 BCPFILE_OEMCP: данные в файле хранятся в кодовой странице изготовителя оборудования (OEM) клиента (по умолчанию).  
  
 BCPFILE_RAW: данные в файле хранятся в кодовой странице [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 BCPFILEFMT  
 Номер версии для формата файла данных. Это может быть 80 ( [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ), 90 ( [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ), 100 ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] или [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ), 110 ( [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ) или 120 ( [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ). 120 используется по умолчанию. Это может оказаться полезным при экспорте или импорте данных в форматах, которые поддерживались прежними версиями сервера. Например, чтобы импортировать данные, полученные из текстового столбца [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] сервера в столбец **varchar (max)** на [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] сервере или более поздней версии, следует указать 80. Аналогично, если указать 80 при экспорте данных из столбца **varchar (max)** , они будут сохранены так же, как текстовые столбцы, сохраняются в [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] формате и могут быть импортированы в текстовый столбец [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] сервера.  
  
 BCPFIRST  
 Первая строка данных, копируемых в файл или таблицу. Значение по умолчанию равно 1. Если задать для этого параметра значение меньше 1, то будет установлено значение по умолчанию.  
  
 BCPFIRSTEX  
 В операциях bcp out задает первую строку таблицы базы данных для копирования в файл данных.  
  
 В операциях bcp in задает первую строку файла данных для копирования в таблицу базы данных.  
  
 Параметр *iValue* должен быть адресом 64-разрядного целого числа со знаком, содержащим значение. Максимальное значение, передаваемое в BCPFIRSTEX, составляет 2^63-1.  
  
 BCPFMTXML  
 Указывает, что формируемый файл форматирования должен быть в формате XML. По умолчанию эта настройка отключена.  
  
 XML-файлы форматирования обеспечивают большую гибкость, но имеют некоторые ограничения. Например, нельзя одновременно указать префикс и признак конца для поля, что было возможно в более старых файлах форматирования.  
  
> [!NOTE]  
>  XML-файлы форматирования поддерживаются только при установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вместе с собственным клиентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 BCPHINTS  
 *iValue* содержит указатель на SQLTCHAR строку символов. Адресуемая строка задает указания для обработки массового копирования [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или инструкцию Transact-SQL, которая возвращает результирующий набор. Если задана инструкция Transact-SQL, которая возвращает несколько результирующих наборов, все результирующие наборы после первого пропускаются. Дополнительные сведения о указаниях по обработке массовых копий см. в разделе [программа bcp](../../tools/bcp-utility.md).  
  
 BCPKEEPIDENTITY  
 Если *iValue* имеет значение true, то указывает, что функции операций с массовым копированием вставляют значения данных, предоставляемые для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбцов, определенных с помощью ограничения IDENTITY. Входной файл должен содержать значения для столбцов идентификаторов. Если эти значения не заданы, то для вставляемых строк создаются новые значения идентификаторов. Данные в файле, предназначенные для столбцов идентификаторов, не учитываются.  
  
 BCPKEEPNULLS  
 Указывает, будут ли пустые значения данных в файле преобразовываться в значения NULL в таблице [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Если *iValue* имеет значение true, пустые значения будут преобразованы в значение NULL в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] таблице. По умолчанию пустые значения преобразовываются в значения по умолчанию для столбца в таблице [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , если значение по умолчанию существует.  
  
 BCPLAST  
 Последняя строка для копирования. По умолчанию установлено копирование всех строк; если задать для этого параметра значение меньше 1, то ему будет установлено значение по умолчанию.  
  
 BCPLASTEX  
 В операциях bcp out задает последнюю строку таблицы базы данных для копирования в файл данных.  
  
 В операциях bcp in задает последнюю строку файла данных для копирования в таблицу базы данных.  
  
 Параметр *iValue* должен быть адресом 64-разрядного целого числа со знаком, содержащим значение. Максимальное значение, передаваемое в BCPLASTEX, составляет 2^63-1.  
  
 BCPMAXERRS  
 Число ошибок, после которого произойдет сбой операции массового копирования. Значение по умолчанию — 10. значение меньше 1 сбрасывает этот параметр до значения по умолчанию. В операции массового копирования допускается не более 65 535 ошибок. Если выполнить попытку установить этот параметр в значение, превышающее 65 535, будет установлено значение 65 535.  
  
 BCPODBC  
 Если значение равно TRUE, то указывает, что значения **DateTime** и **smalldatetime** , сохраненные в символьном формате, будут использовать префикс и суффикс последовательной отметки времени ODBC. Параметр BCPODBC применяется только к DB_OUT.  
  
 При значении FALSE значение **DateTime** , представляющее 1 января 1997, преобразуется в символьную строку: 1997-01-01 00:00:00.000. При значении TRUE одно и то же значение **DateTime** представлено следующим образом: {ts ' 1997-01-01 00:00:00.000 '}.  
  
 BCPROWCOUNT  
 Возвращает число строк, на которые распространяется действие текущей (или последней) операции bcp.  
  
 BCPTEXTFILE  
 Значение TRUE указывает, что файл данных является текстовым, а не двоичным файлом. Если файл является текстовым, BCP определяет, является ли его кодировка кодировкой Юникод, путем проверки байтового маркера Юникода в первых двух байтах файла данных.  
  
 BCPUNICODEFILE  
 В значении TRUE указывает, что входной файл является файлом в кодировке Юникод.  
  
 *iValue*  
 Значение для указанного *eOption*. *iValue* — это целочисленное значение (лонглонг), приведенное к указателю void, чтобы обеспечить будущее расширение до 64 битовых значений.  
  
## <a name="returns"></a>Возвращаемое значение  
 SUCCEED или FAIL.  
  
## <a name="remarks"></a>Remarks  
 Эта функция задает различные параметры управления для операций массового копирования, включая число ошибок, после которого массовое копирование будет отменено, номера первой и последней строк для копирования из файла данных, а также размер пакета.  
  
 Эта функция также используется для указания инструкции SELECT при массовом копировании результирующего набора инструкции SELECT из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Присвойте параметру *eOption* значение bcphints а и задайте для *iValue* значение указатель на строку SQLTCHAR, содержащую инструкцию SELECT.  
  
 Эти параметры управления имеют значение только при копировании между пользовательским файлом и таблицей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Параметры управления параметрами не влияют на строки, скопированные в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md).  
  
## <a name="example"></a>Пример  
  
```  
// Variables like henv not specified.  
SQLHDBC      hdbc;  
DBINT      nRowsProcessed;  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...   
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source, return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bulk copy.   
if (bcp_init(hdbc, _T("address"), _T("address.add"), _T("addr.err"),  
   DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set the number of rows per batch.   
if (bcp_control(hdbc, BCPBATCH, (void*) 1000) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set file column count.   
if (bcp_columns(hdbc, 1) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set the file format.   
if (bcp_colfmt(hdbc, 1, 0, 0, SQL_VARLEN_DATA, '\n', 1, 1)  
   == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Execute the bulk copy.   
if (bcp_exec(hdbc, &nRowsProcessed) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
printf_s("%ld rows processed by bulk copy.", nRowsProcessed);  
  
```  
  
## <a name="see-also"></a>См. также:  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  

---
description: Выделение дескрипторов и соединение с SQL Server (ODBC)
title: Выделение дескрипторов и подключение к SQL Server (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- handles [ODBC]
- handles [ODBC], connection
- handles [ODBC], about handles
ms.assetid: 6172cd52-9c9a-467d-992f-def07f3f3bb1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c981ed247a40c42d8ef8737d1ae891296194dad
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753284"
---
# <a name="allocate-handles-and-connect-to-sql-server-odbc"></a>Выделение дескрипторов и соединение с SQL Server (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-allocate-handles-and-connect-to-sql-server"></a>Выделение дескрипторов и соединение с SQL Server  
  
1.  Включите файлы заголовка ODBC Sql.h, Sqlext.h, Sqltypes.h.  
  
2.  Включите зависящий от драйвера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] файл заголовка Odbcss.h.  
  
3.  Вызовите [функцию SQLAllocHandle](../../odbc/reference/syntax/sqlallochandle-function.md) с **параметром handletype** SQL_HANDLE_ENV, чтобы инициализировать ODBC и выделить обработчик среды.  
  
4.  Вызовите [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) с **атрибутом** , имеющим значение SQL_ATTR_ODBC_VERSION, а **ValuePtr** — значение SQL_OV_ODBC3, чтобы указать, что приложение будет использовать вызовы функций формата ODBC 3. x.  
  
5.  При необходимости вызовите [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) , чтобы задать другие параметры среды, или вызовите [SQLGetEnvAttr](../../odbc/reference/syntax/sqlgetenvattr-function.md) , чтобы получить параметры среды.  
  
6.  Вызовите [функцию SQLAllocHandle](../../odbc/reference/syntax/sqlallochandle-function.md) с **параметром handletype** SQL_HANDLE_DBC, чтобы выделить маркер подключения.  
  
7.  При необходимости вызовите [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) , чтобы задать параметры соединения, или вызовите [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) , чтобы получить параметры соединения.  
  
8.  Вызовите SQLConnect, чтобы использовать существующий источник данных для подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     либо  
  
     Вызовите [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) , чтобы использовать строку подключения для подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Минимальная строка соединения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имеет одну из двух форм:  
  
    ```  
    DSN=dsn_name;Trusted_connection=yes;  
    DRIVER={SQL Server Native Client 10.0};SERVER=server;Trusted_connection=yes;  
    ```  
  
     Если строка подключения не завершена, **SQLDriverConnect** может запросить необходимые сведения. Это определяется значением, указанным для параметра *DriverCompletion* .  
  
     \- или -  
  
     Вызывайте [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) несколько раз в итеративном виде для создания строки подключения и подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
9. При необходимости вызовите [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md) , чтобы получить атрибуты и поведение драйвера для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] источника данных.  
  
10. Выделите и используйте инструкции.  
  
11. Вызовите SQLDisconnect, чтобы отключиться от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и сделать маркер соединения доступным для нового соединения.  
  
12. Вызовите [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) с **параметром handletype** SQL_HANDLE_DBC, чтобы освободить маркер подключения.  
  
13. Вызовите **SQLFreeHandle** с **параметром handletype** SQL_HANDLE_ENV, чтобы освободить обработчик среды.  
  
> [!IMPORTANT]  
>  По возможности используйте аутентификацию Windows. Если проверка подлинности Windows недоступна, запросите у пользователя ввод учетных данных во время выполнения. Избегайте хранения учетных данных в файле. Если необходимо сохранить учетные данные, зашифруйте их с помощью [API-интерфейса шифрования Win32](/windows/win32/seccrypto/cryptography-reference).  
  
## <a name="example"></a>Пример  
 В этом примере показан вызов **SQLDriverConnect** для подключения к экземпляру, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не требуя наличия существующего источника данных ODBC. Передавая неполную строку подключения в **SQLDriverConnect**, драйвер ODBC запрашивает у пользователя ввод недостающих данных.  
  
```  
#define MAXBUFLEN   255  
  
SQLHENV      henv = SQL_NULL_HENV;  
SQLHDBC      hdbc1 = SQL_NULL_HDBC;  
SQLHSTMT      hstmt1 = SQL_NULL_HSTMT;  
  
SQLCHAR      ConnStrIn[MAXBUFLEN] =  
         "DRIVER={SQL Server Native Client 10.0};SERVER=MyServer";  
  
SQLCHAR      ConnStrOut[MAXBUFLEN];  
SQLSMALLINT   cbConnStrOut = 0;  
  
// Make connection without data source. Ask that driver   
// prompt if insufficient information. Driver returns  
// SQL_ERROR and application prompts user  
// for missing information. Window handle not needed for  
// SQL_DRIVER_NOPROMPT.  
retcode = SQLDriverConnect(hdbc1,      // Connection handle  
                  NULL,         // Window handle  
                  ConnStrIn,      // Input connect string  
                  SQL_NTS,         // Null-terminated string  
                  ConnStrOut,      // Address of output buffer  
                  MAXBUFLEN,      // Size of output buffer  
                  &cbConnStrOut,   // Address of output length  
                  SQL_DRIVER_PROMPT);  
```  
  

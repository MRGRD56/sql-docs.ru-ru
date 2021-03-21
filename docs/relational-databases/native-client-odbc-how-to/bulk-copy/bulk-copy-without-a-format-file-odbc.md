---
description: Выполнение массового копирования без файла форматирования (ODBC)
title: Групповое копирование без файла форматирования (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], file formats
- bulk copy [ODBC]
- bulk copy [ODBC], data files
- bulk copy [ODBC], about bulk copy
ms.assetid: 4ee969a7-44ba-40d0-b9ab-8306f1a2b19d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4266867f0aa3ec047c85fff57369a75fd0ff913
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744034"
---
# <a name="bulk-copy-without-a-format-file-odbc"></a>Выполнение массового копирования без файла форматирования (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В этом образце показывается, как использовать функции массового копирования без файла форматирования для создания файла данных в собственном режиме. Этот образец разработан для ODBC версии 3.0 или более поздней.  
  
> [!IMPORTANT]  
>  По возможности используйте аутентификацию Windows. Если проверка подлинности Windows недоступна, запросите у пользователя ввод учетных данных во время выполнения. Избегайте хранения учетных данных в файле. Если необходимо сохранить учетные данные, зашифруйте их с помощью [API-интерфейса шифрования Win32](/windows/win32/seccrypto/cryptography-reference).  
  
### <a name="to-bulk-copy-without-a-format-file"></a>Массовое копирование без файла форматирования  
  
1.  Выделите дескриптор среды и дескриптор соединения.  
  
2.  Включите операцию массового копирования, укажите параметры SQL_COPT_SS_BCP и SQL_BCP_ON.  
  
3.  Соединитесь с SQL Server.  
  
4.  Вызовите [bcp_init](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) , чтобы задать следующие сведения:  
  
    -   Имя таблицы или представления, из которого или в которое будет производиться массовое копирование.  
  
    -   Имя файла данных, в котором содержатся данные для копирования в базу данных или который получает данные при копировании из базы данных.  
  
    -   Имя файла данных, в который сохраняются все сообщения об ошибках массового копирования (укажите значение NULL, если файл для сообщений не требуется).  
  
    -   Направление копирования: DB_IN из файла в представление или таблицу или DB_OUT в файл из таблицы или представления.  
  
5.  Вызовите [bcp_exec](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) , чтобы выполнить операцию с массовым копированием.  
  
 Если при выполнении этих шагов установлено значение DB_OUT, файл создается в собственном формате. Затем файл можно с помощью операции массового копирования скопировать на сервер, выполнив те же шаги, за исключением того, что необходимо установить значение DB_OUT вместо DB_IN. Это возможно только в случае, когда структура исходной и целевой таблиц идентична.  
  
## <a name="example"></a>Пример  
 Этот образец не поддерживается на архитектуре IA64.  
  
 Также необходим источник данных ODBC с именем AdventureWorks, для которого базой данных по умолчанию является образец базы данных AdventureWorks. (Образец базы данных AdventureWorks можно скачать на домашней странице [Microsoft SQL Server примеры и проекты сообщества](https://go.microsoft.com/fwlink/?LinkID=85384) .) Этот источник данных должен быть основан на драйвере ODBC, предоставленном операционной системой (имя драйвера — "SQL Server"). При построении и запуске этого образца как 32-разрядного приложения в 64-разрядной операционной системе необходимо создать источник данных ODBC с помощью программы администрирования ODBC (исполняемый файл %windir%\SysWOW64\odbcad32.exe).  
  
 Этот образец соединяется с установленным на компьютер экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию. Чтобы соединиться с именованным экземпляром, измените определение источника данных ODBC, указав экземпляр в следующем формате: Сервер\ИменованныйЭкземпляр. По умолчанию [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)] устанавливается на именованный экземпляр.  
  
 Скомпилируйте с библиотеками odbc32.lib и odbcbcp.lib.  
  
```  
// compile with: odbc32.lib odbcbcp.lib  
#include <stdio.h>  
#include <string.h>  
#include <windows.h>  
#include <sql.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define MAXBUFLEN 256  
  
SQLHENV henv = SQL_NULL_HENV;  
HDBC hdbc1 = SQL_NULL_HDBC;  
  
void Cleanup() {  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
int main() {  
   RETCODE retcode;  
  
   // Bulk copy variables.  
   SDWORD cRows;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle, set bulk copy mode, and then connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   retcode = SQLSetConnectAttr(hdbc1, SQL_COPT_SS_BCP, (void *)SQL_BCP_ON, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetConnectAttr(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample uses Integrated Security, create the SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"AdventureWorks", SQL_NTS, (UCHAR*)"", SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Initialize the bulk copy.  
   retcode = bcp_init(hdbc1, "Purchasing.Vendor", "BCPODBC.bcp", "BCPERROR.out", DB_OUT);  
   // Test is for the bulk copy return of SUCCEED, not the ODBC return of SQL_SUCCESS.  
   if ( (retcode != SUCCEED) ) {  
      printf("bcp_init(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Execute the bulk copy.  
   retcode = bcp_exec(hdbc1, &cRows);  
   if ( (retcode != SUCCEED) ) {  
      printf("bcp_exec(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   printf("Number of rows bulk copied out = %d.\n", cRows);  
  
   // Cleanup  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции по групповому копированию с помощью драйвера ODBC SQL Server &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/bulk-copy/bulk-copying-with-the-sql-server-odbc-driver-how-to-topics-odbc.md)  
  

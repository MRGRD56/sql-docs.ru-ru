---
description: bcp_init
title: bcp_init | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_init
- bcp_initW
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_init function
ms.assetid: 6a25862c-7f31-4873-ab65-30f3abde89d2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3ce9cdbff8e722e732ae8aaf9ced180d1c45eb12
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749544"
---
# <a name="bcp_init"></a>bcp_init
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Инициализирует операцию массового копирования.  

## <a name="syntax"></a>Синтаксис  
  
```  
RETCODE bcp_init (  
        HDBC hdbc,  
        LPCTSTR szTable,  
        LPCTSTR szDataFile,  
        LPCTSTR szErrorFile,  
        INT eDirection);  
```  

Имена в Юникоде и ANSI:
- bcp_initA (ANSI)
- bcp_initW (Юникод)

## <a name="arguments"></a>Аргументы  
 *hdbc*  
 Дескриптор соединения ODBC с поддержкой массового копирования.  
  
 *szTable*  
 Имя таблицы базы данных, в которую (или из которой) выполняется копирование. Это имя также может включать имя базы данных или владельца. Например, **Pubs. Gracie. titles**, **Pubs.. заголовки**, **Gracie. titles** и **titles** — это имена юридических таблиц.  
  
 Если *eDirection* имеет значение DB_OUT, *сзтабле* также может быть именем представления базы данных.  
  
 Если *eDirection* имеет DB_OUT и инструкция SELECT указывается с помощью [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) перед вызовом [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) , **bcp_init** *сзтабле* должно быть установлено в NULL.  
  
 *szDataFile*  
 Имя пользовательского файла, в который или из которого выполняется копирование. Если данные копируются непосредственно из переменных с помощью [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md), задайте для *сздатафиле* значение null.  
  
 *szErrorFile*  
 Имя файла ошибок, заполняемого сообщениями о ходе работы, сообщениями об ошибках и копиями строк, которые по каким-либо причинам не могут быть скопированы из пользовательского файла в таблицу. Если значение NULL передается как *сзеррорфиле*, файл ошибок не используется.  
  
 *eDirection*  
 Направление копирования (DB_IN или DB_OUT). Значение DB_IN указывает на копирование из переменных программы или пользовательского файла в таблицу. Значение DB_OUT указывает на копирование из таблицы базы данных в пользовательский файл. Если используется значение DB_OUT, необходимо указать имя пользовательского файла.  
  
## <a name="returns"></a>Возвращаемое значение  
 SUCCEED или FAIL.  
  
## <a name="remarks"></a>Remarks  
 Вызовите **bcp_init** перед вызовом любой другой функции небольшого копирования. **bcp_init** выполняет необходимые операции инициализации для выполнения операций с массовым копированием данных между рабочей станцией и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Функция **bcp_init** должна быть предоставлена с помощью обработчика соединений ODBC, включенного для использования с функциями операций с массовым копированием. Чтобы включить этот маркер, используйте [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) с параметром SQL_COPT_SS_BCP, чтобы SQL_BCP_ON для выделенного, но не подключенного обработчика соединения. Попытка назначения атрибута для подключенного дескриптора приведет к ошибке.  
  
 Если указан файл данных, **bcp_init** анализирует структуру источника базы данных или целевой таблицы, а не файл данных. **bcp_init** задает значения формата данных для файла данных на основе каждого столбца в таблице базы данных, представлении или результирующем наборе. Эта спецификация включает тип данных каждого столбца, присутствие или отсутствие длины и допустимости значений NULL и строки байтов признака конца, а также ширину для типов данных с фиксированной длиной. **bcp_init** задает следующие значения:  
  
-   Указанный тип данных представляет собой тип данных столбца в таблице базы данных, представлении или результирующем наборе SELECT. Тип данных ограничивается собственными типами данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], указанными в файле sqlncli.h. Сами данные представляются в машинной форме. Это значит, что данные из столбца **целочисленного** типа представляются в последовательности из четырех байтов с большим или прямым порядком байтов на основе компьютера, создавшего файл данных.  
  
-   Если тип данных базы данных имеет фиксированную длину, то для данных файла данных также задается фиксированная длина. Функции с массовым копированием, обрабатывающие данные (например, [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md)), анализируют строки данных, ожидающие длины данных в файле данных, так, чтобы они совпадали с длиной данных, указанных в таблице, представлении или списке столбцов базы данных. Например, данные для столбца базы данных, определенного как **char (13)** , должны быть представлены 13 символами для каждой строки данных в файле. Данные фиксированной длины могут быть обозначены префиксом с признаком значения NULL, если столбец базы данных допускает значения NULL.  
  
-   После определения последовательности байт, служащей признаком конца, ее длина устанавливается в значение 0.  
  
-   При копировании в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] файл данных должен содержать данные для каждого столбца таблицы базы данных. При копировании на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] данные из всех столбцов в таблице, представлении или результирующем наборе инструкции SELECT базы данных копируются в файл данных.  
  
-   При копировании в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] порядковый номер столбца в файле данных должен совпадать с порядковым номером столбца таблицы базы данных. При копировании [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из **bcp_exec** помещает данные в зависимости от порядкового номера столбца в таблице базы данных.  
  
-   Если тип данных базы данных имеет переменную длину (например, **varbinary (22)**) или столбец базы данных может содержать значения NULL, то данные в файле данных имеют префикс длины или значения NULL. Ширина признака изменяется в зависимости от типа данных и версии массового копирования.  
  
 Чтобы изменить значения формата данных, заданные для файла данных, вызовите [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) и [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md).  
  
 Массовое копирование в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может быть оптимизировано для таблиц, не содержащих индексов, путем установки модели восстановления базы данных в значение SIMPLE или BULK_LOGGED. Дополнительные сведения см. [в разделе Предварительные требования для минимального ведения журнала при выполнении массового импорта](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md) и [изменения базы данных](../../t-sql/statements/alter-database-transact-sql.md).  
  
 Если файл данных не используется, необходимо вызвать метод [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) , чтобы указать формат и расположение данных в памяти для каждого столбца, а затем скопировать строки данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md).  
  
## <a name="example"></a>Пример  
 В этом образце демонстрируется использование функции ODBC bcp_init с файлом форматирования.  
  
 Перед компиляцией и выполнением кода на С++ необходимо выполнить следующие действия.  
  
-   Создайте источник данных ODBC с именем Test. Его можно сопоставить с любой базой данных.  
  
-   Выполните в базе данных следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)]:  
  
    ```  
    CREATE TABLE BCPDate (cola int, colb datetime);  
    ```  
  
-   В каталог, где будет запускаться приложение, добавьте файл Bcpfmt.fmt и добавьте в этот файл следующик код:  
  
    ```  
    8.0  
    2  
    1SQLCHAR04"\t"1colaSQL_Latin1_General_Cp437_Bin  
    2SQLCHAR08"\r\n"2colbSQL_Latin1_General_Cp437_Bin  
    ```  
  
-   В каталог, где будет запускаться приложение, добавьте файл Bcpodbc.bcp и добавьте в этот файл следующик код:  
  
    ```  
    1  
    2  
    ```  
  
 Теперь все готово к компиляции и выполнению кода на C++.  
  
```  
// compile with: odbc32.lib sqlncli11.lib  
#include <stdio.h>  
#include <windows.h>  
#include <sql.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
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
  
   // Allocate ODBC connection handle, set BCP mode, and connect.  
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
  
   // Sample uses Integrated Security. Create SQL Server DSN using Windows NT authentication.  
   retcode = SQLConnect(hdbc1, (UCHAR*)"Test", SQL_NTS, (UCHAR*)"", SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Initialize the bulk copy.  
   retcode = bcp_init(hdbc1, "BCPDate", "BCPODBC.bcp", NULL, DB_IN);  
   if ( (retcode != SUCCEED) ) {  
      printf("bcp_init(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Read the format file.  
   retcode = bcp_readfmt(hdbc1, "BCPFMT.fmt");  
   if ( (retcode != SUCCEED) ) {  
      printf("bcp_readfmt(hdbc1) Failed\n\n");  
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
  
   printf("Number of rows bulk copied in = %d.\n", cRows);  
  
   // Cleanup  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
```  

## <a name="see-also"></a>См. также:  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  

---
description: SQLMoreResults
title: SQLMoreResults | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLMoreResults function
ms.assetid: f65698c3-7291-480d-9dab-58b13feb7771
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a4806bb34651c16d4141c4f0cb387a78028404d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749104"
---
# <a name="sqlmoreresults"></a>SQLMoreResults
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Функция **SQLMoreResults** позволяет приложению получать несколько наборов результирующих строк. Если в инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT содержится предложение COMPUTE или отправленный пакет инструкций ODBC или [!INCLUDE[tsql](../../includes/tsql-md.md)] , это это приведет к тому, что драйвер поставщика ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создаст несколько результирующих наборов. В любом из случаев [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не допускает создания серверного курсора для обработки результатов. Таким образом, разработчик должен убедиться, что инструкция ODBC блокируется. Он должен исчерпать все возвращенные данные или отменить инструкцию ODBC до обработки данных от других активных инструкций соединения.  
  
> [!NOTE]  
>  Выполнение инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] SELECT, содержащих предложение COMPUTE, поддерживается только при подключении к версиям сервера ранее [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 Разработчик может определить свойства столбцов и строк результирующих наборов, создаваемых предложением COMPUTE инструкции SELECT языка [!INCLUDE[tsql](../../includes/tsql-md.md)] . Дополнительные сведения см. в разделе [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md).  
  
 При вызове функции **SQLMoreResults** со строками данных без выборки в результирующем наборе эти строки будут потеряны и станут доступны данные строк из следующего результирующего набора строк.  
  
## <a name="examples"></a>Примеры  
  
```  
void GetComputedRows  
    (  
    SQLHSTMT hStmt  
    )   
    {  
    SQLUSMALLINT    nCols;  
    SQLUSMALLINT    nCol;  
    PODBCSETINFO    pODBCSetInfo = NULL;  
    SQLRETURN       sRet;  
    UINT            nRow;  
    SQLINTEGER      nComputes = 0;  
    SQLINTEGER      nSet;  
    BYTE*           pValue;  
  
    // If SQLNumResultCols failed, then some error occurred in  
    //  statement execution. Exit.  
    if (!SQL_SUCCEEDED(SQLNumResultCols(hStmt, (SQLSMALLINT*) &nCols)))  
        {  
        goto EXIT;  
        }  
  
    // Determine the presence of COMPUTE clause result sets. The SQL  
    //  Server Native Client ODBC driver uses column attributes to report multiple  
    //  sets. The column number must be less than or equal to the   
    //  number of columns returned. You are guaranteed to have at least  
    //  one, so use '1' for the SQLColAttribute ColumnNumber  
    //  parameter.  
    SQLColAttribute(hStmt, 1, SQL_CA_SS_NUM_COMPUTES,  
        NULL, 0, NULL, (SQLPOINTER) &nComputes);  
  
    // Create a result info structure pointer array, one element for  
    //  the normal result rows and one for each compute result set.  
    //  Initialize the array to NULL pointers.  
    pODBCSetInfo = new ODBCSETINFO[1 + nComputes];  
  
    // Process the result sets...  
    nSet = 0;  
    while (TRUE)  
        {  
        // If required, get the column information for the result set.  
        if (pODBCSetInfo[nSet].pODBCColInfo == NULL)  
            {  
            if (pODBCSetInfo[nSet].nCols == 0)  
                {  
                SQLNumResultCols(hStmt, (SQLSMALLINT*) &nCols);  
                pODBCSetInfo[nSet].nCols = nCols;  
                }  
  
            if (GetColumnsInfo(hStmt, pODBCSetInfo[nSet].nCols,  
                &(pODBCSetInfo[nSet].pODBCColInfo)) == SQL_ERROR)  
                {  
                goto EXIT;  
                }  
            }  
  
        // Get memory for bound return values if required.  
        if (pODBCSetInfo[nSet].pRowValues == NULL)  
            {  
            CreateBindBuffer(&(pODBCSetInfo[nSet]));  
            }  
  
        // Rebind columns each time the result set changes.  
        myBindCols(hStmt, pODBCSetInfo[nSet].nCols,  
            pODBCSetInfo[nSet].pODBCColInfo,  
            pODBCSetInfo[nSet].pRowValues);  
  
        // Set for ODBC row array retrieval. Fast retrieve for all  
        //  sets. COMPUTE row sets have only a single row, but  
        //  normal rows can be retrieved in blocks for speed.  
        SQLSetStmtAttr(hStmt, SQL_ATTR_ROW_BIND_TYPE,  
            (void*) pODBCSetInfo[nSet].nResultWidth, SQL_IS_UINTEGER);  
        SQLSetStmtAttr(hStmt, SQL_ATTR_ROW_ARRAY_SIZE,  
            (void*) pODBCSetInfo[nSet].nRows, SQL_IS_UINTEGER);  
        SQLSetStmtAttr(hStmt, SQL_ATTR_ROWS_FETCHED_PTR,  
            (void*) &nRowsFetched, sizeof(SQLINTEGER));  
  
        while (TRUE)  
            {  
            // In ODBC 3.x, SQLFetch supports arrays of bound rows or  
            //  columns. SQLFetchScroll (or ODBC 2.x SQLExtendedFetch)  
            //  is not necessary to support fastest retrieval of   
            //  data rows.  
            if (!SQL_SUCCEEDED(sRet = SQLFetch(hStmt)))  
                {  
                break;  
                }  
  
            for (nRow = 0; nRow < (UINT) nRowsFetched; nRow++)  
                {  
                for (nCol = 0; nCol < pODBCSetInfo[nSet].nCols;  
                        nCol++)  
                    {  
                    // Processing row and column values...  
                    }  
                }  
            }  
  
        // sRet is not SQL_SUCCESS and is not SQL_SUCCESS_WITH_INFO.  
        //  If it's SQL_NO_DATA, then continue. If it's an  
        //  error state, stop.  
        if (sRet != SQL_NO_DATA)  
            {  
            break;  
            }  
  
        // If there's another set waiting, determine the result set  
        //  indicator. The indicator is 0 for regular row sets or an  
        //  ordinal indicating the COMPUTE clause responsible for the  
        //  set.  
        if (SQLMoreResults(hStmt) == SQL_SUCCESS)  
            {  
            sRet = SQLColAttribute(hStmt, 1, SQL_CA_SS_COMPUTE_ID,  
                NULL, 0, NULL, (SQLPOINTER) &nSet);  
            }  
        else  
            {  
            break;  
            }  
        }  
  
EXIT:  
    // Clean-up anything dynamically allocated and return.  
    return;  
    }  
```  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLMoreResults](../../odbc/reference/syntax/sqlmoreresults-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  

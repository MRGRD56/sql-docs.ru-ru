---
description: Использование классификации данных с Microsoft ODBC Driver for SQL Server
title: Использование классификации данных с Microsoft ODBC Driver for SQL Server | Документация Майкрософт
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-makouz
ms.author: v-makouz
manager: kenvh
ms.openlocfilehash: 3b97e6407f541657acee2b15097d3fbb877854a9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837419"
---
# <a name="data-classification"></a>Классификация данных
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Обзор
Для управления конфиденциальными данными в SQL Server и Azure SQL Server появилась возможность предоставлять столбцы базы данных с метаданными чувствительности, что позволяет клиентскому приложению обрабатывать различные типы конфиденциальных данных (например, данные о состоянии здоровья, финансовые данные и т. д.) в соответствии с политикой защиты данных.

Дополнительные сведения о назначении классификации столбцам см. в статье [Обнаружение и классификация данных SQL](../../relational-databases/security/sql-data-discovery-and-classification.md).

Microsoft ODBC Driver 17.2 позволяет получать эти метаданные через SQLGetDescField, используя идентификатор поля SQL_CA_SS_DATA_CLASSIFICATION.

## <a name="format"></a>Формат
Синтаксис SQLGetDescField имеет следующий вид:

```  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```
*DescriptorHandle*  
 [Вход] Дескриптор строки реализации (IRD). Можно получить с помощью вызова SQLGetStmtAttr с атрибутом инструкции SQL_ATTR_IMP_ROW_DESC.
  
 *RecNumber*  
 [Вход] 0
  
 *FieldIdentifier*  
 [Вход] SQL_CA_SS_DATA_CLASSIFICATION.
  
 *ValuePtr*  
 [Выход] Выходной буфер.
  
 *BufferLength*  
 [Вход] Длина выходного буфера в байтах.

 *StringLengthPtr* [Выход] Указатель на буфер, в котором будет возвращаться общее число байтов, доступных для получения из параметра *ValuePtr*.
 
> [!NOTE]
> Если размер буфера неизвестен, его можно определить путем вызова строки SQLGetDescField со значением NULL для параметра *ValuePtr* и проверки значения *StringLengthPtr*.
 
Если сведения о классификации данных недоступны, будет возвращена ошибка *Недопустимое поле дескриптора*.

При успешном вызове SQLGetDescField буфер, на который указывает параметр *ValuePtr*, будет содержать следующие данные:

 `nn nn [n sensitivitylabels] tt tt [t informationtypes] cc cc [c columnsensitivitys]`

> [!NOTE]
> Значения `nn nn`, `tt tt` и `cc cc` являются многобайтовыми целыми числами, которые хранятся с наименее значимым байтом в нижнем адресе.

Значения *`sensitivitylabel`* и *`informationtype`* имеют формат:

 `nn [n bytes name] ii [i bytes id]`

Значение *`columnsensitivity`* имеет формат:

 `nn nn [n sensitivityprops]`

Для каждого столбца *(c)* имеется *n* длиной 4 байта *`sensitivityprops`* .

 `ss ss tt tt`

s — индекс для массива *`sensitivitylabels`* (значение `FF FF`, если он не помечен).

t — индекс для массива *`informationtypes`* (значение `FF FF`, если он не помечен).


<br><br>
Формат данных может выражаться в виде следующих псевдоструктур:

```
struct IDnamePair {
 BYTE nameLen;
 USHORT name[nameLen];
 BYTE idLen;
 USHORT id[idLen];
};

struct SensitivityProp {
 USHORT labelIdx;
 USHORT infoTypeIdx;
};

USHORT nLabels;
struct IDnamePair labels[nLabels];
USHORT nInfoTypes;
struct IDnamePair infotypes[nInfoTypes];
USHORT nColumns;
struct {
 USHORT nProps;
 struct SensitivityProp[nProps];
} columnClassification[nColumns];
```


## <a name="code-sample"></a>Пример кода
Тестовое приложение, которое показывает, как считывать метаданные классификации данных. В Windows его можно скомпилировать с помощью `cl /MD dataclassification.c /I (directory of msodbcsql.h) /link odbc32.lib` и выполнить с использованием строки подключения и SQL-запроса (который возвращает классифицированные столбцы) в качестве параметров:

```
#ifdef _WIN32
#include <windows.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include <msodbcsql.h>
#include <stdio.h>
SQLHANDLE env, dbc, stmt;
void checkRC_exit(SQLRETURN rc, SQLHANDLE hand, SQLSMALLINT htype, int retcode, char *action)
{
    if ((rc == SQL_ERROR || rc == SQL_SUCCESS_WITH_INFO) && hand)
    {
        char msg[1024], state[6];
        int i = 0;
        SQLRETURN rc2;
        SQLINTEGER err;
        SQLSMALLINT lenout;
        while ((rc2 = SQLGetDiagRec(htype, hand, ++i, state, &err, msg, sizeof(msg), &lenout)) == SQL_SUCCESS ||
            rc2 == SQL_SUCCESS_WITH_INFO)
            printf("%d (%d)[%s]%s\n", i, err, state, msg);
    }
    if (rc == SQL_ERROR && retcode)
    {
        printf("Error occurred%s%s\n", action ? " upon " : "", action ? action : "");
        exit(retcode);
    }
}
void printLabelInfo(char *type, char **pptr)
{
    char *ptr = *pptr;
    unsigned short nlabels;
    printf("----- %s(%u) -----\n", type, nlabels = *(unsigned short*)ptr);
    ptr += sizeof(unsigned short);
    while (nlabels--)
    {
        int namelen, idlen;
        char *nameptr, *idptr;
        namelen = *ptr++;
        nameptr = ptr;
        ptr += namelen * 2;
        idlen = *ptr++;
        idptr = ptr;
        ptr += idlen * 2;
        wprintf(L"Name: \"%.*s\" Id: \"%.*s\"\n", namelen, nameptr, idlen, idptr);
    }
    *pptr = ptr;
}
int main(int argc, char **argv)
{
    unsigned char *dcbuf;
    unsigned int dclen = 0;
    SQLRETURN rc;
    SQLHANDLE ird;
    if (argc < 3)
    {
        fprintf(stderr, "usage: dataclassification connstr query\n");
        return 1;
    }
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_ENV, 0, &env), 0, 0,
        2, "allocate environment");
    checkRC_exit(SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0), env, SQL_HANDLE_ENV,
        3, "set ODBC version");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc), env, SQL_HANDLE_ENV,
        4, "allocate connection");
    checkRC_exit(SQLDriverConnect(dbc, 0, argv[1], SQL_NTS, 0, 0, 0, SQL_DRIVER_NOPROMPT), dbc, SQL_HANDLE_DBC,
        5, "connect to server");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt), dbc, SQL_HANDLE_DBC,
        6, "allocate statement");
    checkRC_exit(SQLExecDirect(stmt, argv[2], SQL_NTS), stmt, SQL_HANDLE_STMT,
        7, "execute query");
    checkRC_exit(SQLGetStmtAttr(stmt, SQL_ATTR_IMP_ROW_DESC, (SQLPOINTER)&ird, SQL_IS_POINTER, 0), stmt, SQL_HANDLE_STMT,
        8, "get IRD handle");
    rc = SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, 0, &dclen);
    if (rc != SQL_SUCCESS && rc != SQL_SUCCESS_WITH_INFO)
    {
        checkRC_exit(rc, ird, SQL_HANDLE_DESC, 0, 0);
        printf("Error reading SQL_CA_SS_DATA_CLASSIFICATION IRD field. Ensure that the driver and\n"
            "server both support the Data Classification feature, and that the query returns columns\n"
            "with classification information.\n");
    }
    else
    {
        SQLINTEGER dclenout;
        unsigned char *dcptr;
        unsigned short ncols;
        printf("Data Classification information (%u bytes):\n", dclen);
        if (!(dcbuf = malloc(dclen)))
        {
            printf("Memory Allocation Error");
            return 9;
        }
        checkRC_exit(SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, dclen, &dclenout),
            ird, SQL_HANDLE_DESC, 10, "reading SQL_CA_SS_DATA_CLASSIFICATION");
        dcptr = dcbuf;
        printLabelInfo("Labels", &dcptr);
        printLabelInfo("Information Types", &dcptr);
        printf("----- Column Sensitivities(%u) -----\n", ncols = *(unsigned short*)dcptr);
        dcptr += sizeof(unsigned short);
        while (ncols--)
        {
            unsigned short nprops = *(unsigned short*)dcptr;
            dcptr += sizeof(unsigned short);
            while (nprops--)
            {
                unsigned short labelidx, typeidx;
                labelidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                typeidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                printf(labelidx == 0xFFFF ? "(none) " : "%u ", labelidx);
                printf(typeidx == 0xFFFF ? "(none)\n" : "%u\n", typeidx);
            }
            printf("-----\n");
        }
        if (dcptr != dcbuf + dclen)
        {
            printf("Error: unexpected parse of DATACLASSIFICATION data\n");
            return 11;
        }
        free(dcbuf);
    }
    return 0;
}
```

## <a name="supported-version"></a><a name="bkmk-version"></a>Поддерживаемая версия
Microsoft ODBC Driver 17.2 позволяет получить сведения о классификации данных с помощью `SQLGetDescField`, если `FieldIdentifier` имеет значение `SQL_CA_SS_DATA_CLASSIFICATION` (1237). 

Начиная с версии Microsoft ODBC Driver 17.4.1.1 можно извлечь версию классификации данных, поддерживаемую сервером, с помощью `SQLGetDescField` и идентификатора поля `SQL_CA_SS_DATA_CLASSIFICATION_VERSION` (1238). В версии 17.4.1.1 для поддерживаемой версии классификации данных указано значение 2.

 

Начиная с версии 17.4.2.1 введена версия классификации данных по умолчанию, которая имеет значение 1 и является версией, которую драйвер указывает SQL Server как поддерживаемую. Новый атрибут подключения `SQL_COPT_SS_DATACLASSIFICATION_VERSION` (1400) позволяет приложению изменять поддерживаемую версию классификации данных со значения 1 до максимальной поддерживаемой версии.  

Пример 

Чтобы задать версию, этот вызов требуется сделать прямо перед вызовом SQLConnect или SQLDriverConnect:
```
ret = SQLSetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)2, SQL_IS_INTEGER);
```

Значение поддерживаемой в настоящее время версии классификации данных можно получить с помощью вызова SQLGetConnectAttr: 
```
ret = SQLGetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)&dataClassVersion, SQL_IS_INTEGER, 0);
```
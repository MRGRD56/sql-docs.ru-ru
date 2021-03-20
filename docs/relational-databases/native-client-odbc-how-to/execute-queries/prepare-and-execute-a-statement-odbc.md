---
description: Подготовка и выполнение инструкцию (ODBC)
title: Подготовка и выполнение инструкции (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statement execution
- statement preparation
ms.assetid: 0adecc63-4da5-486c-bc48-09a004a2fae6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 94270caf9aae58a48ec9905062b7f8f317cdfeb3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753724"
---
# <a name="prepare-and-execute-a-statement-odbc"></a>Подготовка и выполнение инструкцию (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-prepare-a-statement-once-and-then-execute-it-multiple-times"></a>Однократная подготовка инструкции и многократное ее выполнение  
  
1.  Вызовите функцию [SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md) , чтобы подготовить инструкцию.  
  
2.  Можно также вызвать метод [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) , чтобы определить количество параметров в подготовленной инструкции.  
  
3.  Выполните следующие действия для каждого параметра подготовленной инструкции (не обязательно).  
  
    -   Вызовите [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) для получения сведений о параметре.  
  
    -   Привяжите каждый параметр к переменной программы с помощью [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md). Присвойте значения всем параметрам времени выполнения.  
  
4.  Перед каждым выполнением подготовленной инструкции.  
  
    -   Если в инструкции имеются маркеры параметров, поместите значения данных в буфер связанного параметра.  
  
    -   Вызовите функцию [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) , чтобы выполнить подготовленную инструкцию.  
  
    -   При использовании входных параметров времени выполнения функция [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) возвращает SQL_NEED_DATA. Отправьте данные по фрагментам при помощи функций [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) и [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md).  
  
### <a name="to-prepare-a-statement-with-column-wise-parameter-binding"></a>Подготовка инструкции с привязкой параметра на уровне столбца  
  
1.  Вызовите функцию [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) , чтобы задать следующие атрибуты.  
  
    -   Атрибуту SQL_ATTR_PARAMSET_SIZE задайте число наборов параметров (S).  
  
    -   Атрибуту SQL_ATTR_PARAM_BIND_TYPE задайте значение SQL_PARAMETER_BIND_BY_COLUMN.  
  
    -   Атрибут SQL_ATTR_PARAMS_PROCESSED_PTR должен указывать на переменную SQLUINTEGER, в которую помещается число обработанных параметров.  
  
    -   Задайте значение SQL_ATTR_PARAMS_STATUS_PTR, которое указывает на массив [S] переменных SQLUSSMALLINT, предназначенный для сохранения признаков состояния параметра.  
  
2.  Вызовите SQLPrepare, чтобы подготовить инструкцию.  
  
3.  Можно также вызвать метод [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) , чтобы определить количество параметров в подготовленной инструкции.  
  
4.  При необходимости для каждого параметра в подготовленной инструкции вызовите SQLDescribeParam, чтобы получить сведения о параметрах.  
  
5.  Для каждого маркера параметра выполните следующее.  
  
    -   Выделите массив из S буферов параметра для сохранения значений данных.  
  
    -   Выделите массив из S буферов параметра для сохранения длин данных.  
  
    -   Вызовите функцию SQLBindParameter , чтобы связать массивы значений данных и длин данных с параметром инструкции.  
  
    -   Если параметр использует данные времени выполнения типов text или image, присвойте ему значение.  
  
    -   Если используются параметры с данными времени выполнения, присвойте им значения.  
  
6.  Перед каждым выполнением подготовленной инструкции.  
  
    -   Поместите значения и длины S-данных в массивы связанных параметров.  
  
    -   Вызовите функцию SQLExecute, чтобы выполнить подготовленную инструкцию.  
  
    -   При использовании входных параметров времени выполнения функция SQLExecute возвращает SQL_NEED_DATA. Отправьте данные по фрагментам при помощи функций SQLParamData и SQLPutData.  
  
### <a name="to-prepare-a-statement-with-row-wise-bound-parameters"></a>Подготовка инструкции с привязкой параметра на уровне строки  
  
1.  Выделите массив структур [S], где S — число наборов параметров. Структура имеет по одному элементу для каждого параметра, а каждый элемент состоит из двух частей.  
  
    -   Первая часть — переменная соответствующего типа данных, в которую помещаются данные параметра.  
  
    -   Вторая часть — переменная SQLINTEGER, в которую помещается признак состояния.  
  
2.  Вызовите функцию [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) , чтобы задать следующие атрибуты.  
  
    -   Атрибуту SQL_ATTR_PARAMSET_SIZE задайте число наборов параметров (S).  
  
    -   Атрибуту SQL_ATTR_PARAM_BIND_TYPE задайте размер структуры, выделенной в шаге 1.  
  
    -   Атрибут SQL_ATTR_PARAMS_PROCESSED_PTR должен указывать на переменную SQLUINTEGER, в которую помещается число обработанных параметров.  
  
    -   Задайте значение SQL_ATTR_PARAMS_STATUS_PTR, которое указывает на массив [S] переменных SQLUSSMALLINT, предназначенный для сохранения признаков состояния параметра.  
  
3.  Вызовите SQLPrepare, чтобы подготовить инструкцию.  
  
4.  Для каждого маркера параметра вызовите SQLBindParameter, чтобы указать значения данных параметра и указатель длины данных на их переменные в первом элементе массива структур, выделенных на шаге 1. Если параметр использует данные времени выполнения, присвойте ему значение.  
  
5.  Перед каждым выполнением подготовленной инструкции.  
  
    -   Заполните массив буфера привязанного параметра значениями данных.  
  
    -   Вызовите функцию SQLExecute, чтобы выполнить подготовленную инструкцию. Драйвер эффективно выполнит инструкцию SQL S раз, по одному разу для каждого набора параметров.  
  
    -   При использовании входных параметров времени выполнения функция SQLExecute возвращает SQL_NEED_DATA. Отправьте данные по фрагментам при помощи функций SQLParamData и SQLPutData.  
  
## <a name="see-also"></a>См. также:  
 [Инструкции по выполнению запросов &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  

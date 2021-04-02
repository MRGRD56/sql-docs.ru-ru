---
description: Аргументы функции Юникода
title: Аргументы функции Юникода | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- Unicode [ODBC], functions
- functions [ODBC], Unicode functions
ms.assetid: eafe8c7e-f6d2-44d7-99ee-cf2148a30f4f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b91a4880cb39fac38f1eec918e861c7aef25787b
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981120"
---
# <a name="unicode-function-arguments"></a>Аргументы функции Юникода
Диспетчер драйверов ODBC 3,5 (или более поздней версии) поддерживает версии ANSI и Юникод всех функций, которые принимают указатели на символьные строки или СКЛПОИНТЕР в своих аргументах. Функции Юникода реализуются как функции (с суффиксом *W*), а не как макросы. Функции ANSI (которые могут вызываться с суффиксом или без *него) идентичны* текущим ФУНКЦИЯМ API ODBC.  
  
## <a name="remarks"></a>Комментарии  
 Для функций Юникода, которые всегда возвращают или принимают строки или аргументы длины, аргументы передаются как количество символов. Для функций, возвращающих сведения о длине данных сервера, размер и точность отображения описаны в разделе число символов. Если длина (размер перемещения данных) может ссылаться на строковые или нестроковые данные, длина описана в октетах. Например, **склжетинфов** будет по-прежнему иметь длину в виде количества байт, но **склексекдиректв** будет использовать количество символов.  
  
 Количество символов — это число байтов (октетов) для функций ANSI и число WCHAR (16-разрядных слов) для функций Юникода. В частности, двойная последовательность символов (DBCS) или многобайтовая кодировка (MBCS) может состоять из нескольких байтов. Последовательность символов Юникода UTF-16 может состоять из нескольких Вчарс.  
  
 Ниже приведен список функций API ODBC, поддерживающих версии Юникода (W) и ANSI (A).  
  
:::row:::
   :::column span="":::
      **SQLBrowseConnect**<br>      **SQLColAttribute**<br>      **SQLColAttributes**<br>      **SQLColumnPrivileges**<br>      **SQLColumns** <br>      **SQLConnect** <br>      **SQLDataSources**<br>      **SQLDescribeCol**  <br>      **SQLDriverConnect** <br>      **SQLDrivers** <br>      **SQLError**  <br>      **SQLExecDirect**<br>      **SQLForeignKeys**<br>      **SQLGetConnectAttr** <br>      **SQLGetConnectOption** <br>      **SQLGetCursorName**<br>      **SQLGetDescField** <br>      **SQLGetDescRec** <br>      **SQLGetDiagField**
   :::column-end:::
   :::column span="":::
      **Функции SQLGetDiagRec**        <br>      **SQLGetInfo**        <br>      **SQLGetStmtAttr**<br>      **SQLGetTypeInfo**<br>      **SQLNativeSql**<br>      **SQLPrepare**<br>      **SQLPrimaryKeys**<br>      **SQLProcedureColumns**<br>      **SQLProcedures**<br>      **SQLSetConnectAttr**<br>      **SQLSetConnectOption**<br>      **SQLSetCursorName**<br>      **SQLSetDescField**<br>      **SQLSetStmtAttr**<br>      **SQLSpecialColumns**<br>      **SQLStatistics**<br>      **SQLTablePrivileges**<br>      **SQLTables**
   :::column-end:::
:::row-end:::
  
 Ниже приведен список функций установщика ODBC и ODBC-переводчиков, поддерживающих версии Юникода (W) и ANSI (A).  
  
:::row:::
   :::column span="":::
      **SQLConfigDataSource**<br>      **склкреатедатасаурце**<br>      **склдатасаурцетодривер**<br>      **склдривертодатасаурце**<br>      **склжетаваилабледриверс**<br>      **склжетинсталледдриверс**<br>      **склжеттранслатор**<br>      **склинсталлдривер**
   :::column-end:::
   :::column span="":::
      **склинсталлдриверманажер**  <br>      **склинсталлереррор**  <br>      **склинсталлодбк**  <br>      **склреадфиледсн**  <br>      **склремоведснфромини**  <br>      **склвалиддсн**  <br>      **склвритедснтоини**
   :::column-end:::
:::row-end:::
  
> [!NOTE]
>  Устаревшие функции имеют поддержку сопоставления Юникода и ANSI, так как диспетчер драйверов ODBC *3. x* поддерживает перекомпиляцию приложений ODBC *2. x* с помощью Юникода **#define**.  
  
 Этот раздел содержит следующие подразделы.  
  
-   [Приложения на базе Юникода](../../../odbc/reference/develop-app/unicode-applications.md)  
  
-   [Драйверы Юникода](../../../odbc/reference/develop-app/unicode-drivers.md)  
  
-   [Сопоставление функции в диспетчере драйверов](../../../odbc/reference/develop-app/function-mapping-in-the-driver-manager.md)

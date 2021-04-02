---
description: Функции ODBC, выполняемые библиотекой курсоров
title: Функции ODBC, выполняемые библиотекой курсоров | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- cursor library [ODBC], functions
- functions [ODBC], cursor library
- ODBC functions [ODBC], cursor library
- ODBC cursor library [ODBC], functions
ms.assetid: 2f1d3386-7e59-4d55-a5b4-3440b61343a3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 68d5c4f12b2b2a3d408e2bd47d5f3c1d138b105e
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980987"
---
# <a name="odbc-functions-executed-by-the-cursor-library"></a>Функции ODBC, выполняемые библиотекой курсоров
> [!IMPORTANT]  
>  Эта функция будет удалена в следующей версии Windows. Избегайте использования этой функции в новых разработках и запланируйте изменение приложений, которые в настоящее время используют эту функцию. Корпорация Майкрософт рекомендует использовать функцию курсора драйвера.  
  
 Библиотека курсоров выполняет следующие функции. Когда приложение вызывает функцию из этого списка, диспетчер драйверов вызывает библиотеку курсоров, а не драйвер. Обратите внимание, что библиотека курсоров может вызывать драйвер при выполнении функции.  
  
:::row:::
   :::column span="":::
      **SQLBindCol**<br>      **склбиндпарам**<br>      **SQLBindParameter**<br>      **SQLCloseCursor**<br>      **SQLEndTran**<br>      **SQLExtendedFetch**<br>      **SQLFetchScroll**<br>      **SQLFreeHandle**<br>      **Функция SQLFreeStmt**<br>      **SQLGetData**<br>      **SQLGetDescField**<br>      **SQLGetDescRec**<br>      **SQLGetFunctions**<br>      **SQLGetInfo**<br>      **SQLGetStmtAttr**
   :::column-end:::
   :::column span="":::
      **SQLGetStmtOption**<br>      **SQLNativeSql**<br>      **SQLNumParams**<br>      **SQLParamOptions**<br>      **SQLRowCount**<br>      **SQLSetConnectAttr**<br>      **SQLSetConnectOption**<br>      **SQLSetDescField**<br>      **SQLSetDescRec**<br>      **функция SQLSetPos;**<br>      **SQLSetScrollOptions**<br>      **SQLSetStmtAttr**<br>      **SQLSetStmtOption**<br>      **SQLTransact**
   :::column-end:::
:::row-end:::

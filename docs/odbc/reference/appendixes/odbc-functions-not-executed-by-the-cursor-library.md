---
description: Функции ODBC, не выполняемые библиотекой курсоров
title: Функции ODBC, не выполняемые библиотекой курсоров | Документация Майкрософт
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
ms.assetid: f2941522-75eb-4db9-9468-4800b884dac2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: db4fac295d8f2426637ca90584df86267eb20dc9
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980876"
---
# <a name="odbc-functions-not-executed-by-the-cursor-library"></a>Функции ODBC, не выполняемые библиотекой курсоров
> [!IMPORTANT]  
>  Эта функция будет удалена в следующей версии Windows. Избегайте использования этой функции в новых разработках и запланируйте изменение приложений, которые в настоящее время используют эту функцию. Корпорация Майкрософт рекомендует использовать функцию курсора драйвера.  
  
 Библиотека курсоров не выполняет следующие функции. Когда приложение вызывает одну из этих функций, диспетчер драйверов вызывает драйвер, а не библиотеку курсоров.  
  
:::row:::
   :::column span="":::
      **SQLFetch**<br>      **SQLGetConnectAttr**<br>      **SQLGetDiagField**<br>      **Функции SQLGetDiagRec**
   :::column-end:::
   :::column span="":::
      **SQLGetEnvAttr**<br>      **SQLSetDescRec**<br>      **SQLSetEnvAttr**  
   :::column-end:::
:::row-end:::


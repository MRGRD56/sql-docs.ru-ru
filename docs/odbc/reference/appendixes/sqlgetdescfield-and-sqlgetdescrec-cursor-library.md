---
description: SQLGetDescField и SQLGetDescRec (библиотека курсоров)
title: SQLGetDescField и SQLGetDescRec (библиотека курсоров) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetDescField function [ODBC], Cursor Library
- SQLGetDescRec function [ODBC], Cursor Library
ms.assetid: 1a801f22-6fea-48aa-a723-3187a2ad852b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ef395940419d593571dc2da6df6323437f2812eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202777"
---
# <a name="sqlgetdescfield-and-sqlgetdescrec-cursor-library"></a>SQLGetDescField и SQLGetDescRec (библиотека курсоров)
> [!IMPORTANT]  
>  Эта функция будет удалена в следующей версии Windows. Избегайте использования этой функции в новых разработках и запланируйте изменение приложений, которые в настоящее время используют эту функцию. Корпорация Майкрософт рекомендует использовать функцию курсора драйвера.  
  
 В этом разделе обсуждается использование функций **SQLGetDescField** и **SQLGetDescRec** в библиотеке курсоров. Общие сведения об этих функциях см. в разделе [Функция SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) и [Функция SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md).  
  
 Библиотека курсоров выполняет **SQLGetDescRec** , чтобы вернуть метаданные для столбцов закладок. Библиотека курсоров выполняет **SQLGetDescField** , чтобы вернуть те же поля, которые возвращаются **SQLGetDescRec**, что SQL_DESC_NAME, SQL_DESC_TYPE, SQL_DESC_DATETIME_INTERVAL_CODE, SQL_DESC_OCTET_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE и SQL_DESC_NULLABLE. Для обеспечения согласованности **SQLGetDescField** также возвращает SQL_DESC_UNNAMED.  
  
 Библиотека курсоров выполняет **SQLGetDescField** при вызове для возвращения значений следующих полей, заданных для привязки столбцов закладки: SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR, SQL_DESC_OCTET_LENGTH_PTR и SQL_DESC_LENGTH.  
  
 Библиотека курсоров выполняет **SQLGetDescField** при вызове для возврата значения поля SQL_DESC_BIND_OFFSET_PTR, SQL_DESC_BIND_TYPE, SQL_DESC_ROW_ARRAY_SIZE или SQL_DESC_ROW_STATUS_PTR. Эти поля могут возвращаться для любой строки, а не только для строки закладки.  
  
 Если приложение вызывает **SQLGetDescField** для возвращения значения любого поля, Кроме упомянутых ранее, Библиотека курсоров передает вызов драйвера.

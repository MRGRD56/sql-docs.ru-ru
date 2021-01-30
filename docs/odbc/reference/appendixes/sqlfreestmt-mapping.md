---
description: Сопоставление SQLFreeStmt
title: Сопоставление SQLFreeStmt | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFreeStmt function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLFreeStmt
ms.assetid: 267d95f2-4f0c-47ab-9411-5afe105215a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a90ddbaab036efd22003b84b551398b0d896226b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202811"
---
# <a name="sqlfreestmt-mapping"></a>Сопоставление SQLFreeStmt
Когда приложение вызывает **SQLFreeStmt** с аргументом *Option* SQL_DROP через драйвер ODBC *3. x* , вызов метода  
  
```  
SQLFreeStmt(hstmt, SQL_DROP)   
```  
  
 сопоставлен с  
  
```  
SQLFreeHandle(SQL_HANDLE_STMT,Handle)  
```  
  
 с аргументом *Handle* , имеющим значение в *хстмт*.

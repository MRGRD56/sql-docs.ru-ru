---
description: Передача данных в двоичной форме
title: Передача данных в двоичной форме | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], transferring in binary form
- transferring data in binary form [ODBC]
- binary data transfers [ODBC]
ms.assetid: 4b12a9de-51d0-416a-87f4-9bf84959cad9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6633395fad5e05a48ae50c35a03e946fba6ecec9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202444"
---
# <a name="transferring-data-in-its-binary-form"></a>Передача данных в двоичной форме
Приложение может безопасно передавать данные (во внутренней форме, используемой указанной СУБД) между двумя источниками данных, использующими одну и ту же СУБД и аппаратную платформу. Для конкретного фрагмента данных типы данных SQL должны быть одинаковыми в исходном и целевом источниках данных. Тип данных C — SQL_C_BINARY.  
  
 Когда приложение вызывает **SQLFetch**, **SQLFetchScroll** или **SQLGetData** для получения данных из источника исходных данных, драйвер извлекает данные из источника данных и передает их без преобразования в место хранения типа SQL_C_BINARY. Когда приложение вызывает **SQLBulkOperations**, **SQLExecute**, **SQLExecDirect**, **SQLPutData или SQLSetPos** для отправки данных в целевой источник данных, драйвер получает данные из места хранения и передает их в целевой источник данных без преобразования.  
  
> [!NOTE]  
>  Приложения, которые передают данные (за исключением двоичных данных), не взаимодействуют между СУБД.  
  
 **Склкопидеск** можно использовать для копирования привязок строк из исходной СУБД в привязки параметров в целевой СУБД.

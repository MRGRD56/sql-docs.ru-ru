---
description: Длительность октета передачи
title: Длина октета при переносе | Документация Майкрософт
ms.custom: ''
ms.date: 10/28/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transfer octet length of data types [ODBC]
- size of data types [ODBC]
- SQL data types [ODBC], column characteristics
- data types [ODBC], transfer octet length
ms.assetid: 9fdc9762-e203-4cff-9212-54f450bf18d9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b910f544be952a18ae2961e9939960820e3805bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202490"
---
# <a name="transfer-octet-length"></a>Длительность октета передачи
Длина октета для столбца — это максимальное число байтов, возвращаемых в приложение, когда данные передаются в тип данных C по умолчанию. Для символьных данных длина октета для обмена не включает пробел для завершающего символа null. Длина октета для столбца может отличаться от числа байтов, необходимых для хранения данных в источнике данных.  
  
 В следующей таблице показана длина октета, определенная для каждого типа данных ODBC SQL.  
  
|Идентификатор типа SQL|Длина|  
|-------------------------|------------|  
|Все символьные типы [a]|Определенная или максимальная длина (для типа переменной) столбца в байтах. Это то же значение, что и поле дескриптора SQL_DESC_OCTET_LENGTH.|  
|SQL_DECIMAL<br />SQL_NUMERIC|Число байтов, необходимое для хранения символьного представления этих данных, если кодировка равна ANSI, и дважды это число, если кодировка UNICODE. Это максимальное число цифр плюс два, поскольку данные возвращаются в виде символьной строки, а для цифр, знака и десятичной запятой требуются символы. Например, длина перемещения столбца, определенного как NUMERIC (10, 3), равна 12.|  
|SQL_TINYINT|1|  
|SQL_SMALLINT|2|  
|SQL_INTEGER|4|  
|SQL_BIGINT| 8 |  
|SQL_REAL|4|  
|SQL_FLOAT|8|  
|SQL_DOUBLE|8|  
|SQL_BIT|1|  
|Все двоичные типы [a]|Число байтов, необходимое для хранения определенных (для фиксированных типов) или максимума (для типов переменных) количества символов.|  
|SQL_TYPE_DATE<br />SQL_TYPE_TIME|6 (размер SQL_DATE_STRUCT или структуры SQL_TIME_STRUCT).|  
|SQL_TYPE_TIMESTAMP|16 (размер структуры SQL_TIMESTAMP_STRUCT).|  
|Все типы данных интервала|34 (размер структуры интервала).|  
|SQL_GUID|16 (размер структуры GUID).|  
| &nbsp; | &nbsp; |

 [a] Если драйвер не может определить длину столбца или параметра для типов переменных, он возвращает SQL_NO_TOTAL.

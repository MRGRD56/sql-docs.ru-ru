---
description: Ограничения типов данных
title: Ограничения типов данных | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], data types
- data types [ODBC], desktop database drivers
- desktop database drivers [ODBC], data types
ms.assetid: 81c4eab7-1f6b-47a0-b940-89d6c6a14dae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9081f52e7cf79613b9021ce7b883a1720da468c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176531"
---
# <a name="data-type-limitations"></a>Ограничения типов данных
Драйверы баз данных Microsoft ODBC для настольных систем накладывают следующие ограничения на типы данных:  
  
|Тип данных|Описание|  
|---------------|-----------------|  
|Все типы данных|Ошибки преобразования типов могут привести к тому, что в затронутом столбце будет установлено значение NULL.|  
|BINARY|Создание ДВОИЧного столбца нулевой длины фактически возвращает 255-байтный ДВОИЧный столбец.|  
|DATE|Тип данных DATE не может быть преобразован в другой тип данных (или сам) функцией CONVERT.|  
|ДЕСЯТИЧное число (точное число)|Не поддерживается.|  
|Типы данных Floating-Point|Число десятичных разрядов в числе чисел с плавающей запятой может быть ограничено числом, заданным в разделе Международная на панели управления Windows.|  
|NUMERIC|Поддерживает максимальную точность и масштаб 28.|  
|timestamp|Тип данных TIMESTAMP не может быть преобразован функцией CONVERT.|  
|TINYINT|Значения TINYINT всегда беззнаковые.|  
|Строки Zero-Length|При использовании dBASE, Microsoft Excel, Paradox или Текстдривер Вставка строки нулевой длины в столбец фактически вставляет значение NULL.|

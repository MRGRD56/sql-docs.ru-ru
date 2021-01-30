---
description: Системные функции
title: Системные функции | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- system functions [ODBC]
- functions [ODBC], system functions
ms.assetid: 36614b4c-e037-43ef-8692-67f4861b144d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 469d77de424b69bf5b3bab0f6cea301ad23e3af2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202512"
---
# <a name="system-functions"></a>Системные функции
В следующей таблице перечислены системные функции, которые включены в набор скалярных функций ODBC. Вызывая **SQLGetInfo** с *типом сведений* SQL_SYSTEM_FUNCTIONS, приложение может определить, какие системные функции поддерживаются драйвером.  
  
 Аргументами, обозначенными как *exp* , может быть имя столбца, результат другой скалярной функции или литерал, где базовый тип данных может быть представлен как SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL, SQL_DOUBLE, SQL_TYPE_DATE, SQL_TYPE_TIME или SQL_TYPE_TIMESTAMP.  
  
 Аргументы, обозначенные как *value* , могут быть литеральной константой, где базовый тип данных может представляться как SQL_NUMERIC, SQL_DECIMAL, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_FLOAT, SQL_REAL, SQL_DOUBLE, SQL_TYPE_DATE, SQL_TYPE_TIME или SQL_TYPE_TIMESTAMP.  
  
 Возвращаемые значения представлены как типы данных ODBC.  
  
|Функция|Описание|  
|--------------|-----------------|  
|**База данных ()**  (ODBC 1,0)|Возвращает имя базы данных, соответствующей обработчику соединения. (Имя базы данных также доступно путем вызова **SQLGetConnectOption** с параметром подключения SQL_CURRENT_QUALIFIER.)|  
|**Ифнулл (** _exp_,_значение_**)**  (ODBC 1,0)|Если *exp* имеет значение null, возвращается *значение* . Если *exp* не равен null, возвращается *exp* . Допустимый тип данных или типы *значения* должны быть совместимы с типом данных *exp*.|  
|**User ()**  (ODBC 1,0)|Возвращает имя пользователя в СУБД. (Имя пользователя также доступно для **SQLGetInfo** путем указания типа данных: SQL_USER_NAME.) Это значение может отличаться от имени входа.|

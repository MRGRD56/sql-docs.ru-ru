---
description: Способы реализации курсоров
title: Как реализуются курсоры | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- ODBC cursors, about ODBC cursors
- ODBC applications, cursors
- cursors [ODBC], about ODBC cursors
ms.assetid: 2b1d7dd4-08a4-43fc-b3eb-70c183d0941f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0eaa1db7e0a1de4a893e95e2ca6c6d7c7e7e761f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438677"
---
# <a name="how-cursors-are-implemented"></a>Способы реализации курсоров
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Приложения ODBC управляют поведением курсора путем задания одного или нескольких атрибутов инструкции перед выполнением инструкции SQL. ODBC может указывать характеристики курсора двумя разными способами.  
  
-   Тип курсора  
  
     Типы курсоров задаются с помощью атрибута SQL_ATTR_CURSOR_TYPE [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md). Типы курсора ODBC бывают с последовательным доступом, статические, управляемые набором ключей, смешанные и динамические. Изначально метод указания курсоров в ODBC заключался в задании типа курсора.  
  
-   Режим работы курсоров  
  
     Поведение курсора задается с помощью атрибутов SQL_ATTR_CURSOR_SCROLLABLE и SQL_ATTR_CURSOR_SENSITIVITY **SQLSetStmtAttr**. Эти атрибуты смоделированы на ключевых словах SCROLL и SENSITIVE, которые в стандартах ISO определены для инструкции DECLARE CURSOR. Два этих параметра ISO появились в ODBC версии 3.0.  
  
 Характеристики курсора ODBC следует указывать с помощью одного из этих методов; при этом предпочтительнее использовать типы курсоров ODBC.  
  
 Помимо установки типа курсора приложения ODBC также задают и другие параметры, например число строк, возвращаемое при каждом извлечении, параметры параллелизма, а также уровни изоляции транзакции. Эти параметры можно задавать для курсоров в стиле ODBC (с последовательным доступом, статических, управляемых набором ключей, смешанных и динамических) или курсоров в стиле ISO (прокручиваемость и чувствительность).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента поддерживает несколько способов физической реализации различных типов курсоров. Некоторые типы курсоров драйвер реализует с помощью результирующего набора [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию; другие курсоры реализуются им как серверные курсоры либо при помощи библиотеки курсоров ODBC.  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Использование результирующих наборов по умолчанию в SQL Server](../../../relational-databases/native-client-odbc-cursors/implementation/using-sql-server-default-result-sets.md)  
  
-   [Использование серверных курсоров](../../../relational-databases/native-client-odbc-cursors/implementation/using-server-cursors.md)  
  
-   [Библиотека курсоров ODBC](../../../relational-databases/native-client-odbc-cursors/implementation/odbc-cursor-library.md)  
  
## <a name="see-also"></a>См. также:  
 [Использование курсоров &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  

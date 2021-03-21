---
description: Позиционированное обновление (ODBC)
title: Позиционированные обновления (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- SQLSetPos function
- SQLSetCursorName function
- ODBC applications, cursors
- cursors [ODBC], positioned updates
- positioned updates [ODBC]
- ODBC cursors, positioned updates
ms.assetid: ff404e02-630f-474d-b5d4-06442b756991
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b75d3ca4e493804325680954375bf3ae2850ca8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756274"
---
# <a name="positioned-updates-odbc"></a>Позиционированное обновление (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC поддерживает два метода выполнения позиционированных обновлений в курсоре:  
  
-   **функция SQLSetPos;**  
  
-   предложение WHERE CURRENT OF.  
  
 Наиболее распространенный подход заключается в использовании функции **SQLSetPos**. Она имеет следующие параметры.  
  
 SQL_POSITION  
 Позиционирует курсор в определенной строке в текущем наборе строк.  
  
 SQL_REFRESH  
 Обновляет программные переменные, привязанные к столбцам результирующего набора, присваивая им новые значения из строки, в которой в настоящий момент позиционирован курсор.  
  
 SQL_UPDATE  
 Обновляет текущую строку в курсоре значениями, хранимыми в программных переменных, которые привязаны к столбцам результирующего набора.  
  
 SQL_DELETE  
 Удаляет текущую строку в курсоре.  
  
 Функцию **SQLSetPos** можно использовать с любым результирующим набором инструкций, когда инструкция обрабатывает атрибуты курсора для использования серверных курсоров. Столбцы результирующего набора должны быть привязаны к переменным программы. После того как приложение выберет строку, она вызывает функцию **SQLSetPos**(SQL_POSTION) для позиционирования курсора в строке. Затем приложение может вызвать функцию SQLSetPos(SQL_DELETE) для удаления текущей строки или изменить значения привязанных переменных программы и вызвать функцию SQLSetPos(SQL_UPDATE) для обновления текущей строки.  
  
 Приложения могут обновлять или удалять любую строку в наборе строк с помощью функции **SQLSetPos**. Вызов **SQLSetPos** — это удобная альтернатива созданию и выполнению инструкции SQL. Функция **SQLSetPos** работает с текущим набором строк и может использоваться только после вызова [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md).  
  
 Размер набора строк задается вызовом [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) с аргументом атрибута SQL_ATTR_ROW_ARRAY_SIZE. В **SQLSetPos** используется новый размер набора строк, но только после вызова **SQLFetch** или **SQLFetchScroll**. Например, если изменяется размер набора строк, вызывается функция **SQLSetPos** , а затем вызывается **SQLFetch** или **SQLFetchScroll** . При вызове функции **SQLSetPos** используется старый размер набора строк, но **SQLFetch** или **SQLFetchScroll** использует новый размер набора строк.  
  
 Первая строка в наборе строк имеет номер 1. Аргумент RowNumber в **SQLSetPos** должен указывать строку в наборе строк; то есть его значение должно находиться в диапазоне от 1 до количества недавно выбранных строк. Оно может быть меньше размера набора строк. Если аргумент RowNumber имеет значение 0, то операция применяется к каждой строке набора строк.  
  
 Операция удаления функции **SQLSetPos** позволяет источнику данных удалить одну или несколько выбранных строк таблицы. Чтобы удалить строки с помощью функции **SQLSetPos**, приложение вызывает функцию **SQLSetPos** с набором операций SQL_DELETE, а функция RowNumber задает номер удаляемой строки. Если аргумент RowNumber имеет значение 0, то из набора строк удаляются все строки.  
  
 После возврата функции **SQLSetPos** удаленная строка будет текущей строкой, а ее состояние будет SQL_ROW_DELETED. Строку нельзя использовать в дополнительных позиционированных операциях, например в вызовах [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) или **SQLSetPos**.  
  
 При удалении всех строк набора строк (RowNumber равен 0) приложение может запретить драйверу удалять определенные строки с помощью массива операций строк, как и для операции обновления **SQLSetPos**.  
  
 Каждая удаляемая строка должна существовать в результирующем наборе. Если буферы приложения заполняются выборкой, а массив состояния строк сохраняется, то значения каждой из этих позиций строк не должны иметь значение SQL_ROW_DELETED, SQL_ROW_ERROR, или SQL_ROW_NOROW.  
  
 Позиционированные обновления также можно выполнить с помощью предложения WHERE CURRENT OF инструкций UPDATE, DELETE и INSERT. Если для CURRENT из требуется имя курсора, создаваемое ODBC при вызове функции [SQLGetCursorName](../../relational-databases/native-client-odbc-api/sqlgetcursorname.md) , или которое можно указать путем вызова **SQLSetCursorName**. Для обновления с помощью предложения WHERE CURRENT OF в предложении ODBC используются следующие основные шаги.  
  
-   Вызовите **SQLSetCursorName** , чтобы установить имя курсора для маркера инструкции.  
  
-   Создайте инструкцию SELECT с предложением FOR UPDATE OF и выполните ее.  
  
-   Вызовите **SQLFetchScroll** , чтобы получить набор строк или **SQLFetch** для получения строки.  
  
-   Вызовите функцию **SQLSetPos** (SQL_POSITION), чтобы поместить курсор в строку.  
  
-   Создайте и выполните инструкцию UPDATE с предложением WHERE CURRENT OF, используя имя курсора, заданное с помощью **SQLSetCursorName**.  
  
 Кроме того, можно вызвать **SQLGetCursorName** после выполнения инструкции SELECT вместо вызова **SQLSetCursorName** перед выполнением инструкции SELECT. **SQLGetCursorName** возвращает имя курсора по умолчанию, назначаемое ODBC, если не задано имя курсора с помощью **SQLSetCursorName**.  
  
 Параметр **SQLSetPos** предпочтительнее, чем при использовании серверных курсоров. Если используется статический обновляемый курсор с библиотекой курсоров ODBC, то данная библиотека реализует обновления предложения WHERE CURRENT OF путем добавления предложения WHERE с ключевыми значениями базовой таблицы. Это может вызвать непреднамеренные обновления, если ключи в таблице не являются уникальными.  
  
## <a name="see-also"></a>См. также:  
 [Использование курсоров &#40;ODBC&#41;](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  

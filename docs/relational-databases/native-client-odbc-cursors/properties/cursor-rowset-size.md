---
description: Размер набора строк курсора
title: Размер набора строк курсора | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], rowset size
- ODBC cursors, rowset size
- rowsets [ODBC]
ms.assetid: 2febe2ae-fdc1-490e-a79f-c516bc8e7c3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5d135dc225e33fa28680b0ff7e74cc81346f1642
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756184"
---
# <a name="cursor-rowset-size"></a>Размер набора строк курсора
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возможности курсоров ODBC не ограничены выбором только одной строки за один раз. Они могут получать несколько строк в каждом вызове **SQLFetch** или [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md). При работе с базой данных Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в режиме клиент-сервер значительно эффективнее извлекать несколько строк за один раз. Число строк, возвращаемых при выборке, называется размером набора строк и задается с помощью SQL_ATTR_ROW_ARRAY_SIZE [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
```  
SQLUINTEGER uwRowsize;  
SQLSetStmtAttr(m_hstmt, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER)uwRowsetSize, SQL_IS_UINTEGER);  
```  
  
 Курсоры, имеющие размер набора строк более 1, называются блочными курсорами.  
  
 Есть два параметра привязки результирующего набора строк для блочных курсоров.  
  
-   Привязка на уровне столбца  
  
     Каждый столбец привязывается к массиву переменных. Число элементов в каждом массиве равно размеру набора строк.  
  
-   Привязка на уровне строки  
  
     Массив создается с использованием структур, которые содержат данные и индикаторы для всех столбцов в строке. Число структур в массиве равно размеру набора строк.  
  
 При использовании привязки на уровне столбца или на уровне строки каждый вызов **SQLFetch** или **SQLFetchScroll** заполняет привязанные массивы данными из полученного набора строк.  
  
 [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) также можно использовать для извлечения данных столбца из блочного курсора. Поскольку **SQLGetData** работает по одной строке за раз, необходимо вызвать функцию **SQLSetPos** , чтобы задать определенную строку в наборе строк в качестве текущей строки перед вызовом **SQLGetData**.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента обеспечивает оптимизацию с помощью наборов строк для быстрого получения всего результирующего набора. Чтобы использовать эту оптимизацию, задайте для атрибутов курсора значения по умолчанию ("только вперед", "только для чтения", размер набора строк = 1) во время вызова **SQLExecDirect** или **SQLExecute** . [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента устанавливает результирующий набор по умолчанию. Это более эффективно, чем использование серверных курсоров при передаче результатов клиенту без прокрутки. После выполнения инструкции увеличьте размер набора строк и примените привязку на уровне столбца или строки. Это позволяет [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использовать результирующий набор по умолчанию для эффективного отправки результирующих строк клиенту, в то время как [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента непрерывно извлекает строки из сетевых буферов на клиенте.  
  
## <a name="see-also"></a>См. также:  
 [Свойства курсора](../../../relational-databases/native-client-odbc-cursors/properties/cursor-properties.md)  
  
  

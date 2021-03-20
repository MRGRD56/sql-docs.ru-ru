---
description: Назначение хранилища
title: Назначение хранилища | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- column-wise binding [ODBC]
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- SQLBindCol function
- storing data [ODBC]
- result sets [ODBC], storage
- SQL Server Native Client ODBC driver, data storage
- row-wise binding
- binding result sets [SQL Server Native Client]
- array binding
ms.assetid: 11c81955-5300-495f-925f-9256f2587b58
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9fc7a4bd577783042b0b234f04bd83f8c1f0e030
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752794"
---
# <a name="assigning-storage"></a>Назначение хранилища
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Приложение может назначить хранилище для результатов перед выполнением инструкции SQL или после него. Если приложение сначала подготавливает или выполняет инструкцию SQL, оно может запросить о результирующем наборе перед назначением хранилища для результатов. Например, если результирующий набор неизвестен, приложение должно получить количество столбцов, прежде чем им можно будет назначить хранилище.  
  
 Чтобы связать хранилище для столбца данных, приложение вызывает [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)и передает его:  
  
-   Тип данных, в который будут преобразованы данные.  
  
-   Адрес выходного буфера для данных.  
  
     Приложение должно выделить этот буфер, причем буфер должен иметь достаточный размер для данных в той форме, в которую они будут преобразованы.  
  
-   Длину выходного буфера.  
  
     Это значение пропускается, если возвращаемые данные имеют фиксированную длину в C, например целое число, вещественное число или структура данных.  
  
-   Адрес буфера хранения, в который возвращается определенное число байтов доступных данных.  
  
 Приложение также может привязать столбцы результирующего набора к массивам переменных программы, обеспечивая поддержку получения строк результирующего набора поблочно. Существуют два различных типа привязки массивов.  
  
-   Привязка на уровне столбца завершается, когда каждый столбец привязан к собственному массиву переменных.  
  
     Привязка на уровне столбца задается путем вызова [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) с *атрибутом* , для которого задано значение SQL_ATTR_ROW_BIND_TYPE, а *ValuePtr* — значение SQL_BIND_BY_COLUMN. Все массивы должны содержать одинаковое количество элементов.  
  
-   Привязка на уровне строки завершается, когда все параметры в инструкции SQL привязаны как единое целое к массиву структур, которые содержат отдельные переменные для параметров.  
  
     Привязка на уровне строки задается путем вызова **SQLSetStmtAttr** с *атрибутом* , имеющим значение SQL_ATTR_ROW_BIND_TYPE, а *ValuePtr* задает размер структуры, содержащей переменные, которые будут принимать столбцы результирующего набора.  
  
 Приложение также указывает для атрибута SQL_ATTR_ROW_ARRAY_SIZE значение, равное количеству элементов в массивах столбцов или строк, и устанавливает значения SQL_ATTR_ROW_STATUS_PTR и SQL_ATTR_ROWS_FETCHED_PTR.  
  
## <a name="see-also"></a>См. также:  
 [Обработка результатов &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  

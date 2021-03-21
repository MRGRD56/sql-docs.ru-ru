---
description: Результаты обработки — получение сведений о результирующем наборе
title: Получение сведений о результирующем наборе (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- result sets [ODBC]
- result sets [ODBC], fetching
ms.assetid: 34f235e4-f80b-4123-8764-9deb18506f14
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2413dac3a2430303356a8ff0b34d9af7c3a0d945
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751904"
---
# <a name="processing-results---retrieve-result-set-information"></a>Результаты обработки — получение сведений о результирующем наборе
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-get-information-about-a-result-set"></a>Получение сведений о результирующем наборе  
  
1.  Вызовите [SQLNumResultCols](../../relational-databases/native-client-odbc-api/sqlnumresultcols.md) , чтобы получить количество столбцов в результирующем наборе.  
  
2.  Для каждого столбца в результирующем наборе:  
  
    -   Вызовите [SQLDescribeCol](../../relational-databases/native-client-odbc-api/sqldescribecol.md) , чтобы получить сведения о столбце Result.  
  
     либо  
  
    -   Вызовите [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md) , чтобы получить конкретные сведения о дескрипторе столбца результатов.  
  
## <a name="see-also"></a>См. также:  
[Обработка результатов &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/processing-results-process-results.md)

[Определение характеристик результирующего набора &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/determining-the-characteristics-of-a-result-set-odbc.md)  
  
  

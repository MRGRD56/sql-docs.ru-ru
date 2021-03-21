---
description: Создание пакетной обработки вызовов хранимых процедур
title: Пакетирование вызовов хранимых процедур | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], batching
- ODBC, stored procedures
- SQL Server Native Client ODBC driver, stored procedures
- batches [ODBC]
- ODBC CALL escape sequence
ms.assetid: b7f53e11-15f0-4602-8134-b166160888f0
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c0f585919b2bfb5cce895dd75bdd99f500290849
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752724"
---
# <a name="batching-stored-procedure-calls"></a>Создание пакетной обработки вызовов хранимых процедур
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента автоматически выполняет пакетные вызовы хранимых процедур на сервере, если это уместно. Драйвер производит это действие только при использовании управляющей последовательности ODBC CALL и не производит его для инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] EXECUTE. Создание пакетов вызовов хранимых процедур может уменьшить количество обменов данных с сервером и значительно повысить производительность.  
  
 Драйвер создает пакеты вызовов процедур на сервере при выполнении пакета, который содержит несколько управляющих последовательностей ODBC CALL. Он также создает пакеты вызовов процедур, когда массивы связанных параметров используются с управляющей последовательностью ODBC CALL. Например, если для привязки массива с пятью элементами к параметрам инструкции SQL CALL, то при вызове **SQLExecute** или **SQLExecDirect** драйвер отправляет один пакет с пятью вызовами процедур на сервер, при использовании привязки параметра на уровне строк или на уровне столбца.  
  
## <a name="see-also"></a>См. также:  
 [Выполнение хранимых процедур](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)  
  
  

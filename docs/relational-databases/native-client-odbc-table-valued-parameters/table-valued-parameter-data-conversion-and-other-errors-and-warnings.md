---
description: Ошибки и предупреждения преобразования данных возвращающих табличное значение параметров и другие
title: Table-Valued преобразование данных параметра
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), data conversion
- table-valued parameters (ODBC), error messages
ms.assetid: edd45234-59dc-4338-94fc-330e820cc248
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4e28b47009e094a7fa82fac7d106aee966d59e40
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748262"
---
# <a name="table-valued-parameter-data-conversion-and-other-errors-and-warnings"></a>Ошибки и предупреждения преобразования данных возвращающих табличное значение параметров и другие
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Значения столбцов возвращающих табличные значения параметров могут преобразовываться из клиентских типов данных в серверные и обратно таким же образом, как и значения других столбцов и параметров. Но поскольку возвращающий табличное значение параметр может содержать несколько столбцов и несколько строк, важно иметь возможность идентификации фактического значения там, где возникла ошибка.  
  
 При обнаружении ошибки или предупреждения в столбце параметра, возвращающего табличное значение, собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] формирует диагностическую запись. Сообщение об ошибке содержит номер возвращающего табличное значение параметра, а также порядковый номер столбца и номер строки. Приложение может также использовать диагностические поля SQL_DIAG_SS_TABLE_COLUMN_NUMBER и SQL_DIAG_SS_TABLE_ROW_NUMBER внутри диагностических записей для определения того, какие значения ассоциируются с ошибками и предупреждениями. Эти диагностические поля доступны в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях.  
  
 Во всех прочих отношениях SQLSTATE и компоненты сообщений диагностических записей соответствуют существующим нормам функционирования ODBC. То есть, за исключением сведений о параметрах, строках и столбцах, сообщения об ошибках имеют одинаковые значения для возвращающих табличное значение параметров, так же как и для параметров, не возвращающих табличное значение.  
  
## <a name="see-also"></a>См. также:  
 [Возвращающие табличное значение параметры &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  

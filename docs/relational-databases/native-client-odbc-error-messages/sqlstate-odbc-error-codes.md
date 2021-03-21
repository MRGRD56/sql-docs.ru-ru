---
title: SQLSTATE (коды ошибок ODBC) | Документация Майкрософт
description: Когда SQL Server драйвер ODBC запускает хранимые процедуры в качестве удаленных хранимых процедур, процедура может иметь целочисленные коды возврата и выходные параметры.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, errors
- ODBC error handling, cause information
- messages [ODBC], cause information
- SQLSTATEs
- errors [ODBC], cause information
ms.assetid: 84cce528-edb0-473f-a85f-3eb87fbe2cf3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c269c59557f0673d7b8c733e01c6a294a7c2a666
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753054"
---
# <a name="sqlstate-odbc-error-codes"></a>SQLSTATE (коды ошибок ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Код SQLSTATE предоставляет подробные сведения о причине предупреждения или ошибки. Для ошибок, возникающих в источнике данных, обнаруженном и возвращенном [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента сопоставляет возвращенный номер собственной ошибки с соответствующим SQLSTATE. Если машинный номер ошибки не имеет кода ошибки ODBC для сопоставлений, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента возвращает значение SQLSTATE 42000 ("Синтаксическая ошибка или нарушение прав доступа"). Для ошибок, обнаруженных драйвером, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента создает соответствующее значение SQLSTATE.  
  
 Дополнительные сведения о кодах ошибок состояния см. в следующих разделах.  
  
-   [Приложение А. Коды ошибок ODBC](../../odbc/reference/appendixes/appendix-a-odbc-error-codes.md)  
  
-   [Сопоставления SQLSTATE](../../odbc/reference/develop-app/sqlstate-mappings.md)  
  
## <a name="see-also"></a>См. также:  
 [Обработка ошибок и сообщений](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  

---
description: sp_unprepare (Transact-SQL)
title: sp_unprepare (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_cursor_unprepare_TSQL
- sp_cursor_unprepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unprepare
ms.assetid: 14320251-c551-49d8-b933-057406114978
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 035277942891af31b6d4eac980be2ed653ec703f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480795"
---
# <a name="sp_unprepare-transact-sql"></a>sp_unprepare (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Отменяет план выполнения, созданный хранимой процедурой sp_prepare. sp_unprepare вызывается путем указания ID = 15 в пакете потока табличных данных (TDS).  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_unprepare handle           
```  
  
## <a name="arguments"></a>Аргументы  
 *справиться*  
 Значение *обработчика* , возвращаемое sp_prepare.  
  
## <a name="examples"></a>Примеры  
 Следующий пример показывает, как подготовить, выполнить и отменить подготовку простой инструкции.  
  
```SQL  
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@P1 NVARCHAR(128), @P2 NVARCHAR(100)',  
    N'SELECT database_id, name FROM sys.databases WHERE name = @P1 AND state_desc = @P2';  
EXEC sp_execute @P1, N'tempdb', N'ONLINE';  
EXEC sp_unprepare @P1;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Следующий пример показывает, как подготовить, выполнить и отменить подготовку простой инструкции.  
  
```SQL  
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@P1 NVARCHAR(128), @P2 NVARCHAR(100)',  
    N'SELECT database_id, name FROM sys.databases WHERE name = @P1 AND state_desc = @P2';  
EXEC sp_execute @P1, N'tempdb', N'ONLINE';  
EXEC sp_unprepare @P1;  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_prepare &#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   


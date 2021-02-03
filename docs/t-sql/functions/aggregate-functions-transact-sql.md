---
description: Агрегатные функции (Transact-SQL)
title: Агрегатные функции (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], aggregate
- aggregate functions [SQL Server], about aggregate functions
- summarizing functions
- aggregate functions [SQL Server]
ms.assetid: 0c06ae42-eb0a-4d77-9d74-aa1e7f344009
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b51617ece2e2de35d20a262ccd5de011a2180497
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204698"
---
# <a name="aggregate-functions-transact-sql"></a>Агрегатные функции (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Агрегатная функция выполняет вычисление на наборе значений и возвращает одиночное значение. Агрегатные функции, за исключением `COUNT(*)`, не учитывают значения NULL. Агрегатные функции часто используются в выражении GROUP BY инструкции SELECT.
  
Все агрегатные функции являются детерминированными. Другими словами, агрегатные функции возвращают одну и ту же величину при каждом их вызове на одном и том же наборе входных значений. Дополнительные сведения о детерминированности функций см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md). [Предложение OVER](../../t-sql/queries/select-over-clause-transact-sql.md) может следовать за всеми агрегатными функциями, кроме STRING_AGG, GROUPING или GROUPING_ID.
  
Агрегатные функции можно использовать в качестве выражений только в следующих случаях.
-   Список выбора инструкции SELECT (вложенный или внешний запрос).  
-   Предложение HAVING.  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] предоставляет следующие агрегатные функции.

- [APPROX_COUNT_DISTINCT](../../t-sql/functions/approx-count-distinct-transact-sql.md)
- [AVG](../../t-sql/functions/avg-transact-sql.md)
- [CHECKSUM_AGG](../../t-sql/functions/checksum-agg-transact-sql.md)
- [COUNT](../../t-sql/functions/count-transact-sql.md)
- [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md)
- [GROUPING](../../t-sql/functions/grouping-transact-sql.md)
- [GROUPING_ID](../../t-sql/functions/grouping-id-transact-sql.md)
- [MAX](../../t-sql/functions/max-transact-sql.md)
- [MIN](../../t-sql/functions/min-transact-sql.md)
- [STDEV](../../t-sql/functions/stdev-transact-sql.md)
- [STDEVP](../../t-sql/functions/stdevp-transact-sql.md)
- [STRING_AGG](../../t-sql/functions/string-agg-transact-sql.md)
- [SUM](../../t-sql/functions/sum-transact-sql.md)
- [VAR](../../t-sql/functions/var-transact-sql.md)
- [VARP](../../t-sql/functions/varp-transact-sql.md)
  
## <a name="see-also"></a>См. также раздел
[Встроенные функции (Transact-SQL)](../../t-sql/functions/functions.md)  
[Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  

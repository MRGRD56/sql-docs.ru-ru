---
description: Функции работы с курсорами (Transact-SQL)
title: Функции работы с курсорами (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], cursors
- cursor functions
ms.assetid: 7d9daa10-4c50-4212-9400-42120222b2b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: c08ca5b485a33a31c67b2aa532c113764025a5c9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184079"
---
# <a name="cursor-functions-transact-sql"></a>Функции работы с курсорами (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Следующие скалярные функции возвращают сведения о курсорах:
  
- [@@CURSOR_ROWS](../../t-sql/functions/cursor-rows-transact-sql.md)
- [@@FETCH_STATUS](../../t-sql/functions/fetch-status-transact-sql.md)
- [CURSOR_STATUS](../../t-sql/functions/cursor-status-transact-sql.md)
  
Все функции работы с курсорами являются недетерминированными. Это означает, что такие функции не всегда возвращают одинаковые значения при каждом выполнении, даже если наборы входных значений одинаковы. Дополнительные сведения о детерминированности функций см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).
  
## <a name="see-also"></a>См. также раздел

[Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)

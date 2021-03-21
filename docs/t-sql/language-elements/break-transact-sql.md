---
description: BREAK (Transact-SQL)
title: BREAK (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BREAK
- BREAK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- exiting innermost loop [SQL Server]
- END keyword
- ignored statements
- BREAK keyword
ms.assetid: 67c30b8d-3f15-41ad-b9a9-a4ced3b2af9f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a387b5e6319ca4236c8eaa03ee2a357a98a329f9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752364"
---
# <a name="break-transact-sql"></a>BREAK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Оператор BREAK прерывает и выходит из текущего цикла WHILE. Если текущий цикл WHILE вложен в другой цикл, то оператор BREAK прерывает только текущий цикл, а управление передается следующему оператору во внешнем цикле.

Оператор BREAK обычно располагают внутри инструкции IF.

## <a name="examples"></a>Примеры

```sql
WHILE (1=1)
BEGIN
   IF EXISTS (SELECT * FROM ##MyTempTable WHERE EventCode = 'Done')
   BEGIN
      BREAK;  -- 'Done' row has finally been inserted and detected, so end this loop.
   END

   PRINT N'The other process is not yet done.';  -- Re-confirm the non-done status to the console.
   WAITFOR DELAY '00:01:30';  -- Sleep for 90 seconds.
END
```

## <a name="see-also"></a>См. также:

- [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md)
- [WHILE &#40;Transact-SQL&#41;](../../t-sql/language-elements/while-transact-sql.md)
- [IF...ELSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/if-else-transact-sql.md)


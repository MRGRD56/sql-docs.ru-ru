---
description: = (оператор присваивания) (Transact-SQL)
title: = (оператор присваивания) (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], assignment
- assignment operators [Transact-SQL]
- headings [SQL Server columns]
- relationships [SQL Server], assignment operators
- column headings [SQL Server]
ms.assetid: c3040db6-21d6-40ac-a783-82c98ec006cc
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0b391e5cbc4279c029b6b0c40a282b6e9d8a045b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751524"
---
# <a name="-assignment-operator-transact-sql"></a>= (оператор присваивания) (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-all_md](../../includes/tsql-appliesto-ss2012-all-md.md)]

  Знак равенства (=) является единственным оператором присваивания в языке [!INCLUDE[tsql](../../includes/tsql-md.md)]. В следующем примере создается переменная `@MyCounter`, а затем оператор присваивания устанавливает `@MyCounter` в значение выражения.  
  
```sql  
DECLARE @MyCounter INT;  
SET @MyCounter = 1;  
```  
  
 Оператор присваивания может также использоваться для установления связи между заголовком столбца и выражением, которое определяет значение для столбца. Следующий пример отображает заголовки столбцов `FirstColumnHeading` и `SecondColumnHeading`. Строка `xyz` выводится в заголовке столбца `FirstColumnHeading` для всех строк. Затем все коды продуктов из таблицы `Product` перечисляются в заголовке столбца `SecondColumnHeading`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstColumnHeading = 'xyz',  
       SecondColumnHeading = ProductID  
FROM Production.Product;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [Составные операторы (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  

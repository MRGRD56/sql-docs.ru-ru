---
description: Операторы сравнения (Transact-SQL)
title: Операторы сравнения (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], testing
- operators [Transact-SQL], comparison
- testing expressions
- Boolean data type
- Boolean expressions
- comparing expressions
- comparison operators [SQL Server]
ms.assetid: b0cc68ef-3029-484c-a917-0c15dcbc230d
author: rothja
ms.author: jroth
ms.openlocfilehash: 34df520dcb0f193e2b548a909ea10cd4113995fc
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124481"
---
# <a name="comparison-operators-transact-sql"></a>Операторы сравнения (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Операторы сравнения позволяют проверить, одинаковы ли два выражения. Операторы сравнения можно применять ко всем выражениям, за исключением выражений типов **text**, **ntext** и **image**. Операторы сравнения [!INCLUDE[tsql](../../includes/tsql-md.md)] приведены в следующей таблице:  
  
|Оператор|Значение|  
|--------------|-------------|  
|[= (равно)](../../t-sql/language-elements/equals-transact-sql.md)|Равно|  
|[> (больше)](../../t-sql/language-elements/greater-than-transact-sql.md)|Больше|  
|[< (меньше)](../../t-sql/language-elements/less-than-transact-sql.md)|Меньше чем|  
|[>= (больше или равно)](../../t-sql/language-elements/greater-than-or-equal-to-transact-sql.md)|Больше или равно|  
|[<= (меньше или равно)](../../t-sql/language-elements/less-than-or-equal-to-transact-sql.md)|Меньше или равно|  
|[<> (не равно)](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)|Не равно|  
|[!= (не равно)](../../t-sql/language-elements/not-equal-to-transact-sql-exclamation.md)|Не равно (не определено стандартом ISO)|  
|[\!< (не меньше)](../../t-sql/language-elements/not-less-than-transact-sql.md)|Не меньше (не определено стандартом ISO)|  
|[\!> (не больше чем)](../../t-sql/language-elements/not-greater-than-transact-sql.md)|Не больше (не определено стандартом ISO)|  
  
## <a name="boolean-data-type"></a>Логический тип данных  
 Результат выполнения оператора сравнения имеет тип данных **Boolean**. Он имеет три значения: TRUE, FALSE и UNKNOWN. Выражения, возвращающие значения типа **Boolean**, называются логическими.  
  
 В отличие от других типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], тип **Boolean** не может быть типом столбца таблицы или переменной и не может быть возвращен в результирующем наборе.  
  
 Если параметр SET ANSI_NULLS имеет значение ON, оператор, в число операндов которого входит хотя бы одно выражение NULL, возвращает UNKNOWN. Если параметр SET ANSI_NULLS имеет значение OFF, применяются те же правила, за исключением операторов равенства (=) и неравенства (<>). Если параметр SET ANSI_NULLS имеет значение OFF, эти операторы обрабатывают значение NULL как известное значение, эквивалентное любым другим значениям NULL, и возвращают только значение TRUE или FALSE (и никогда UNKNOWN).  
  
 Выражения со значениями типа **Boolean** используются в предложении WHERE для фильтрации строк, удовлетворяющих условиям поиска, и в инструкциях языка управления потоком, таких как IF и WHILE, например:  
  
```syntaxsql  
-- Uses AdventureWorks  
  
DECLARE @MyProduct INT;  
SET @MyProduct = 750;  
IF (@MyProduct <> 0)  
   SELECT ProductID, Name, ProductNumber  
   FROM Production.Product  
   WHERE ProductID = @MyProduct;  
```  
  
## <a name="see-also"></a>См. также  
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)  
  
  

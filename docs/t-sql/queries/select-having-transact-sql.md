---
description: SELECT — HAVING (Transact-SQL)
title: HAVING (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HAVING
- HAVING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- restricting conditions for result set
- search conditions [SQL Server], HAVING clause
- HAVING clause
- HAVING clause, about HAVING clause
ms.assetid: 55650709-001e-42f4-902f-ead09a3c34af
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d3e800a6a8ae3fc3ad6cbf3a1b28eeb6b4ba93cc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751544"
---
# <a name="select---having-transact-sql"></a>SELECT — HAVING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Определяет условие поиска для группы или статистического выражения. Предложение HAVING можно использовать только в инструкции SELECT. HAVING обычно используется с предложением GROUP BY. Если предложение GROUP BY не используется, имеется одна неявная агрегированная группа.   
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
[ HAVING <search condition> ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
\<search_condition> задает один предикат или несколько, которые должны выполняться для групп или агрегатов. Дополнительные сведения об условиях поиска и предикатах см. в статье [Условие поиска (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md).  
  
 Типы данных **text**, **image** и **ntext** нельзя использовать в предложении HAVING.  
  
## <a name="examples"></a>Примеры  
 В следующем примере, который использует простое предложение `HAVING`, из таблицы `SalesOrderID` извлекается сумма всех полей `SalesOrderDetail`, значение которых превышает `$100000.00`.  
  
```sql
USE AdventureWorks2012 ;  
GO  
SELECT SalesOrderID, SUM(LineTotal) AS SubTotal  
FROM Sales.SalesOrderDetail  
GROUP BY SalesOrderID  
HAVING SUM(LineTotal) > 100000.00  
ORDER BY SalesOrderID ;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В приведенном ниже примере предложение `HAVING` используется с целью получения общей суммы `SalesAmount`, превышающей `80000` для каждого `OrderDateKey` из таблицы `FactInternetSales`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales  
GROUP BY OrderDateKey   
HAVING SUM(SalesAmount) > 80000  
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>См. также:  
 [GROUP BY (Transact-SQL)](../../t-sql/queries/select-group-by-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  


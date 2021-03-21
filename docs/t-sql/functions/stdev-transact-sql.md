---
description: STDEV (Transact-SQL)
title: STDEV (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STDEV_TSQL
- STDEV
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], statistical standard deviation
- STDEV function [Transact-SQL]
- statistical standard deviation
ms.assetid: ff41b4fc-4f71-4f18-bf78-96614ea908cc
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 226f25ba42c1dc27f1e85ebe552047de2809b807
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748241"
---
# <a name="stdev-transact-sql"></a>STDEV (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает статистическое стандартное отклонение всех значений в указанном выражении.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql    
-- Aggregate Function Syntax   
STDEV ( [ ALL | DISTINCT ] expression )  
  
-- Analytic Function Syntax   
STDEV ([ ALL ] expression) OVER ( [ partition_by_clause ] order_by_clause)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 **ALL**  
 Применяет функцию ко всем значениям. ALL является параметром по умолчанию.  
  
 DISTINCT  
 Указывает, что учитывается каждое уникальное значение.  
  
 *expression*  
 Числовое [выражение](../../t-sql/language-elements/expressions-transact-sql.md). Агрегатные функции и вложенные запросы не допускаются. *expression* — выражение категории точного числового или приблизительного числового типа данных, за исключением типа данных **bit**.  
  
 OVER **(** [ *partition_by_clause* ] _order\_by\_clause_ **)**  
 _partition\_by\_clause_ делит результирующий набор, полученный с помощью предложения FROM, на секции, к которым применяется функция. Если этот параметр не указан, функция обрабатывает все строки результирующего набора запроса как отдельные группы. _order\_by\_clause_ определяет логический порядок, в котором выполняется операция. _order\_by\_clause_ — это обязательный элемент. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **float**  
  
## <a name="remarks"></a>Комментарии  
 Если функция STDEV используется для всех элементов в инструкции SELECT, то в вычисление включается каждое значение результирующего набора. Функцию STDEV можно использовать только для числовых столбцов. Значения NULL пропускаются.  
  
 STDEV — это детерминированная функция, если она используется без предложений OVER и ORDER BY. Она не детерминирована при использовании с предложениями OVER и ORDER BY. Дополнительные сведения см. в разделе [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-stdev"></a>А. Использование функции STDEV  
 Следующий пример возвращает стандартное отклонение для всех дополнительных значений в таблице `SalesPerson` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT STDEV(Bonus)  
FROM Sales.SalesPerson;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-using-stdev"></a>Б. Использование функции STDEV  
 В приведенном ниже примере возвращается стандартное отклонение значений квот на продажу в таблице `dbo.FactSalesQuota`. Первый столбец содержит стандартное отклонение всех уникальных значений, а второй — стандартное отклонение всех значений, включая повторяющиеся.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT STDEV(DISTINCT SalesAmountQuota)AS Distinct_Values, STDEV(SalesAmountQuota) AS All_Values  
FROM dbo.FactSalesQuota;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Distinct_Values   All_Values
----------------  ----------------
398974.27         398450.57
 ```  
  
### <a name="c-using-stdev-with-over"></a>В. Использование STDEV с предложением OVER  
 В приведенном ниже примере возвращается стандартное отклонение для значений квот на продажу в каждом квартале календарного года. Обратите внимание на то, что ORDER BY в предложении OVER упорядочивает STDEV, а ORDER BY в инструкции SELECT упорядочивает результирующий набор.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CalendarYear AS Year, CalendarQuarter AS Quarter, SalesAmountQuota AS SalesQuota,  
       STDEV(SalesAmountQuota) OVER (ORDER BY CalendarYear, CalendarQuarter) AS StdDeviation  
FROM dbo.FactSalesQuota  
WHERE EmployeeKey = 272 AND CalendarYear = 2002  
ORDER BY CalendarQuarter;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Year  Quarter  SalesQuota              StdDeviation
----  -------  ----------------------  -------------------
2002  1         91000.0000             null
2002  2        140000.0000             34648.23
2002  3         70000.0000             35921.21
2002  4        154000.0000             39752.36
 ```  
  
## <a name="see-also"></a>См. также:  
 [Агрегатные функции (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
  


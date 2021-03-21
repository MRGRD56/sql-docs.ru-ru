---
description: PERCENTILE_CONT (Transact-SQL)
title: PERCENTILE_CONT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENTILE_CONT_TSQL
- PERCENTILE_CONT
dev_langs:
- TSQL
helpviewer_keywords:
- analytic functions, PERCENTILE_CONT
- PERCENTILE_CONT function
ms.assetid: d019419e-5297-4994-97d5-e9c8fc61bbf4
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 074f2e7e972dd9795cb1f84b98d0dd73feb25102
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753044"
---
# <a name="percentile_cont-transact-sql"></a>PERCENTILE_CONT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Вычисляет процентиль на основе постоянного распределения значения столбца в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Результат интерполируется и может отличаться от всех конкретных значений из этого столбца.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
PERCENTILE_CONT ( numeric_literal )   
    WITHIN GROUP ( ORDER BY order_by_expression [ ASC | DESC ] )  
    OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *numeric_literal*  
 Процентиль, который необходимо вычислить. Значение должно находиться в диапазоне от 0.0 до 1,0.  
  
 WITHIN GROUP **(** ORDER BY *order_by_expression* [ **ASC** | DESC ] **)**  
 Указывает список числовых значений, который следует отсортировать и по которому будет вычисляться процентиль. Разрешен только один аргумент *order_by_expression*. Результатом вычисления выражения должен быть точный или приблизительный числовой тип. Другие типы данных недопустимы. Точными числовыми типами являются **int**, **bigint**, **smallint**, **tinyint**, **numeric**, **bit**, **decimal**, **smallmoney** и **money**. Приблизительными числовыми типами являются **float** и **real**. По умолчанию задан порядок сортировки по возрастанию.  
  
 OVER **(** \<partition_by_clause> **)**  
 Делит результирующий набор, полученный с помощью предложения FROM, на секции, к которым применяется функция вычисления процентиля. Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md). В функции PERCENTILE_CONT нельзя указывать \<ORDER BY clause> и \<rows or range clause>синтаксиса OVER.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **float(53)**  
  
## <a name="compatibility-support"></a>Поддержка совместимости  
 При уровне совместимости 110 и выше WITHIN GROUP является зарезервированным ключевым словом. Дополнительные сведения см. в разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="general-remarks"></a>Общие замечания  
 Все значения NULL, имеющиеся в наборе данных, игнорируются.  
  
 Функция PERCENTILE_CONT не детерминирована. Дополнительные сведения см. в разделе [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-basic-syntax-example"></a>A. Пример простого синтаксиса  
 В следующем примере функции PERCENTILE_CONT и PERCENTILE_DISC используются для определения медианной заработной платы сотрудников в каждом отделе. Эти функции могут возвращать разные значения. Функция PERCENTILE_CONT интерполирует соответствующее значение независимо от того, существует ли оно в наборе данных, тогда как функция PERCENTILE_DISC всегда возвращает фактическое значение из набора.  
  
```sql  
USE AdventureWorks2012;  
  
SELECT DISTINCT Name AS DepartmentName  
      ,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianCont  
      ,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY ph.Rate)   
                            OVER (PARTITION BY Name) AS MedianDisc  
FROM HumanResources.Department AS d  
INNER JOIN HumanResources.EmployeeDepartmentHistory AS dh   
    ON dh.DepartmentID = d.DepartmentID  
INNER JOIN HumanResources.EmployeePayHistory AS ph  
    ON ph.BusinessEntityID = dh.BusinessEntityID  
WHERE dh.EndDate IS NULL;  
```  
  
 Здесь приводится частичный результирующий набор.  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.8269      16.8269
Engineering            34.375       32.6923
Executive              54.32695     48.5577
Human Resources        17.427850    16.5865
```  

### <a name="b-basic-syntax-example"></a>Б. Пример простого синтаксиса  
 В следующем примере функции PERCENTILE_CONT и PERCENTILE_DISC используются для определения медианной заработной платы сотрудников в каждом отделе. Эти функции могут возвращать разные значения. Функция PERCENTILE_CONT интерполирует соответствующее значение независимо от того, существует ли оно в наборе данных, тогда как функция PERCENTILE_DISC всегда возвращает фактическое значение из набора.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DISTINCT DepartmentName  
,PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianCont  
,PERCENTILE_DISC(0.5) WITHIN GROUP (ORDER BY BaseRate)  
    OVER (PARTITION BY DepartmentName) AS MedianDisc  
FROM dbo.DimEmployee; 
```  
  
 Здесь приводится частичный результирующий набор.  
  
 ```
DepartmentName        MedianCont    MedianDisc
--------------------   ----------   ----------
Document Control       16.826900    16.8269
Engineering            34.375000    32.6923
Human Resources        17.427850    16.5865
Shipping and Receiving 9.250000      9.0000
```  
  
## <a name="see-also"></a>См. также:  
 [PERCENTILE_DISC (Transact-SQL)](../../t-sql/functions/percentile-disc-transact-sql.md)  
  
 

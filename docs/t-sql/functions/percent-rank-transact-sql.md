---
description: PERCENT_RANK (Transact-SQL)
title: PERCENT_RANK (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/20/2015
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PERCENT_RANK_TSQL
- PERCENT_RANK
dev_langs:
- TSQL
helpviewer_keywords:
- PERCENT_RANK function
- analytic functions, PERCENT_RANK
ms.assetid: e361c2d4-c01f-4da4-8e89-1ddc724a2629
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab0ebbaea4c00c1cf10fa8d6728ba44b7ea2180f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746534"
---
# <a name="percent_rank-transact-sql"></a>PERCENT_RANK (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Вычисляет относительный ранг строки из группы строк в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Для вычисления относительного положения значения в секции или результирующем наборе запроса используется функция PERCENT_RANK. Функция PERCENT_RANK похожа на функцию CUME_DIST.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
PERCENT_RANK( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 *partition_by_clause* делит результирующий набор, полученный с помощью предложения FROM, на секции, к которым применяется функция. Если этот параметр не указан, функция обрабатывает все строки результирующего набора запроса как отдельные группы. _order\_by\_clause_ определяет логический порядок, в котором выполняется операция. Аргумент *order_by_clause* является обязательным. В функции PERCENT_RANK нельзя указывать \<rows or range clause\> синтаксиса OVER.  Дополнительные сведения см. в статье [Предложение OVER (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **float(53)**  
  
## <a name="general-remarks"></a>Общие замечания  
 Диапазон значений, возвращаемый функцией PERCENT_RANK, больше 0 и меньше или равен 1. В первой строке любого набора PERCENT_RANK равна. 0. Значения NULL по умолчанию включаются и рассматриваются как наименьшие возможные значения.  
  
 Функция PERCENT_RANK не детерминирована. Дополнительные сведения см. в разделе [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере с помощью функции CUME_DIST выполняется вычисление процентиля заработной платы для каждого сотрудника указанного отдела. Значение, возвращаемое функцией CUME_DIST, представляет процент сотрудников, заработная плата которых меньше или равна заработной плате текущего сотрудника этого отдела. Функция PERCENT_RANK вычисляет ранг заработной платы сотрудника в рамках отдела в виде процентного значения. Предложение PARTITION BY указывается для секционирования строк результирующего набора по отделам. Предложение ORDER BY в предложении OVER упорядочивает строки в каждой секции. Предложение ORDER BY в инструкции SELECT сортирует сроки во всем результирующем наборе.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также:  
 [CUME_DIST &#40; Transact-SQL &#41;](../../t-sql/functions/cume-dist-transact-sql.md)  
  
  

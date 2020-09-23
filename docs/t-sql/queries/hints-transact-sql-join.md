---
description: Указания (Transact-SQL) — соединение
title: Указания в соединении (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Join Hint
- Join_Hint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- HASH join hint
- REMOTE join hint
- LOOP join hint
- join hints
- MERGE join hint
- hints [SQL Server], join
ms.assetid: 09069f4a-f2e3-4717-80e1-c0110058efc4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 37f988a345bcc4280f6e76f039049ca56a4f6a81
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2020
ms.locfileid: "91116308"
---
# <a name="hints-transact-sql---join"></a>Указания (Transact-SQL) — соединение
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Подсказки в соединении указывают оптимизатору запросов на выбор определенной стратегии соединения двух таблиц в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Общие сведения о соединениях и синтаксисе соединения см. в статье [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).  
  
> [!CAUTION]  
>  Поскольку оптимизатор запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обычно выбирает наилучший план выполнения запроса, подсказки рекомендуется использовать только опытным разработчикам и администраторам баз данных в качестве последнего средства.
  
 **Применимо к:**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [SELECT](../../t-sql/queries/select-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
<join_hint> ::=   
     { LOOP | HASH | MERGE | REMOTE }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 LOOP \| HASH \| MERGE  
 Задает использование циклов, хэша и операций объединения при соединении в запросе. Использование LOOP | HASH | MERGE JOIN автоматически создает соединение между двумя таблицами. Аргумент LOOP не может указываться вместе с параметрами RIGHT или FULL в качестве типа соединения. Дополнительные сведения см. в статье о [соединениях](../../relational-databases/performance/joins.md).
  
 REMOTE  
 Задает, что операция соединения проводится на сайте в таблице, расположенной справа. Данный аргумент удобно использовать в случае, когда таблица, расположенная слева, является локальной, а справа располагается удаленная таблица. Аргумент REMOTE может использоваться в случае, когда в таблице слева содержится меньшее количество строк, чем в таблице справа.  
  
 Если таблица, расположенная справа, является локальной, то операция соединения также проводится локально. Если обе таблицы являются удаленными, но расположены в различных источниках данных, то при задании аргумента REMOTE операция соединения проводится на сайте в таблице, расположенной справа. Если обе таблицы являются удаленными таблицами в одном источнике данных, то аргумент REMOTE не требуется.  
  
 Аргумент REMOTE не может быть использован для сравнения в предикате соединения значений, одно из которых приведено к другим параметрам сортировки с помощью предложения COLLATE.  
  
 Аргумент REMOTE может быть использован только при операциях INNER JOIN.  
  
## <a name="remarks"></a>Remarks  
 Указания по соединению задаются в запросе в предложении FROM. Указания по соединению принудительно активируют стратегию соединения между двумя таблицами. При задании указаний соединений для любых двух таблиц оптимизатор запросов принудительно автоматически активирует порядок соединения для всех соединенных таблиц в запросе, основанном на положении ключевого слова ON. При использовании CROSS JOIN без предложения ON для определения порядка соединения могут использоваться скобки.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-hash"></a>A. Использование HASH  
 В следующем примере операция `JOIN` в запросе выполняется с помощью соединения `HASH`. В этом примере используется база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
SELECT p.Name, pr.ProductReviewID  
FROM Production.Product AS p  
LEFT OUTER HASH JOIN Production.ProductReview AS pr  
ON p.ProductID = pr.ProductID  
ORDER BY ProductReviewID DESC;  
```  
  
### <a name="b-using-loop"></a>Б. Использование LOOP  
 В следующем примере операция `JOIN` в запросе выполняется с помощью соединения `LOOP`. В этом примере используется база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
DELETE FROM Sales.SalesPersonQuotaHistory   
FROM Sales.SalesPersonQuotaHistory AS spqh  
    INNER LOOP JOIN Sales.SalesPerson AS sp  
    ON spqh.SalesPersonID = sp.SalesPersonID  
WHERE sp.SalesYTD > 2500000.00;  
GO  
```  
  
### <a name="c-using-merge"></a>В. Использование инструкции MERGE  
 В следующем примере операция `JOIN` в запросе выполняется с помощью соединения `MERGE`. В этом примере используется база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
SELECT poh.PurchaseOrderID, poh.OrderDate, pod.ProductID, pod.DueDate, poh.VendorID   
FROM Purchasing.PurchaseOrderHeader AS poh  
INNER MERGE JOIN Purchasing.PurchaseOrderDetail AS pod   
    ON poh.PurchaseOrderID = pod.PurchaseOrderID;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)  
  
  

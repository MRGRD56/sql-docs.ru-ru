---
description: Вычисления pushdown в PolyBase
title: Вычисления pushdown в PolyBase
dexcription: Enable pushdown computation to improve performance of queries on your Hadoop cluster. You can select a subset of rows/columns in an external table for pushdown.
ms.date: 03/09/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 872d23ca6c908bae5e238a50aad76c52ff4bca26
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103891"
---
# <a name="pushdown-computations-in-polybase"></a>Вычисления pushdown в PolyBase

[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Вычисление pushdown повышает производительность запросов во внешних источниках данных. Начиная с версии [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], вычисления pushdown были доступны для внешних источников данных Hadoop. [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] предлагает вычисления pushdown для других типов внешних источников данных.  

## <a name="enable-pushdown-computation"></a> Активация вычислений pushdown

В следующих статьях содержатся сведения о настройке вычислений pushdown для конкретных типов внешних источников данных.

- [Enable pushdown computation](polybase-configure-hadoop.md#pushdown) (Активация вычислений pushdown)
- [Настройка PolyBase для доступа к внешним данным в Oracle](polybase-configure-oracle.md)
- [Настройка PolyBase для доступа к внешним данным в Teradata](polybase-configure-teradata.md)
- [Настройка PolyBase для доступа к внешним данным в MongoDB](polybase-configure-mongodb.md)
- [Настройка доступа к внешним данным в PolyBase с помощью универсальных типов ODBC](polybase-configure-odbc-generic.md)
- [Настройка PolyBase для доступа к внешним данным в SQL Server](polybase-configure-sql-server.md)

В таблице ниже приведена поддержка вычислений pushdown для различных внешних источников данных:

| Источник данных      | Объединения  | Проекции | Агрегации | фильтры;   | Статистика |
|------------------|--------|-------------|--------------|-----------|------------|
| **Базовый протокол ODBC** | Да    | Да         | Да          | Да       | Да        |  
| **Oracle**       | Да    | Да         | Да          | Да       | Да        |
| **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**   | Да    | Да         | Да          | Да       | Да        |
| **Teradata**     | Да    | Да         | Да          | Да       | Да        |  
| **MongoDB**      | **Нет** | Да         | Да          | Да       | Да        |
| **Hadoop\** _     | _ *Нет** | Да         | Частично\*\*     | Частично\*\*  | Да        |  
|                  |

\* PolyBase сейчас поддерживает два поставщика Hadoop — Hortonworks Data Platform (HDP) и Cloudera Distributed Hadoop (CDH). В плане вычислений pushdown между этими двумя компонентами нет различий.

\*\* Дополнительные сведения о поддержке функций pushdown для Hadoop см. в разделе [Поддержка вычислений pushdown в операторах T-SQL](polybase-versioned-feature-summary.md#pushdown-computation-supported-by-t-sql-operators).

> [!NOTE]
> Вычисления pushdown могут блокироваться определенным синтаксисом T-SQL. Дополнительные сведения: [Синтаксис для блокировки pushdown](polybase-versioned-feature-summary.md#syntax-that-prevents-pushdown).

## <a name="key-beneficial-scenarios-of-pushdown-computation"></a>Ключевые сценарии эффективного использования вычислений pushdown

Вычисления pushdown в PolyBase позволяют делегировать задачи вычислений внешним источникам данных. Это уменьшает нагрузку на экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и может существенно повысить производительность. 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может отправлять внешним источникам соединения, проекции, агрегаты и фильтры, чтобы использовать удаленные вычисления, а также ограничивать передачу данных по сети. 

### <a name="pushdown-of-joins"></a>Pushdown-отправка соединений

Во многих случаях PolyBase может обеспечить pushdown-отправку оператора соединения, тем самым значительно увеличивая производительность.  

Если соединение можно выполнить на внешнем источнике данных, это сокращает число перемещений данных и повышает производительность запроса. Если не выполнять pushdown-отправку соединения, то данные из соединяемых таблиц необходимо перенести в локальную базу tempdb и соединить только там.

### <a name="select-a-subset-of-rows"></a>Выбор подмножества строк

Включение предиката позволяет повысить производительность для запроса, отбирающего подмножество строк из внешней таблицы.

В этом примере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] инициирует задание map-reduce для получения строк, соответствующих предикату `customer.account_balance < 200000` в Hadoop. Так как запрос может быть выполнен и без сканирования всех строк в таблице, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] копируются только строки, удовлетворяющие условиям предиката. Это существенно экономит время и место для временного хранения данных, если число клиентов с балансом < 200 000 меньше числа клиентов с балансом >= 200 000.

```sql
SELECT * FROM customer WHERE customer.account_balance < 200000;
SELECT * FROM SensorData WHERE Speed > 65;  
```

### <a name="select-a-subset-of-columns"></a>Выбор подмножества столбцов

Включение предиката позволяет повысить производительность для запроса, отбирающего подмножество столбцов из внешней таблицы.

В этом запросе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запускает задание map-reduce для предварительной обработки текстового файла Hadoop с разделителями, при которой в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] копируются данные только для двух столбцов — customer.name и customer.zip_code.

```sql
SELECT customer.name, customer.zip_code
FROM customer
WHERE customer.account_balance < 200000;
```

### <a name="pushdown-for-basic-expressions-and-operators"></a>Включение основных выражений и операторов

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет использовать для pushdown предикатов следующие основные выражения и операторы.

- Операторы двоичного сравнения (`<`, `>`, `=`, `!=`, `<>`, `>=`, `<=`) для чисел, значений дат и времени.
- Арифметические операторы (`+`, `-`, `*`, `/`, `%`).
- Логические операторы (`AND`, `OR`).
- Унарные операторы (`NOT`, `IS NULL`, `IS NOT NULL`).

Операторы `BETWEEN`, `NOT`, `IN` и `LIKE` поддерживают принудительную отправку. Фактическое поведение зависит от того, каким образом оптимизатор запросов перезаписывает выражения операторов как последовательности инструкций с использованием основных операторов отношения.

В этом примере запрос содержит несколько предикатов, которые могут быть переданы в Hadoop. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может отправлять в Hadoop задания map-reduce для выполнения предиката `customer.account_balance <= 200000`. Выражение `BETWEEN 92656 AND 92677` также состоит из двоичных и логических операций, которые могут быть переданы в Hadoop. Логический оператор **AND** в столбцах `customer.account_balance AND customer.zipcode` является конечным выражением.

С учетом этого сочетания предикатов задания map-reduce могут выполнять все условия, указанные в предложении WHERE. Только данные, которые соответствуют условиям `SELECT`, копируются обратно в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

```sql
SELECT * FROM customer 
WHERE customer.account_balance <= 200000 
    AND customer.zipcode BETWEEN 92656 AND 92677;
```

## <a name="examples"></a>Примеры

### <a name="force-pushdown"></a>Принудительное включение

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (FORCE EXTERNALPUSHDOWN);
```

### <a name="disable-pushdown"></a>Отключить включение

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65
OPTION (DISABLE EXTERNALPUSHDOWN);
```

## <a name="next-steps"></a>Дальнейшие действия

См. дополнительные сведения о том, [что такое PolyBase.](polybase-guide.md)

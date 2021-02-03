---
description: Работа с оптимизированными для памяти темпоральными таблицами с системным управлением версиями
title: Работа с оптимизированными для памяти темпоральными таблицами с системным управлением версиями | Документация Майкрософт
ms.custom: ''
ms.date: 05/05/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 691d4f80-6754-43f5-8b43-d4facf08f6fc
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: acfdadf0216bd243f61339982615c4bf83796dfa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177978"
---
# <a name="working-with-memory-optimized-system-versioned-temporal-tables"></a>Работа с оптимизированными для памяти темпоральными таблицами с системным управлением версиями


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


В этом разделе обсуждается, как работа с оптимизированной для памяти темпоральной таблицей с системным управлением версиями отличается от работы с дисковой темпоральной таблицей с системным управлением версиями.

> [!NOTE]
> Использование Temporal с оптимизированными для памяти таблицами применяется только к [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] и не относится к [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

## <a name="discovering-metadata"></a>Обнаружение метаданных

Для обнаружения метаданных об оптимизированной для памяти темпоральной таблице с системным управлением версиями необходимо объединить информацию из разделов [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) и [sys.internal_tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md). Темпоральная таблица с системным управлением версиями представляется в виде parent_object_id внутренней таблицы журнала в памяти.

В этом примере показано, как выполнить запрос к этим таблицам и их соединение.

```sql
SELECT SCHEMA_NAME (T1.schema_id) as TemporalTableSchema
    , OBJECT_NAME(IT.parent_object_id) as TemporalTableName
    , T1.object_id as TemporalTableObjectId
    , IT.Name as InternalHistoryStagingName
    , SCHEMA_NAME (T2.schema_id) as HistoryTableSchema
    , OBJECT_NAME (T1.history_table_id) as HistoryTableName
FROM sys.internal_tables IT
JOIN sys.tables T1
    ON IT.parent_object_id = T1.object_id
JOIN sys.tables T2
    ON T1.history_table_id = T2.object_id
WHERE T1.is_memory_optimized = 1 AND T1.temporal_type = 2

```

## <a name="modifying-data"></a>Изменение данных

Оптимизированные для памяти темпоральные таблицы с системным управлением версиями можно изменять посредством хранимых процедур, скомпилированных в собственном коде. Это позволяет преобразовывать нетемпоральные оптимизированные для памяти таблицы в таблицы с системным управлением версиями и сохранять при этом существующие собственные хранимые процедуры.

В этом примере показано, как можно изменить ранее созданную таблицу в модуле, скомпилированном в собственном коде.

```sql
CREATE PROCEDURE dbo.UpdateFXCurrencyPair
   (
      @ProviderID int
      , @CurrencyID1 int
      , @CurrencyID2 int
      , @BidRate decimal(8,4)
      , @AskRate decimal(8,4)
   )
WITH NATIVE_COMPILATION, SCHEMABINDING
   , EXECUTE AS OWNER
AS
   BEGIN ATOMIC WITH
   (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'English')
      UPDATE dbo.FXCurrencyPairs SET AskRate = @AskRate, BidRate = @BidRate
     WHERE ProviderID = @ProviderID AND CurrencyID1 = @CurrencyID1 AND CurrencyID2 = @CurrencyID2
END
GO ;

```

## <a name="see-also"></a>См. также:

- [Темпоральные таблицы с системным управлением версиями и таблицы, оптимизированные для памяти](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Создание оптимизированной для памяти темпоральной таблицы с системным управлением версиями](../../relational-databases/tables/creating-a-memory-optimized-system-versioned-temporal-table.md)
- [Мониторинг оптимизированных для памяти темпоральных таблиц с системным управлением версиями](../../relational-databases/tables/monitoring-memory-optimized-system-versioned-temporal-tables.md)
- [Вопросы, связанные с производительностью оптимизированных для памяти темпоральных таблиц с системным управлением версиями](../../relational-databases/tables/
- [Темпоральные таблицы](../../relational-databases/tables/temporal-tables.md)
- [Проверка согласованности системной темпоральной таблицы](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Управление хранением данных журнала в темпоральных таблицах с системным управлением версиями](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Представления и функции метаданных для временной таблицы](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)

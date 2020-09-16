---
description: Остановка системного управления версиями в темпоральной таблице с системным управлением версиями
title: Остановка системного управления версиями в темпоральной таблице с системным управлением версиями | Документация Майкрософт
ms.custom: ''
ms.date: 04/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: dddd707e-bfb1-44ff-937b-a84c5e5d1a94
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2686ba2c5ac8e4db03f49a1d090ed8de1066b2c7
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2020
ms.locfileid: "89550955"
---
# <a name="stopping-system-versioning-on-a-system-versioned-temporal-table"></a>Остановка системного управления версиями в темпоральной таблице с системным управлением версиями


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Возможно, вам временно или навсегда понадобится остановить управление версиями в темпоральной таблице. Для этого нужно задать для предложения **SYSTEM_VERSIONING** значение **OFF**.

## <a name="setting-system_versioning--off"></a>Установка для предложения SYSTEM_VERSIONING значения OFF

Остановите системное управление версиями, если в темпоральной таблице нужно провести определенные операции обслуживания или таблица с управлением версиями больше не нужна. В результате этой операции вы получите две отдельные таблицы:

- текущую таблицу с определением периода;

- прежнюю таблицу в качестве обычной таблицы.

### <a name="important-remarks"></a>Важные замечания

- Таблица журнала **остановит** поставку обновлений в течение **SYSTEM_VERSIONING = OFF**.
- Если задать значение **SYSTEM_VERSIONING = OFF** или удалить период **SYSTEM_TIME**, это не приведет к потере данных в **темпоральной таблице**.
- Если задать **SYSTEM_VERSIONING = OFF** и не исключить удаление периода **SYSTEM_TIME** , система продолжит обновлять столбцы периода при каждой операции вставки и обновления. Элементы, удаленные из текущей таблицы, не подлежат восстановлению.
- Удалите период **SYSTEM_TIME** , чтобы полностью удалить столбцы периода.
- Если задать **SYSTEM_VERSIONING = OFF**, все пользователи с необходимыми разрешениями смогут изменять схему и содержимое прежней таблицы или даже окончательно удалить ее.
- Нельзя задать **SYSTEM_VERSIONING = OFF** при наличии других объектов, созданных с помощью SCHEMABINDING, с использованием временных расширений запросов, таких как ссылки **SYSTEM_TIME**. Это ограничение предотвращает сбой этих объектов, если задано **SYSTEM_VERSIONING = OFF**.

### <a name="permanently-remove-system_versioning"></a>Окончательное удаление SYSTEM_VERSIONING

В этом примере окончательно удаляется параметр SYSTEM_VERSIONING и полностью удаляются столбцы периода. Удалять столбцы периода необязательно.

```sql
ALTER TABLE dbo.Department SET (SYSTEM_VERSIONING = OFF);
/*Optionally, DROP PERIOD if you want to revert temporal table to a non-temporal*/
ALTER TABLE dbo.Department
DROP PERIOD FOR SYSTEM_TIME;
```

### <a name="temporarily-remove-system_versioning"></a>Временное удаление SYSTEM_VERSIONING

Это список операций, которые требуют, чтобы для системного управления версиями было задано значение **OFF**:

- удаление ненужных данных из журнала (**DELETE** или **TRUNCATE**);
- удаление данных из текущей таблицы без управления версиями (**DELETE**, **TRUNCATE**);
- секционирование **SWITCH OUT** из текущей таблицы;
- секционирование **SWITCH IN** в прежнюю таблицу.

В этом примере действие SYSTEM_VERSIONING временно прекращается для выполнения определенных операций обслуживания. Если необходимо временно остановить управление версиями для обслуживания таблицы, настоятельно рекомендуем делать это в ходе транзакции, чтобы обеспечить согласованность данных.

> [!NOTE]
> При повторном включении системного управления версиями не забудьте указать аргумент HISTORY_TABLE. Невыполнение этого требования приведет к созданию новой таблицы журнала, которая связывается с текущей таблицей. Исходная таблица журнала по-прежнему будет существовать как обычная таблица, но не будет связана с текущей таблицей.

```sql
BEGIN TRAN
ALTER TABLE dbo.Department SET (SYSTEM_VERSIONING = OFF);
TRUNCATE TABLE [History].[DepartmentHistory]
WITH (PARTITIONS (1,2))
ALTER TABLE dbo.Department SET
(
SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.DepartmentHistory)
);
COMMIT ;
```

## <a name="next-steps"></a>Дальнейшие действия

- [Темпоральные таблицы](../../relational-databases/tables/temporal-tables.md)
- [Приступая к работе c темпоральными таблицами с системным управлением версиями](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Управление хранением данных журнала в темпоральных таблицах с системным управлением версиями](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Темпоральные таблицы с системным управлением версиями и таблицы, оптимизированные для памяти](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Создание темпоральной таблицы с системным управлением версиями](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
- [Изменение данных в темпоральной таблице с системным управлением версиями](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [Запрос данных в темпоральной таблице с системным управлением версиями](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [Изменение схемы темпоральной таблицы с системным управлением версиями](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)

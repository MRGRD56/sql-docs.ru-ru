---
description: Возможности и ограничения PolyBase
title: Возможности и ограничения PolyBase
descriptions: This article summarizes PolyBase features available for SQL Server products and services. It lists T-SQL operators supported for pushdown and known limitations.
ms.date: 04/06/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4bfb81324da4bb224e8c2e83fd6e62584bb44819
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003910"
---
# <a name="polybase-features-and-limitations"></a>Возможности и ограничения PolyBase

[!INCLUDE[appliesto-ss2016-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

В этой статье приведена сводка функций PolyBase, доступных для продуктов и служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="feature-summary-for-product-releases"></a>Сводка функций по выпускам продукта

В следующей таблице перечислены основные функции PolyBase и продукты, в которых они доступны.  

|**Компонент** |**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** (с версии 2016) |**База данных SQL Azure** |**[!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]** |**Параллельное хранилище данных** |
|---------|---------|---------|---------|---------|
|Запрос данных Hadoop с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)]|Да|Нет|Нет|Да|
|Импорт данных из Hadoop|Да|Нет|Нет|Да|
|Экспорт данных в Hadoop  |Да|Нет|Нет| Да|
|Запрос, импорт и экспорт данных в Azure HDInsight |нет|Нет|Нет|нет
|Отправка результатов вычислений запросов в Hadoop|Да|Нет|Нет|Да|  
|Импорт данных из хранилища BLOB-объектов Azure|Да|Да<sup>*</sup>|Да|Да|
|Экспорт данных в хранилище BLOB-объектов Azure|Да|Нет|Да|Да|  
|Импорт данных из хранилища Azure Data Lake Store|нет|Нет|Да|нет|
|Экспорт данных в хранилище Azure Data Lake Store|нет|Нет|Да|нет|
|Выполнение запросов PolyBase из средств бизнес-аналитики Майкрософт|Да|Нет|Да|Да|

<sup>*</sup> Появилось в версии [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]. См. [Примеры массового доступа к данным в хранилище BLOB-объектов Azure](../import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).


## <a name="syntax-that-prevents-pushdown"></a>Синтаксис для блокировки pushdown

Следующие функции и синтаксис T-SQL блокируют pushdown-вычисления:

- `AT TIME ZONE`
- `CONCAT_WS`
- `TRANSLATE`
- `RAND`
- `CHECKSUM`
- `BINARY_CHECKSUM`
- `ISJSON`
- `JSON_VALUE`
- `JSON_QUERY`
- `JSON_MODIFY`
- `NEWID`
- `STRING_ESCAPE`
- `COMPRESS`
- `DECOMPRESS`
- `GREATEST`
- `LEAST`
- `PARSE`

Поддержка pushdown для синтаксиса `FORMAT` и `TRIM` была впервые представлена в [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] с накопительным пакетом обновления 10 (CU10).

Дополнительные сведения: [Вычисления pushdown в PolyBase](polybase-pushdown-computation.md).

## <a name="pushdown-computation-supported-by-t-sql-operators"></a>Поддержка передачи вычислений в операторах T-SQL

В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и APS лишь некоторые операторы T-SQL поддерживают pushdown-отправку в кластер Hadoop. В следующей таблице приведены все поддерживаемые операторы и ряд неподдерживаемых.

|**Тип оператора** |**Отправка в Hadoop** |**Отправка в хранилище BLOB-объектов** | 
|---------|---------|---------|
|Проекции столбцов|Да|нет|
|Предикаты|Да|Нет|
|Статистические выражения|Частично\*|нет|
|Соединения между внешними таблицами|нет|нет|
|Соединения между внешними и локальными таблицами|нет|нет|
|Сортировки|нет|Нет|

\* Частичная агрегация означает, что итоговая агрегация должна происходить после прихода данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Однако часть статистического вычисления происходит в Hadoop. Это стандартный метод вычисления статистических выражений в системах обработки данных с массовым параллелизмом.  

#### <a name="hadoop-pushdown-support"></a>Поддержка pushdown в Hadoop

Поставщики Hadoop поддерживают следующее:

| **Агрегации**                  | **Фильтры (двоичное сравнение)** | 
|-----------------------------------|---------------------------------| 
| Count_Big                         | NotEqual                        | 
| SUM                               | LessThan;                        | 
| Сред.                               | LessOrEqual                     | 
| Макс.                               | GreaterOrEqual                  | 
| Min                               | GreaterThan                     | 
| Approx_Count_Distinct             | Is                              | 
|                                   | IsNot;                           | 
|                                   |                                 | 

## <a name="known-limitations"></a>Известные ограничения

PolyBase имеет следующие ограничения.

- Чтобы использовать PolyBase, необходимо иметь роль системного администратора или разрешения на управление сервером базы данных.

- В версиях до [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] допустимый размер строк, в том числе полная длина столбцов переменной длины, не может превышать 32 КБ для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или 1 МБ для [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]. Начиная с версии [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)], это ограничение снято. Лимит в 1 МБ сохраняется для источников данных Hadoop, но для других источников действует только максимальный лимит [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- При экспорте данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)] в файлы формата ORC возможно ограничение числа столбцов с большим количеством текста. Их число может даже не превышать 50, что связано с сообщениями об ошибках нехватки памяти Java. Чтобы обойти эту проблему, экспортируйте подмножество столбцов.

- PolyBase не может подключаться к экземплярам Hadoop при включении Knox.

- Если вы используете таблицы Hive с параметром transactional = true, то PolyBase не может обратиться к данным в каталоге таблицы Hive.

<!--SQL Server 2016-->
::: moniker range="= sql-server-2016 "

- [PolyBase не устанавливается при добавлении узла в отказоустойчивый кластер [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]](https://support.microsoft.com/help/3173087/fix-polybase-feature-doesn-t-install-when-you-add-a-node-to-a-sql-server-2016-failover-cluster).

::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

См. дополнительные сведения о том, [что такое PolyBase.](polybase-guide.md)

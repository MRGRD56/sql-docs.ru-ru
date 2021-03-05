---
title: 'Предварительная версия Azure Synapse Pathway: сопутствующие ресурсы.'
description: Технические подробности того, как Azure Synapse Pathway преобразует код.
author: anshul82-ms
ms.author: anrampal.
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: conceptual
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-concept
ms.openlocfilehash: dbd362e53b5bfcd916c53e90d6f66c8fb44f0374
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873147"
---
# <a name="azure-synapse-pathway-preview-behind-the-scenes"></a>Предварительная версия Azure Synapse Pathway: сопутствующие ресурсы
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Цель Azure Synapse Pathway — сохранить функциональное назначение исходного кода при оптимизации для Synapse SQL. Synapse Pathway использует трехэтапный процесс для преобразования кода SQL из исходной системы в Azure Synapse SQL.

Каждый из этапов сохраняет и дополняет данные об источнике, включая метаданные, относящиеся к источнику, чтобы обеспечить наивысшее качество преобразования.

 ![Azure Synapse Pathway.](./media/technical-deep-dive/behind-the-scene.png)

## <a name="stage-1--lexing-and-parsing"></a>Этап 1. Лексический и синтаксический анализ

Синтаксический анализ SQL — это проблема, которая решена уже много раз. Существует множество средств анализа, как коммерческих, так и с открытым кодом, которые помогают базовому процессу взять исходную инструкцию, разбить ее на логические маркеры, а затем выполнить ее в отношении набора или правил средства анализа, чтобы обеспечить согласованность языка. 

Synapse SQL определяет исходные грамматики, позволяющие средству идентифицировать и преобразовать входные данные SQL в дополненное дерево абстрактного синтаксиса (AST), которое используется при дальнейшем преобразовании. 

## <a name="stage-2---augmented-abstract-syntax-tree-ast"></a>Этап 2. Дополненное дерево абстрактного синтаксиса (AST)

Synapse Pathway определяет общее представление всех объектов в дополненном дереве абстрактного синтаксиса (AST). AST Synapse Pathway содержит дополнительные инструкции или фрагментированные метаданные, которые используются для принятия правильного решения при преобразовании кода из одной системы в другую.

Отслеживая, что маркер является не просто функцией, а требованием к типу исходной системы, компоненты создания сценариев могут принимать более обоснованные решения о преобразовании в Synapse SQL.

Например, исходная функция для абсолютной функции определяется следующим образом:

```sql  
ABS( float_expression ) 
```

Azure Synapse SQL определяет абсолютную функцию следующим образом:

```sql  
ABS ( numeric_expression )  
```

В этом простом случае Synapse Pathway понимает, что преобразование в Synapse SQL из типа числа с плавающей точкой в числовой тип является неявным [преобразованием](../../t-sql/functions/cast-and-convert-transact-sql?view=azure-sqldw-latest#implicit-conversions) и не требует дальнейшего приведения типов. Простое и эффективное преобразование кода.

Хранение этих метаданных об исходных инструкциях и фрагментах помогает понять структурные различия между платформами, например преобразования в логике отказа для предикатов условий поиска в предложении WHERE.

## <a name="stage-3---syntax-generation"></a>Этап 3. Создание синтаксиса

Последний этап процесса — создание синтаксиса для Synapse SQL. Используя структуру AST, созданную из исходных файлов, Synapse Pathway передает каждый объект DDL в отдельный файл. Генераторы синтаксиса используют подробные сведения о целевой платформе для оптимизации инструкций.

Например, общий шаблон, который используется в сценариях загрузки данных, сначала удаляет все содержимое в промежуточной таблице, а затем загружает данные из другой промежуточной таблицы в режиме INSERT/SELECT.

```sql  
DELETE staging.table1 ALL;
INSERT INTO staging.table1…
FROM staging.table2;
```

В Synapse SQL имеется оптимизированный способ для этого сценария — [CREATE TABLE AS SELECT](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-develop-ctas). Инструкция CTAS — это операция на основе пакетной обработки и с минимальным протоколированием, что приводит к повышению производительности благодаря использованию всей доступной вычислительной инфраструктуры. Без этих аналитических сведений о Synapse SQL средства часто выполняют усечение и создают инструкции INSERT или SELECT.

```sql  
TRUNCATE TABLE staging.table1;
INSERT INTO staging.table1…
FROM staging.table2;
```

Хотя это и неплохо, этот код можно оптимизировать с использованием DROP TABLE и CTAS для повышения производительности.

```sql  
DROP TABLE staging.table1;
CREATE TABLE staging.table1
WITH
(
    -- Derived from the original table definition 
    DISTRIBUTION = HASH(column1),
    -- Derived from the original table definition
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT  * FROM staging.table2;
```

## <a name="next-steps"></a>Дальнейшие действия

- [Загрузить Azure Synapse Pathway](synapse-pathway-download.md)
- [Часто задаваемые вопросы](pathway-faq.md)


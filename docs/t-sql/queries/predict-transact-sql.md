---
description: PREDICT (Transact-SQL)
title: PREDICT (Transact-SQL)
titleSuffix: SQL machine learning
ms.custom: ''
ms.date: 06/26/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- PREDICT
- PREDICT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- PREDICT clause
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||>=azure-sqldw-latest'
ms.openlocfilehash: 9ccee4b3502dafac331803ba1e8e56c068666bbc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347547"
---
# <a name="predict-transact-sql"></a>PREDICT (Transact-SQL)

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Создает предсказанное значение или оценки на основе хранимой модели. Дополнительные сведения см.в разделе [Native scoring using the PREDICT T-SQL function](../../machine-learning/predictions/native-scoring-predict-transact-sql.md) (Собственная оценка с использованием функции PREDICT T-SQL).

## <a name="syntax"></a>Синтаксис

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```syntaxsql
PREDICT  
(  
  MODEL = @model | model_literal,  
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

MODEL = @model | model_literal  
```

::: moniker-end

::: moniker range=">=azure-sqldw-latest"

```syntaxsql
PREDICT  
(  
  MODEL = <model_object>,
  DATA = object AS <table_alias>
  [, RUNTIME = ONNX ]
)  
WITH ( <result_set_definition> )  

<result_set_definition> ::=  
  {  
    { column_name  
      data_type  
      [ COLLATE collation_name ]  
      [ NULL | NOT NULL ]  
    }  
      [,...n ]  
  }  

<model_object> ::=
  {
    model_literal
    | model_variable
    | ( scalar_subquery )
  }
```

::: moniker-end

### <a name="arguments"></a>Аргументы

**MODEL**

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Параметр `MODEL` используется для указания модели, используемой для оценки и прогнозирования. Модель указывается как переменная или литерал или скалярное выражение.

Функция `PREDICT` поддерживает модели, обученные с помощью пакетов [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) и [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Параметр `MODEL` используется для указания модели, используемой для оценки и прогнозирования. Модель указывается как переменная или литерал или скалярное выражение.

В Управляемом экземпляре SQL Azure `PREDICT` поддерживает модели в формате [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) или модели, обученные с помощью пакетов [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) и [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md).
::: moniker-end

::: moniker range=">=azure-sqldw-latest"
Параметр `MODEL` используется для указания модели, используемой для оценки и прогнозирования. Модель указывается как переменная, литерал, скалярное выражение или скалярный вложенный запрос.

В Azure Synapse Analytics `PREDICT` поддерживает модели в формате [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html).
::: moniker-end

**Данные**

Параметр DATA используется для указания данных, используемых для оценки и прогнозирования. Данные указываются в виде источника таблицы в запросе. Источник таблицы может быть таблицей, псевдонимом таблицы, псевдонимом обобщенного табличного выражения, представлением или функцией с табличным значением.

**RUNTIME = ONNX**

> [!IMPORTANT]
> Аргумент `RUNTIME = ONNX` доступен только в [Управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview), [SQL Azure для пограничных вычислений](/azure/sql-database-edge/onnx-overview) и [Azure Synapse Analytics](/azure/synapse-analytics/overview-what-is).

Указывает подсистему машинного обучения, используемую для выполнения модели. Параметр `RUNTIME` всегда имеет значение `ONNX`. В SQL Azure для пограничных вычислений и Azure Synapse Analytics этот параметр является обязательным. В Управляемом экземпляре SQL Azure этот параметр является необязательным и требуется только при использовании моделей ONNX.

**WITH ( <result_set_definition> )**

Предложение WITH используется для указания схемы выходных данных, возвращаемых функцией `PREDICT`.

Помимо столбцов, возвращаемых самой функцией `PREDICT`, в запросе можно использовать все столбцы, которые являются частью входных данных.

### <a name="return-values"></a>Возвращаемые значения

Предопределенные схемы недоступны. Содержимое модели не проверяется, возвращаемые значения столбцов тоже не проверяются.

- Функция `PREDICT` проходит через столбцы в качестве входных данных.
- Функция `PREDICT` также создает новые столбцы, но число столбцов и типы данных зависят от типа модели, использованной для прогнозирования.

Сообщения об ошибках, связанные с данными, моделью или форматом столбца, выводятся функцией прогнозирования, связанной с моделью.

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017"
## <a name="remarks"></a>Remarks

Функция `PREDICT` поддерживается во всех выпусках SQL Server 2017 и более поздних версий на Windows и Linux. Для использования функции `PREDICT` не требуется включать [Службы машинного обучения](../../machine-learning/sql-server-machine-learning-services.md).
::: moniker-end

### <a name="supported-algorithms"></a>Поддерживаемые алгоритмы

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017"
Используемая модель должна быть создана с помощью одного из поддерживаемых алгоритмов из пакета [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) или [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md). Список поддерживаемых в настоящее время моделей см. в разделе [Собственная оценка с помощью функции T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end
::: moniker range="=azure-sqldw-latest"
Поддерживаются алгоритмы, которые можно преобразовать в формат модели [ONNX](https://onnx.ai/).
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Поддерживаются алгоритмы, которые можно преобразовать в формат модели [ONNX](https://onnx.ai/), и модели, созданные с помощью одного из поддерживаемых алгоритмов из пакета [RevoScaleR](../../machine-learning/r/ref-r-revoscaler.md) или [revoscalepy](../../machine-learning/python/ref-py-revoscalepy.md). Список поддерживаемых в настоящее время алгоритмов в пакетах RevoScaleR и revoscalepy см. в разделе [Собственная оценка с помощью функции T-SQL PREDICT](../../machine-learning/predictions/native-scoring-predict-transact-sql.md).
::: moniker-end

### <a name="permissions"></a>Разрешения

Для `PREDICT` разрешения не требуются, но пользователю нужно разрешение `EXECUTE` на базу данных и разрешение на запрос любых данных, используемых в качестве входных. У пользователя должна быть возможность чтения модели из таблицы, если модель хранится в таблице.

## <a name="examples"></a>Примеры

В следующих примерах демонстрируется синтаксис вызова `PREDICT`.

### <a name="using-predict-in-a-from-clause"></a>Использование PREDICT в предложении FROM

Этот пример ссылается на функцию `PREDICT` в предложении `FROM` инструкции `SELECT`:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d) WITH (Score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT test_model FROM scoring_model WHERE model_id = 1);

SELECT d.*, p.Score
FROM PREDICT(MODEL = @model,
    DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

::: moniker-end

Псевдоним **d**, указанный для табличного источника в параметре `DATA`, используется для ссылки на столбцы, принадлежащие таблице `dbo.mytable`. Псевдоним **p**, указанный для функции `PREDICT`, используется для ссылки на столбцы, возвращаемые функцией `PREDICT`.

- Модель сохраняется как столбец `varbinary(max)` в таблице с именем **Models**. Дополнительные сведения, такие как **идентификатор** и **описание**, сохраняются в этой таблице для идентификации модели.
- Псевдоним **d**, указанный для табличного источника в параметре `DATA`, используется для ссылки на столбцы, принадлежащие таблице `dbo.mytable`. Имена столбцов входных данных должны соответствовать именам входных данных для модели.
- Псевдоним **p**, указанный для функции `PREDICT`, используется для ссылки на прогнозируемый столбец, возвращаемый функцией `PREDICT`. Имя столбца должно совпадать с именем выходных данных для модели.
- Все столбцы входных данных и прогнозируемые столбцы доступны для вывода в инструкции SELECT.

::: moniker range=">=azure-sqldw-latest"

Предыдущий пример запроса можно переписать для создания представления, указав `MODEL` в качестве скалярного вложенного запроса:

```sql
CREATE VIEW predictions
AS
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT test_model FROM scoring_model WHERE model_id = 1),
             DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH (Score FLOAT) AS p;
```

:::moniker-end

### <a name="combining-predict-with-an-insert-statement"></a>Использование предложения PREDICT с инструкцией INSERT

Прогнозирование часто используется для формирования оценки входных данных и последующей вставки прогнозируемых значений в таблицу. В следующем примере предполагается, что вызывающее приложение использует хранимую процедуру, чтобы вставить в таблицу строку, содержащую прогнозируемое значение:

::: moniker range=">=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d) WITH(score FLOAT) AS p;
```

:::moniker-end

::: moniker range=">=azure-sqldw-latest"

```sql
DECLARE @model VARBINARY(max) = (SELECT model FROM scoring_model WHERE model_name = 'ScoringModelV1');

INSERT INTO loan_applications (c1, c2, c3, c4, score)
SELECT d.c1, d.c2, d.c3, d.c4, p.score
FROM PREDICT(MODEL = @model, DATA = dbo.mytable AS d, RUNTIME = ONNX) WITH(score FLOAT) AS p;
```

:::moniker-end

- Результаты функции `PREDICT` сохраняются в таблице с именем PredictionResults. 
- Модель сохраняется как столбец `varbinary(max)` в таблице с именем **Models**. Дополнительные сведения, такие как идентификатор и описание, сохраняются в этой таблице для идентификации модели.
- Псевдоним **d**, указанный для табличного источника в параметре `DATA`, используется для ссылки на столбцы в таблице `dbo.mytable`. Имена столбцов входных данных должны соответствовать именам входных значений для модели.
- Псевдоним **p**, указанный для функции `PREDICT`, используется для ссылки на прогнозируемый столбец, возвращаемый функцией `PREDICT`. Имя столбца должно совпадать с именем выходных данных для модели.
- Все входные и прогнозируемые столбцы доступны для вывода в инструкции SELECT.

## <a name="next-steps"></a>Дальнейшие действия

- [Native scoring using the PREDICT T-SQL function](../../machine-learning/predictions/native-scoring-predict-transact-sql.md) (Собственная оценка с использованием функции PREDICT T-SQL)
::: moniker range="=azure-sqldw-latest||=azuresqldb-mi-current"
-   [Дополнительные сведения о моделях ONNX](/azure/machine-learning/concept-onnx#get-onnx-models)
::: moniker-end

---
title: Собственная оценка с помощью PREDICT T-SQL
titleSuffix: SQL machine learning
description: Узнайте, как использовать собственную оценку с функцией PREDICT T-SQL для создания прогнозных значений для новых входных данных практически в реальном времени.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||=azuresqldb-current||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest'
ms.openlocfilehash: 74fc66dd6ed235d7508f774e7397c9b4d55e80a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352544"
---
# <a name="native-scoring-using-the-predict-t-sql-function-with-sql-machine-learning"></a>Собственная оценка с использованием функции PREDICT T-SQL с помощью машинного обучения SQL

[!INCLUDE [sqlserver2017-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi-asa.md)]

Узнайте, как использовать собственную оценку с [функцией PREDICT T-SQL](../../t-sql/queries/predict-transact-sql.md) для создания прогнозных значений для новых входных данных практически в реальном времени. Для собственной оценки требуется уже обученная модель.

Функция `PREDICT` использует собственные возможности расширения C++ в [машинном обучении SQL](../index.yml). Эта методика обеспечивает самую высокую скорость рабочих нагрузок прогнозирования, а также моделей поддержки в формате [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html) или моделей, обученных с помощью пакетов [RevoScaleR](../r/ref-r-revoscaler.md) и [revoscalepy](../python/ref-py-revoscalepy.md).

## <a name="how-native-scoring-works"></a>Как работает собственная оценка

В ходе собственной оценки используются библиотеки, которые могут считывать модели в ONNX или стандартном двоичном формате, а также формировать оценки для новых входных данных. Поскольку модель обучена, развернута и сохранена, ее можно использовать для оценки без вызова интерпретатора R или Python. Это означает, что затраты на взаимодействие нескольких процессов сокращаются, что приводит к значительному повышению производительности прогнозирования.

Чтобы использовать собственную оценку, вызовите функцию `PREDICT` T-SQL и передайте следующие обязательные входные данные.

+ Совместимая модель, основанная на поддерживаемой модели и алгоритме.
+ Входные данные, обычно определяемые в виде запроса T-SQL.

Функция возвращает прогнозы для входных данных вместе со всеми столбцами исходных данных, которые необходимо передать.

## <a name="prerequisites"></a>Предварительные требования

Функция `PREDICT` доступна в следующих версиях.

+ Все выпуски SQL Server 2017 и более поздних версий в Windows и Linux
+ Управляемый экземпляр SQL Azure
+ База данных SQL Azure
+ SQL Azure для пограничных вычислений
+ Azure Synapse Analytics

Функция включена по умолчанию. Вам не нужно устанавливать R, Python или включать дополнительные функции.

## <a name="supported-models"></a>Поддерживаемые модели

Форматы модели, поддерживаемые функцией `PREDICT`, зависят от платформы SQL, для которой выполняется собственная оценка. См. таблицу ниже, чтобы узнать, какие форматы моделей поддерживаются на той или иной платформе.

| Платформа | Формат модели ONNX | Формат модели RevoScale |
|-|-|-|
| SQL Server | Нет | Да |
| Управляемый экземпляр SQL Azure | да | Да |
| База данных SQL Azure | Нет | Да |
| SQL Azure для пограничных вычислений | Да | Нет |
| Azure Synapse Analytics | Да | Нет |

::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="onnx-models"></a>Модели ONNX

Модель должна быть в формате [Open Neural Network Exchange (ONNX)](https://onnx.ai/get-started.html).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current||=azuresqldb-current"
### <a name="revoscale-models"></a>Модели RevoScale

Модель должна быть заранее обучена с помощью одного из поддерживаемых алгоритмов **rx**, указанных ниже, с использованием пакетов [RevoScaleR](../r/ref-r-revoscaler.md) или [revoscalepy](../python/ref-py-revoscalepy.md).

Сериализуйте модель с помощью [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) для R или [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) для Python. Эти функции сериализации оптимизированы для поддержки быстрой оценки.

<a name="bkmk_native_supported_algos"></a> 

#### <a name="supported-revoscale-algorithms"></a>Поддерживаемые алгоритмы RevoScale

В revoscalepy и RevoScaleR поддерживаются следующие алгоритмы.

+ Алгоритмы revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) 
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) 
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) 
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) 

+ Алгоритмы RevoScaleR

  + [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/r-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/r-server/r-reference/revoscaler/rxdtree)
  + [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

Если необходимо использовать алгоритмы из MicrosoftML или MicrosoftML, используйте [оценки в реальном времени с sp_rxPredict](../predictions/real-time-scoring.md).

Поддерживаемые типы моделей включают следующие:

+ Модели, содержащие другие преобразования
+ Модели, использующие алгоритмы `rxGlm` или `rxNaiveBayes` в эквивалентах RevoScaleR или revoscalepy
+ Модели PMML
+ Модели, созданные с помощью других библиотек с открытым исходным кодом или библиотек сторонних разработчиков
::: moniker-end

## <a name="examples"></a>Примеры
::: moniker range="=azuresqldb-mi-current||=azure-sqldw-latest"
### <a name="predict-with-an-onnx-model"></a>PREDICT с моделью ONNX

В этом примере показано, как использовать модель ONNX, хранимую в таблице `dbo.models`, для собственной оценки.

```sql
DECLARE @model VARBINARY(max) = (
        SELECT DATA
        FROM dbo.models
        WHERE id = 1
        );

WITH predict_input
AS (
    SELECT TOP (1000) [id]
        , CRIM
        , ZN
        , INDUS
        , CHAS
        , NOX
        , RM
        , AGE
        , DIS
        , RAD
        , TAX
        , PTRATIO
        , B
        , LSTAT
    FROM [dbo].[features]
    )
SELECT predict_input.id
    , p.variable1 AS MEDV
FROM PREDICT(MODEL = @model, DATA = predict_input, RUNTIME=ONNX) WITH (variable1 FLOAT) AS p;
```

> [!NOTE]
> Поскольку столбцы и значения, возвращаемые функцией **PREDICT**, могут зависеть от типа модели, необходимо определить схему возвращаемых данных с помощью предложения **WITH**.
::: moniker-end

::: moniker range=">=sql-server-2017||=azuresqldb-mi-current||>=sql-server-linux-2017"
### <a name="predict-with-revoscale-model"></a>PREDICT с моделью RevoScale

В этом примере создается модель с помощью **RevoScaleR** в R, а затем вызывается функция прогнозирования в реальном времени из T-SQL.

#### <a name="step-1-prepare-and-save-the-model"></a>Шаг 1. Подготовка и сохранение модели

Выполните следующий код, чтобы создать пример базы данных и необходимые таблицы.

```sql
CREATE DATABASE NativeScoringTest;
GO
USE NativeScoringTest;
GO
DROP TABLE IF EXISTS iris_rx_data;
GO
CREATE TABLE iris_rx_data (
    "Sepal.Length" float not null, "Sepal.Width" float not null
  , "Petal.Length" float not null, "Petal.Width" float not null
  , "Species" varchar(100) null
);
GO
```

Используйте следующую инструкцию, чтобы заполнить таблицу данных данными из набора данных **iris**.

```sql
INSERT INTO iris_rx_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width" , "Species")
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'iris_data <- iris;'
  , @input_data_1 = N''
  , @output_data_1_name = N'iris_data';
GO
```

Теперь создайте таблицу для хранения моделей.

```sql
DROP TABLE IF EXISTS ml_models;
GO
CREATE TABLE ml_models ( model_name nvarchar(100) not null primary key
  , model_version nvarchar(100) not null
  , native_model_object varbinary(max) not null);
GO
```

Следующий код создает модель на основе набора данных **iris** и сохраняет его в таблицу **models**.

```sql
DECLARE @model varbinary(max);
EXECUTE sp_execute_external_script
  @language = N'R'
  , @script = N'
    iris.sub <- c(sample(1:50, 25), sample(51:100, 25), sample(101:150, 25))
    iris.dtree <- rxDTree(Species ~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width, data = iris[iris.sub, ])
    model <- rxSerializeModel(iris.dtree, realtimeScoringOnly = TRUE)
    '
  , @params = N'@model varbinary(max) OUTPUT'
  , @model = @model OUTPUT
  INSERT [dbo].[ml_models]([model_name], [model_version], [native_model_object])
  VALUES('iris.dtree','v1', @model) ;
```

> [!NOTE]
> Обязательно используйте функцию [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) из RevoScaleR для сохранения модели. Стандартная функция R `serialize` не может обеспечить необходимый формат сериализации.

Для просмотра сохраненной модели в двоичном формате можно выполнить следующую инструкцию:

```sql
SELECT *, datalength(native_model_object)/1024. as model_size_kb
FROM ml_models;
```

#### <a name="step-2-run-predict-on-the-model"></a>Шаг 2. Выполнение инструкции PREDICT для модели

Следующая простая инструкция PREDICT получает классификацию из модели дерева принятия решений с помощью функции **собственной оценки**. Она прогнозирует виды ирисов на основе указанных атрибутов: длины и ширины лепестка.

```sql
DECLARE @model varbinary(max) = (
  SELECT native_model_object
  FROM ml_models
  WHERE model_name = 'iris.dtree'
  AND model_version = 'v1');
SELECT d.*, p.*
  FROM PREDICT(MODEL = @model, DATA = dbo.iris_rx_data as d)
  WITH(setosa_Pred float, versicolor_Pred float, virginica_Pred float) as p;
go
```

Если вы получили сообщение об ошибке "Ошибка при выполнении функции PREDICT. Модель повреждена или является недопустимой", обычно это означает, что запрос не возвратил модель. Проверьте, правильно ли указано название модели и пуста ли таблица модели.

> [!NOTE]
> Поскольку столбцы и значения, возвращаемые функцией **PREDICT**, могут зависеть от типа модели, необходимо определить схему возвращаемых данных с помощью предложения **WITH**.
::: moniker-end

## <a name="next-steps"></a>Дальнейшие действия

+ [Функция PREDICT T-SQL](../../t-sql/queries/predict-transact-sql.md)
+ [Документация по машинному обучению на SQL](../index.yml)
+ [Машинное обучение и ИИ с применением ONNX в SQL для пограничных вычислений](/azure/azure-sql-edge/onnx-overview)
+ [Развертывание и создание прогнозов с помощью модели ONNX в SQL Azure для пограничных вычислений](/azure/azure-sql-edge/deploy-onnx)
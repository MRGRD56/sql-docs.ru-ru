---
title: sp_rxPredict
description: sp_rxPredict создает прогнозируемое значение для заданного ввода, состоящего из модели машинного обучения, хранящейся в двоичном формате в базе данных SQL Server.
ms.date: 04/05/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- sp_rxPredict
- sp_rxPredict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_rxPredict procedure
author: dphansen
ms.author: davidph
ms.custom: ''
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d4933bc344e1fb3ffade12e4ad9dd594ac8f90d6
ms.sourcegitcommit: ab0c654d924eeb5647e47444abb59d934345b205
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450187"
---
# <a name="sp_rxpredict"></a>sp_rxPredict  
[!INCLUDE [SQL Server 2016 Windows only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]

Создает прогнозируемое значение для заданного ввода, состоящего из модели машинного обучения, хранящейся в двоичном формате в базе данных SQL Server.

Обеспечивает оценку в моделях машинного обучения R и Python практически в реальном времени. `sp_rxPredict` — это хранимая процедура, предоставляемая как оболочка для 
- `rxPredict` Функция R в [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler) и [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)и функция [rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) Python в [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) и [MicrosoftML](/machine-learning-server/python-reference/microsoftml/microsoftml-package). Она написана на языке C++ и оптимизирована специально для операций оценки.

Несмотря на то, что модель должна быть создана с помощью R или Python, после ее сериализации и сохранения в двоичном формате в целевом экземпляре ядра СУБД его можно использовать в экземпляре ядра СУБД, даже если интеграция R или Python не установлена. Дополнительные сведения см. [в статье Оценка в реальном времени с помощью sp_rxPredict](../../machine-learning/predictions/real-time-scoring.md).

## <a name="syntax"></a>Синтаксис

```
sp_rxPredict  ( @model, @input )
```

### <a name="arguments"></a>Аргументы

**model**

Предварительно обученная модель в поддерживаемом формате. 

**input**

Допустимый SQL-запрос

### <a name="return-values"></a>Возвращаемые значения

Возвращается столбец оценки, а также все передаваемые столбцы из источника входных данных.
Дополнительные столбцы оценки, например доверительный интервал, могут быть возвращены, если алгоритм поддерживает создание таких значений.

## <a name="remarks"></a>Remarks

Чтобы включить использование хранимой процедуры, в экземпляре необходимо включить SQLCLR.

> [!NOTE]
> Включение этого параметра связано с вопросами безопасности. Используйте альтернативную реализацию, например функцию [прогнозирования Transact-SQL](../../t-sql/queries/predict-transact-sql.md?view=sql-server-2017&preserve-view=true) , если на сервере невозможно включить SQLCLR.

Пользователю требуется `EXECUTE` разрешение на базу данных.

### <a name="supported-algorithms"></a>Поддерживаемые алгоритмы

Чтобы создать и обучить модель, используйте один из поддерживаемых алгоритмов для R или Python, предоставляемый [SQL Server службы машинного обучения (R или Python)](../../machine-learning/sql-server-machine-learning-services.md), [SQL Server 2016 r Services](../../machine-learning/r/sql-server-r-services.md), [SQL Server Machine Learning Server (автономный) (r или Python)](../../machine-learning/r/r-server-standalone.md)или [SQL Server 2016 R Server (изолированный)](../../machine-learning/r/r-server-standalone.md?view=sql-server-2016&preserve-view=true).

#### <a name="r-revoscaler-models"></a>R: модели RevoScaleR

  + [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)\*
  + [rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit)\*
  + [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees)\*
  + [rxDtree](/machine-learning-server/r-reference/revoscaler/rxdtree)\*
  + [rxdForest](/machine-learning-server/r-reference/revoscaler/rxdforest)\*

Модели, помеченные \* также, поддерживают встроенную оценку с помощью `PREDICT` функции.

#### <a name="r-microsoftml-models"></a>R: модели MicrosoftML

  + [rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](/machine-learning-server/r-reference/microsoftml/rxfastlinear)

#### <a name="r-transformations-supplied-by-microsoftml"></a>R: преобразования, предоставляемые MicrosoftML

  + [featurizeText](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](/machine-learning-server/r-reference/microsoftml/selectFeatures)

#### <a name="python-revoscalepy-models"></a>Python: модели revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)\*
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit)\*
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees)\*
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree)\*
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest)\*

Модели, помеченные \* также, поддерживают встроенную оценку с помощью `PREDICT` функции.

#### <a name="python-microsoftml-models"></a>Python: модели microsoftml

  + [rx_fast_trees](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [rx_fast_forest](/machine-learning-server/python-reference/microsoftml/rx-fast-forest)
  + [rx_logistic_regression](/machine-learning-server/python-reference/microsoftml/rx-logistic-regression)
  + [rx_oneclass_svm](/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm)
  + [rx_neural_network](/machine-learning-server/python-reference/microsoftml/rx-neural-network)
  + [rx_fast_linear](/machine-learning-server/python-reference/microsoftml/rx-fast-linear)

#### <a name="python-transformations-supplied-by-microsoftml"></a>Python: преобразования, предоставляемые microsoftml

  + [featurize_text](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [concat](/machine-learning-server/python-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/python-reference/microsoftml/categorical)
  + [categorical_hash](/machine-learning-server/python-reference/microsoftml/categorical-hash)
  
### <a name="unsupported-model-types"></a>Неподдерживаемые типы моделей

Следующие типы моделей не поддерживаются:

+ Модели, использующие `rxGlm` `rxNaiveBayes` алгоритмы или в RevoScaleR.
+ Модели PMML в R.
+ Модели, созданные с помощью других сторонних библиотек.
+ модели, использующие функцию преобразования или формулу, содержащую преобразование, например `A ~ log(B`. Чтобы использовать модель этого типа, рекомендуется выполнить преобразование входных данных перед их передачей в систему оценки в реальном времени.

В режиме оценки в реальном времени не используется интерпретатор, поэтому все функции, которые могут потребовать интерпретатор, не поддерживаются на этапе оценки.

## <a name="examples"></a>Примеры

```sql
DECLARE @model = SELECT @model 
FROM model_table 
WHERE model_name = 'rxLogit trained';

EXEC sp_rxPredict @model = @model,
@inputData = N'SELECT * FROM data';
```

В дополнение к допустимому SQL-запросу входные данные в *\@ inputData* должны включать столбцы, совместимые со столбцами в хранимой модели.

`sp_rxPredict` поддерживает только следующие типы столбцов .NET: Double, float, Short, ushort, Long, ulong и String. Может потребоваться отфильтровать неподдерживаемые типы во входных данных, прежде чем использовать их для оценки в реальном времени.

Сведения о соответствующих типах SQL см. в статье [Сопоставление типов SQL и CLR](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping) или [Сопоставление данных о параметрах CLR](../clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

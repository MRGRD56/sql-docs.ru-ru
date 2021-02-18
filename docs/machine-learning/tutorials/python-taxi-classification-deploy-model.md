---
title: Учебник по Python. Запуск прогнозов в хранимых процедурах SQL
titleSuffix: SQL machine learning
description: В последнем, 5-м из серии руководств, вы ознакомитесь с внедрением встроенных скриптов Python в хранимые процедуры SQL с функциями T-SQL в машинном обучении SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 39717778ca2456d77b6ceff4ba72e8de06dac75f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339700"
---
# <a name="python-tutorial-run-predictions-using-python-embedded-in-a-stored-procedure"></a>Учебник по Python. Прогнозирования с помощью встроенного кода Python в хранимой процедуре
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

В части 5 этой серии учебников вы узнаете, как *внедрять* модели, обученные и сохраненные в предыдущей части.

В этом сценарии ввод в эксплуатацию означает развертывание модели в рабочей среде для оценки. Интеграция с SQL Server делает эту задачу довольно простой, так как код Python можно внедрить в хранимую процедуру. Чтобы получить прогнозы с помощью модели на основе новых входных данных, просто вызовите хранимую процедуру из приложения и передайте новые данные.

В этой части серии учебников демонстрируются два способа создания прогнозов на основе модели Python: пакетная оценка и построчная оценка.

+ **Пакетная оценка**. Чтобы предоставить несколько строк входных данных, передайте запрос SELECT в качестве аргумента хранимой процедуры. В результате будет получена таблица наблюдений, соответствующих входным вариантам.
+ **Индивидуальная оценка**. В качестве входных данных передается набор значений отдельных параметров.  Хранимая процедура возвращает одну строку или значение.

Весь код Python, необходимый для оценки, предоставляется в составе хранимых процедур.

Работая с этой статьей, вы узнаете о следующем.

> [!div class="checklist"]
> + Создание и использование хранимых процедур для пакетной оценки
> + Создание и использование хранимых процедур для оценки одной строки

В [первой части](python-taxi-classification-introduction.md) были установлены необходимые компоненты и восстановлена демонстрационная база данных.

Во [второй части](python-taxi-classification-explore-data.md) вы изучили образец данных и создали несколько графиков.

В [третьей части](python-taxi-classification-create-features.md) вы узнали, как создавать функции из необработанных данных с помощью функции Transact-SQL. Затем вы вызвали эту функцию из хранимой процедуры, чтобы создать таблицу, содержащую значения характеристик.

В [четвертой части](python-taxi-classification-train-model.md) вы научились загружать модули и вызывать необходимые функции для создания и обучения модели с помощью хранимой процедуры SQL Server.

## <a name="batch-scoring"></a>Пакетная оценка

Первые две хранимые процедуры иллюстрируют базовый синтаксис для заключения вызова прогноза Python в хранимую процедуру. Для обеих хранимых процедур в качестве входных данных требуется таблица.

+ Имя используемой модели указывается в качестве входного параметра хранимой процедуры. Хранимая процедура загружает сериализованную модель из таблицы базы данных `nyc_taxi_models`.table, используя инструкцию SELECT в хранимой процедуре.
+ Сериализованная модель сохраняется в переменной Python `mod` для дальнейшей обработки с помощью Python.
+ Новые варианты для оценки получаются с помощью запроса [!INCLUDE[tsql](../../includes/tsql-md.md)], указанного в `@input_data_1`. По мере считывания данных запроса строки сохраняются в кадре данных по умолчанию `InputDataSet`.
+ Обе хранимые процедуры используют функции из `sklearn` для вычисления метрики точности AUC (площадь под кривой). Для определения метрик точности, таких как AUC, необходимо также предоставить целевую метку (столбец _tipped_). Для прогнозов целевая метка (переменная `y`) не нужна, но для вычисления метрики точности — нужна.

  Поэтому если целевых меток для оцениваемых данных нет, можно изменить хранимую процедуру, удалив вычисления AUC, чтобы возвращалась только вероятность получения чаевых на основе признаков (переменная `X` в хранимой процедуре).

### <a name="predicttipscikitpy"></a>PredictTipSciKitPy

Чтобы создать хранимые процедуры, выполните приведенные ниже инструкции T-SQL. Для этой хранимой процедуры требуется модель на основе пакета scikit-learn, так как в ней используются функции, относящиеся к этому пакету.

Кадр данных, содержащий входные данные, передается в функцию `predict_proba` модели логистической регрессии `mod`. Функция `predict_proba` (`probArray = mod.predict_proba(X)`) возвращает значение типа **float**, которое представляет вероятность получения чаевых (в любом размере).

```sql
DROP PROCEDURE IF EXISTS PredictTipSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSciKitPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = mod.predict_proba(X)
probList = []
for i in range(len(probArray)):
  probList.append((probArray[i])[1])

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',  
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttiprxpy"></a>PredictTipRxPy

Эта хранимая процедура использует те же самые входные данные и создает такие же оценки, что и предыдущая хранимая процедура, но использует функции из пакета **revoscalepy**, входящего в состав службы машинного обучения SQL Server.

```sql
DROP PROCEDURE IF EXISTS PredictTipRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipRxPy] (@model varchar(50), @inquery nvarchar(max))
AS
BEGIN
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from sklearn import metrics
from revoscalepy.functions.RxPredict import rx_predict;

mod = pickle.loads(lmodel2)
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]
y = numpy.ravel(InputDataSet[["tipped"]])

probArray = rx_predict(mod, X)
probList = probArray["tipped_Pred"].values 

probArray = numpy.asarray(probList)
fpr, tpr, thresholds = metrics.roc_curve(y, probArray)
aucResult = metrics.auc(fpr, tpr)
print ("AUC on testing data is: " + str(aucResult))

OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @input_data_1_name = N'InputDataSet',
  @params = N'@lmodel2 varbinary(max)',
  @lmodel2 = @lmodel2
WITH RESULT SETS ((Score float));
END
GO
```

## <a name="run-batch-scoring-using-a-select-query"></a>Выполнение пакетной оценки с помощью запроса SELECT

Хранимым процедурам **PredictTipSciKitPy** и **PredictTipRxPy** требуются два входных параметра:

+ запрос, извлекающий данные для оценки;
+ имя обученной модели.

Путем передачи этих аргументов в хранимую процедуру можно выбрать конкретную модель или изменить данные, используемые для оценки.

1. Чтобы использовать модель **scikit-learn** для оценки, вызовите хранимую процедуру **PredictTipSciKitPy**, передав имя модели и строку запроса в качестве входных данных.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipSciKitPy] 'SciKit_model', @query_string;
   ```

   Хранимая процедура возвращает прогнозируемые вероятности для каждой поездки, переданной во входном запросе. 

   Если для выполнения запросов используется среда SSMS (SQL Server Management Studio), вероятности отобразятся в виде таблицы в области **Результаты**. В области **Сообщения** выводится метрика точности (AUC или площадь под кривой) со значением приблизительно 0,56.

2. Чтобы использовать модель **revoscalepy** для оценки, вызовите хранимую процедуру **PredictTipRxPy**, передав имя модели и строку запроса в качестве входных данных.

   ```sql
   DECLARE @query_string nvarchar(max) -- Specify input query
     SET @query_string='
     select tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance,
     dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance
     from nyctaxi_sample_testing'
   EXEC [dbo].[PredictTipRxPy] 'revoscalepy_model', @query_string;
   ```

## <a name="single-row-scoring"></a>Оценка одной строки

Иногда вместо пакетной оценки может потребоваться передать из приложения один вариант и получить один результат на его основе. Например, можно настроить лист Excel, веб-приложение или отчет так, чтобы они вызывали хранимую процедуру, передавая в нее входные значения, введенные или выбранные пользователями.

В этом разделе вы узнаете, как создавать отдельные прогнозы, вызывая две хранимые процедуры.

+ Процедура [PredictTipSingleModeSciKitPy](#predicttipsinglemodescikitpy) предназначена для оценки одной строки с помощью модели scikit-learn.
+ Процедура [PredictTipSingleModeRxPy](#predicttipsinglemoderxpy) предназначена для оценки одной строки с помощью модели revoscalepy.
+ Если вы еще не обучили модель, вернитесь к [части 5](python-taxi-classification-train-model.md).

Обе модели принимают в качестве входных данных ряд отдельных значений, таких как число пассажиров, расстояние поездки и т. д. Функция с табличным значением `fnEngineerFeatures` служит для преобразования значений широты и долготы из входных данных в новый признак — прямое расстояние. Эта функция описывается в [части 4](python-taxi-classification-create-features.md).

Обе хранимые процедуры создают оценку на основе модели Python.

> [!NOTE]
>
> При вызове хранимой процедуры из внешнего приложения важно предоставить все входные признаки, необходимые для модели Python. Чтобы избежать ошибок, может потребоваться привести или преобразовать входные данные в тип данных Python, а также проверить тип и длину данных.

### <a name="predicttipsinglemodescikitpy"></a>PredictTipSingleModeSciKitPy

Сначала изучите код хранимой процедуры, которая выполняет оценку с помощью модели **scikit-learn**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeSciKitPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeSciKitPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
  DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
    '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)
probList = []
probList.append((mod.predict_proba(X)[0])[1])

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="predicttipsinglemoderxpy"></a>PredictTipSingleModeRxPy

Приведенная ниже хранимая процедура выполняет оценку с помощью модели **revoscalepy**.

```sql
DROP PROCEDURE IF EXISTS PredictTipSingleModeRxPy;
GO

CREATE PROCEDURE [dbo].[PredictTipSingleModeRxPy] (@model varchar(50), @passenger_count int = 0,
  @trip_distance float = 0,
  @trip_time_in_secs int = 0,
  @pickup_latitude float = 0,
  @pickup_longitude float = 0,
  @dropoff_latitude float = 0,
  @dropoff_longitude float = 0)
AS
BEGIN
DECLARE @inquery nvarchar(max) = N'
  SELECT * FROM [dbo].[fnEngineerFeatures]( 
    @passenger_count,
    @trip_distance,
    @trip_time_in_secs,
    @pickup_latitude,
    @pickup_longitude,
    @dropoff_latitude,
    @dropoff_longitude)
  '
DECLARE @lmodel2 varbinary(max) = (select model from nyc_taxi_models where name = @model);
EXEC sp_execute_external_script 
  @language = N'Python',
  @script = N'
import pickle;
import numpy;
from revoscalepy.functions.RxPredict import rx_predict;

# Load model and unserialize
mod = pickle.loads(model)

# Get features for scoring from input data
X = InputDataSet[["passenger_count", "trip_distance", "trip_time_in_secs", "direct_distance"]]

# Score data to get tip prediction probability as a list (of float)

probArray = rx_predict(mod, X)

probList = []
probList = probArray["tipped_Pred"].values

# Create output data frame
OutputDataSet = pandas.DataFrame(data = probList, columns = ["predictions"])
',
  @input_data_1 = @inquery,
  @params = N'@model varbinary(max),@passenger_count int,@trip_distance float,
    @trip_time_in_secs int ,
    @pickup_latitude float ,
    @pickup_longitude float ,
    @dropoff_latitude float ,
    @dropoff_longitude float',
    @model = @lmodel2,
    @passenger_count =@passenger_count ,
    @trip_distance=@trip_distance,
    @trip_time_in_secs=@trip_time_in_secs,
    @pickup_latitude=@pickup_latitude,
    @pickup_longitude=@pickup_longitude,
    @dropoff_latitude=@dropoff_latitude,
    @dropoff_longitude=@dropoff_longitude
WITH RESULT SETS ((Score float));
END
GO
```

### <a name="generate-scores-from-models"></a>Создание оценок на основе моделей

После создания хранимых процедур можно легко создать оценку на основе любой из двух моделей. Просто откройте новое окно **Запрос** и введите или вставьте параметры для каждого из столбцов признаков. Семь обязательных значений для столбцов признаков указываются в следующем порядке:

+ *passenger_count*
+ *trip_distance*
+ *trip_time_in_secs*
+ *pickup_latitude*
+ *pickup_longitude*
+ *dropoff_latitude*
+ *dropoff_longitude*

1. Чтобы создать прогноз с помощью модели **revoscalepy**, выполните следующую инструкцию:
  
   ```sql
   EXEC [dbo].[PredictTipSingleModeRxPy] 'revoscalepy_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

2. Чтобы создать оценку с помощью модели **scikit-learn**, выполните следующую инструкцию:

   ```sql
   EXEC [dbo].[PredictTipSingleModeSciKitPy] 'SciKit_model', 1, 2.5, 631, 40.763958,-73.973373, 40.782139,-73.977303
   ```

Результат обеих процедур представляет собой вероятность получения чаевых для поездки в такси с указанными параметрами или признаками.

## <a name="conclusions"></a>Выводы

В этой серии учебников вы узнали, как работать с кодом на языке Python, внедренным в хранимые процедуры. Интеграция с [!INCLUDE[tsql](../../includes/tsql-md.md)] значительно упрощает развертывание моделей Python для прогнозирования и включение переобучения моделей в корпоративные рабочие процессы по обработке данных.

## <a name="next-steps"></a>Дальнейшие шаги

Работая с этой статьей, вы выполните следующие задачи:

> [!div class="checklist"]
> + Созданные и используемые хранимые процедуры для пакетной оценки
> + Созданные и используемые хранимые процедуры для оценки одной строки

Дополнительные сведения о Python см. в разделе [Расширение Python в SQL Server](../concepts/extension-python.md).

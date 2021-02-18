---
title: Учебник по Python. Создание характеристик данных
titleSuffix: SQL machine learning
description: В третьей части этой серии учебников вы добавите вычисления в хранимые процедуры для использования в моделях машинного обучения Python с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 630a5db712e3bd6e288c0ac12ca767a07c3ac4ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339715"
---
# <a name="python-tutorial-create-data-features-using-t-sql"></a>Учебник по Python. Создание характеристик данных с помощью T-SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Из третьей части этой серии из пяти учебников вы узнаете, как создавать характеристики на основе необработанных данных с помощью функции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Затем вы вызовите эту функцию из хранимой процедуры SQL, чтобы создать таблицу, содержащую значения характеристик.

Процесс *конструирования признаков* (создание признаков на основе необработанных данных) — важнейший этап расширенного аналитического моделирования.

Работая с этой статьей, вы узнаете о следующем.

> [!div class="checklist"]
> + Изменение пользовательской функции для вычисления расстояния поездки
> + Сохранение признаков с помощью другой пользовательской функции

В [первой части](python-taxi-classification-introduction.md) были установлены необходимые компоненты и восстановлена демонстрационная база данных.

Во [второй части](python-taxi-classification-explore-data.md) вы изучили образец данных и создали несколько графиков.

В [четвертой части](python-taxi-classification-train-model.md) вы научитесь загружать модули и вызывать необходимые функции для создания и обучения модели с помощью хранимой процедуры SQL Server.

Из [пятой части](python-taxi-classification-deploy-model.md) вы узнаете, как ввести в эксплуатацию модели, которые были обучены и сохранены в соответствии с инструкциями в четвертой части.

## <a name="define-the-function"></a>Определение функции

Значения расстояний, которые содержатся в исходных данных, основаны на показаниях счетчиков и необязательно отражают расстояние по карте или фактическое расстояние поездки. Поэтому необходимо вычислить прямое расстояние между местами посадки и высадки с помощью координат, доступных в исходном наборе данных по работе такси в Нью-Йорке. Это можно сделать с помощью [формулы гаверсинуса](https://en.wikipedia.org/wiki/Haversine_formula) в пользовательской функции [!INCLUDE[tsql](../../includes/tsql-md.md)] .

Вы используете пользовательскую функцию T-SQL _fnCalculateDistance_ для вычисления расстояния по формуле гаверсинуса, а затем другую пользовательскую функцию T-SQL _fnEngineerFeatures_ для создания таблицы, содержащей все характеристики.

### <a name="calculate-trip-distance-using-fncalculatedistance"></a>Вычисление расстояния поездки с помощью функции fnCalculateDistance

1. Функция _fnCalculateDistance_ включена в образец базы данных. Вкратце изучите код.
  
2. В среде [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]последовательно разверните узлы **Программируемость**, **Функции** и **Скалярные функции**.
   Щелкните правой кнопкой мыши функцию _fnCalculateDistance_, а затем выберите команду **Изменить** , чтобы открыть скрипт [!INCLUDE[tsql](../../includes/tsql-md.md)] в новом окне запроса.
  
   ```sql
   CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
   -- User-defined function that calculates the direct distance between two geographical coordinates
   RETURNS float
   AS
   BEGIN
     DECLARE @distance decimal(28, 10)
     -- Convert to radians
     SET @Lat1 = @Lat1 / 57.2958
     SET @Long1 = @Long1 / 57.2958
     SET @Lat2 = @Lat2 / 57.2958
     SET @Long2 = @Long2 / 57.2958
     -- Calculate distance
     SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
     --Convert to miles
     IF @distance <> 0
     BEGIN
       SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
     END
     RETURN @distance
   END
   GO
   ```

**Примечания.**

+ Функция является скалярной и возвращает одно значение предопределенного типа.
+ Она принимает в качестве входных значений широту и долготу мест посадки и высадки. Формула гаверсинуса преобразует координаты в радианы и использует полученные значения для вычисления прямого расстояния в километрах между этими двумя местами.

Для добавления вычисленного значения в таблицу, которую можно использовать для обучения модели, применяется другая функция: _fnEngineerFeatures_.

### <a name="save-the-features-using-_fnengineerfeatures_"></a>Сохранение характеристик с помощью функции _fnEngineerFeatures_

1. Вкратце ознакомьтесь с кодом настраиваемой функции T-SQL, _fnEngineerFeatures_, которая включена в состав образца базы данных.
  
   Это табличная функция, которая принимает несколько столбцов в качестве входных значений и выводит таблицу с несколькими столбцами характеристик.  Назначение этой функции — создать набор характеристик, который будет использоваться при построении модели. Функция _fnEngineerFeatures_ вызывает ранее созданную функцию T-SQL _fnCalculateDistance_ для получения прямого расстояния между местами посадки и высадки.
  
   ```sql
   CREATE FUNCTION [dbo].[fnEngineerFeatures] (
   @passenger_count int = 0,
   @trip_distance float = 0,
   @trip_time_in_secs int = 0,
   @pickup_latitude float = 0,
   @pickup_longitude float = 0,
   @dropoff_latitude float = 0,
   @dropoff_longitude float = 0)
   RETURNS TABLE
   AS
     RETURN
     (
     -- Add the SELECT statement with parameter references here
     SELECT
       @passenger_count AS passenger_count,
       @trip_distance AS trip_distance,
       @trip_time_in_secs AS trip_time_in_secs,
       [dbo].[fnCalculateDistance](@pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude) AS direct_distance
     )
   GO
   ```
  
2. Чтобы убедиться в том, что эта функция работает, можно с ее помощью вычислить географическое расстояние поездок, для которых расстояние по счетчику было равно 0, но места посадки и высадки были разными.
  
   ```sql
       SELECT tipped, fare_amount, passenger_count,(trip_time_in_secs/60) as TripMinutes,
       trip_distance, pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) AS direct_distance
       FROM nyctaxi_sample
       WHERE pickup_longitude != dropoff_longitude and pickup_latitude != dropoff_latitude and trip_distance = 0
       ORDER BY trip_time_in_secs DESC
   ```
  
   Как видите, расстояние по счетчику не всегда соответствует географическому расстоянию. Вот почему формирование характеристик имеет такое большое значение.

В следующей части вы научитесь использовать характеристики данных для создания и обучения модели машинного обучения с помощью языка Python.

## <a name="next-steps"></a>Дальнейшие шаги

Работая с этой статьей, вы выполните следующие задачи:

> [!div class="checklist"]
> + Изменение пользовательской функции для вычисления расстояния поездки
> + Сохранение признаков с помощью другой пользовательской функции

> [!div class="nextstepaction"]
> [Учебник по Python. Обучение и сохранение модели Python с помощью T-SQL](python-taxi-classification-train-model.md)
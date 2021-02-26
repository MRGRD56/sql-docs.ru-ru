---
title: Учебник по R. Конструирование признаков
description: Учебник, показывающий, как создавать признаки данных с помощью функций SQL Server для аналитики в базе данных.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 502600980a3cb1f80eaeca64284f0a189cfcb405
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272985"
---
# <a name="create-data-features-using-r-and-sql-server-walkthrough"></a>Создание признаков данных с помощью R и SQL Server (пошаговое руководство)
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

Проектирование данных — это важная составляющая машинного обучения. Прежде чем использовать данные для прогнозирующего моделирования, их зачастую требуется преобразовать. Если данные не имеют требуемых характеристик, их необходимо создать на основе существующих значений.

Для этой задачи моделирования вместо необработанных значений широты и долготы мест посадки и высадки желательно использовать значения расстояния в километрах между этими двумя местами. Чтобы создать такой признак, необходимо вычислить прямое линейное расстояние между двумя точками по [формуле гаверсинуса](https://en.wikipedia.org/wiki/Haversine_formula).

На этом этапе вы узнаете о двух разных способах создания признаков на основе данных:

> [!div class="checklist"]
> * Использование пользовательской функции R
> * Использование пользовательской функции T-SQL в [!INCLUDE[tsql](../../includes/tsql-md.md)]

Целью является создание нового набора данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], включающего в себя исходные столбцы и новый числовой признак — *direct_distance*.

## <a name="prerequisites"></a>Предварительные условия

Для этого этапа требуется продолжение сеанса R из предыдущих этапов этого пошагового руководства. В нем используются строки подключения и объекты источников данных, созданные на этих этапах. Для запуска скрипта используются следующие средства и пакеты.

+ Rgui.exe для выполнения команд R
+ Management Studio для выполнения T-SQL

## <a name="featurization-using-r"></a>Добавление признаков с помощью R

Язык R известен своими статистическими библиотеками с широкими и разнообразными возможностями, но вам все еще нужно создавать пользовательские преобразования данных.

Во-первых, давайте сделаем так, как привыкли пользователи R: получите данные на ноутбук, а затем запустите пользовательскую функцию R *ComputeDist*, которая вычисляет линейное расстояние между двумя точками, заданными значениями широты и долготы.

1. Помните, что созданный ранее объект источника данных возвращает только первые 1000 строк. Итак, давайте определим запрос, который получает все данные.

    ```R
    bigQuery <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,  pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude FROM nyctaxi_sample";
    ```

2. Создайте новый объект источника данных с помощью запроса.

    ```R
    featureDataSource <- RxSqlServerData(sqlQuery = bigQuery,colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric", dropoff_longitude = "numeric", dropoff_latitude = "numeric", passenger_count  = "numeric", trip_distance  = "numeric", trip_time_in_secs  = "numeric", direct_distance  = "numeric"), connectionString = connStr);
    ```

    - [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata) может принимать либо запрос, состоящий из допустимого запроса SELECT, предоставленный в качестве аргумента для параметра _sqlQuery_, либо имя объекта таблицы, предоставленное в качестве параметра _table_.
    
    - Если необходимо выполнить выборку данных из таблицы, используйте параметр _sqlQuery_, определите параметры выборки с помощью предложения T-SQL TABLESAMPLE и задайте для аргумента _rowBuffering_ значение FALSE.

3. Выполните следующий код для создания пользовательской функции R. ComputeDist принимает две пары значений широты и долготы и вычисляет линейное расстояние между ними, возвращая расстояние в милях.

    ```R
    env <- new.env();
    env$ComputeDist <- function(pickup_long, pickup_lat, dropoff_long, dropoff_lat){
      R <- 6371/1.609344 #radius in mile
      delta_lat <- dropoff_lat - pickup_lat
      delta_long <- dropoff_long - pickup_long
      degrees_to_radians = pi/180.0
      a1 <- sin(delta_lat/2*degrees_to_radians)
      a2 <- as.numeric(a1)^2
      a3 <- cos(pickup_lat*degrees_to_radians)
      a4 <- cos(dropoff_lat*degrees_to_radians)
      a5 <- sin(delta_long/2*degrees_to_radians)
      a6 <- as.numeric(a5)^2
      a <- a2+a3*a4*a6
      c <- 2*atan2(sqrt(a),sqrt(1-a))
      d <- R*c
      return (d)
    }
    ```
  
    + В первой строке определяется новая среда. В R среду можно использовать для инкапсуляции пространств имен в пакетах и отдельно. С помощью функции `search()` можно просмотреть среды, имеющиеся в рабочем пространстве. Чтобы просмотреть объекты в определенной среде, введите `ls(<envname>)`.
    + Строки начиная с `$env.ComputeDist` содержат код, который определяет формулу гаверсинуса, вычисляющую *ортодромическое расстояние* между двумя точками на сфере.

4. Определив функцию, вы примените ее к данным, чтобы создать столбец признаков *direct_distance*. Но перед преобразованием измените контекст вычислений на локальный.

    ```R
    rxSetComputeContext("local");
    ```

5. Вызовите функцию [rxDataStep](/r-server/r-reference/revoscaler/rxdatastep), чтобы получить данные о проектировании признаков, и примените функцию `env$ComputeDist` к данным в памяти.

    ```R
    start.time <- proc.time();
  
    changed_ds <- rxDataStep(inData = featureDataSource,
    transforms = list(direct_distance=ComputeDist(pickup_longitude,pickup_latitude, dropoff_longitude, dropoff_latitude),
    tipped = "tipped", fare_amount = "fare_amount", passenger_count = "passenger_count",
    trip_time_in_secs = "trip_time_in_secs",  trip_distance="trip_distance",
    pickup_datetime = "pickup_datetime",  dropoff_datetime = "dropoff_datetime"),
    transformEnvir = env,
    rowsPerRead=500,
    reportProgress = 3);
  
    used.time <- proc.time() - start.time;
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""));
    ```

    + Функция rxDataStep поддерживает различные методы изменения данных на месте. Дополнительные сведения см. в статье [Преобразование данных и выделение подмножеств в Microsft R](/r-server/r/how-to-revoscaler-data-transform).
    
    Однако стоит отметить несколько моментов, касающихся rxDataStep. 
    
    В других источниках данных можно использовать аргументы *varsToKeep* и *varsToDrop*, но они не поддерживаются для источников данных SQL Server. Поэтому в этом примере мы использовали аргумент _transforms_, чтобы указать как сквозные столбцы, так и преобразованные. Кроме того, при выполнении в контексте вычислений SQL Server аргумент _inData_ может принимать только источник данных SQL Server.

    Приведенный выше код может также выдавать предупреждающее сообщение при выполнении в больших наборах данных. Если произведение создаваемых строк и столбцов превышает заданное значение (по умолчанию — 3 000 000), rxDataStep возвращает предупреждение, а число строк в возвращенном кадре данных будет обрезано. Чтобы устранить это предупреждение, можно изменить аргумент _maxRowsByCols_ в функции rxDataStep. Однако если значение _maxRowsByCols_ слишком большое, могут возникнуть проблемы при загрузке кадра данных в память.

7. Кроме того, вы можете вызвать функцию [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo), чтобы проверить схему преобразованного нового источника данных.

    ```R
    rxGetVarInfo(data = changed_ds);
    ```

## <a name="featurization-using-transact-sql"></a>Добавление признаков с помощью Transact-SQL

В этом упражнении вы узнаете, как выполнить ту же задачу с помощью функций SQL, а не пользовательских функций R. 

Для запуска скрипта T-SQL перейдите в [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) или другой редактор запросов.

1. Используйте функцию SQL с именем *fnCalculateDistance*. Функция уже должна существовать в базе данных NYCTaxi_Sample. В обозревателе объектов убедитесь в том, что функция существует, перейдя по пути "Базы данных > NYCTaxi_Sample > Программируемость > Функции > Скалярные функции > dbo.fnCalculateDistance".

    Если функция не существует, создайте функцию в базе данных NYCTaxi_Sample, используя SQL Server Management Studio.

    ```sql
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
    -- User-defined function calculates the direct distance between two geographical coordinates.
    RETURNS decimal(28, 10)
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
    ```

2. В Management Studio в новом окне запросов выполните следующую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] из любого приложения, которое поддерживает [!INCLUDE[tsql](../../includes/tsql-md.md)], чтобы увидеть, как работает функция.

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude 
    FROM nyctaxi_sample
    ```
3. Чтобы вставить значения непосредственно в новую таблицу (сначала необходимо создать ее), можно добавить предложение **INTO**, указав имя таблицы.

    ```sql
    USE nyctaxi_sample
    GO

    SELECT tipped, fare_amount, passenger_count, trip_time_in_secs, trip_distance, pickup_datetime, dropoff_datetime,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) as direct_distance, pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude
    INTO NewFeatureTable
    FROM nyctaxi_sample
    ```

4. Вы также можете вызвать функцию SQL из кода R. Вернитесь в Rgui и сохраните запрос добавления признаков SQL в переменной R.

    ```R
    featureEngineeringQuery = "SELECT tipped, fare_amount, passenger_count,
        trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime,
        dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) as direct_distance,
        pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude
        FROM nyctaxi_sample
        tablesample (1 percent) repeatable (98052)"
    ```
  
    > [!TIP]
    > Запрос был изменен для того, чтобы уменьшить выборку данных и ускорить работу с этим пошаговым руководством. Если требуется получить все данные, можно удалить предложение TABLESAMPLE. Однако в зависимости от среды загрузка полного набора данных R может оказаться невозможной, что приведет к ошибке.
  
5. Используйте приведенные ниже строки кода, чтобы вызвать функцию [!INCLUDE[tsql](../../includes/tsql-md.md)] из среды R и применить ее к данным, определенным в *featureEngineeringQuery*.
  
    ```R
    featureDataSource = RxSqlServerData(sqlQuery = featureEngineeringQuery,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
        dropoff_longitude = "numeric", dropoff_latitude = "numeric",
        passenger_count  = "numeric", trip_distance  = "numeric",
        trip_time_in_secs  = "numeric", direct_distance  = "numeric"),
      connectionString = connStr)
    ```
  
6.  Теперь, когда признак создан, вызовите **rxGetVarsInfo**, чтобы получить сводку данных в таблице признаков.
  
    ```R
    rxGetVarInfo(data = featureDataSource)
    ```

    **Результаты**

    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: numeric
    Var 4: trip_time_in_secs, Type: numeric
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: direct_distance, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: pickup_longitude, Type: numeric
    Var 11: dropoff_latitude, Type: numeric
    Var 12: dropoff_longitude, Type: numeric
    ```

    > [!NOTE]
    > Иногда может возникать ошибка наподобие следующей: *Разрешение EXECUTE было отклонено для объекта "fnCalculateDistance"* . В этом случае убедитесь в том, что используемое имя входа имеет разрешения на выполнение скриптов и создание объектов в базе данных, а не только в экземпляре.
    > Проверьте схему для объекта fnCalculateDistance. Если объект был создан владельцем базы данных, а имя входа принадлежит роли db_datareader, то необходимо предоставить имени входа явные разрешения на запуск скрипта.

## <a name="comparing-r-functions-and-sql-functions"></a>Сравнение функций R с функциями SQL

Помните этот фрагмент кода, используемый для времени в коде R?

```R
start.time <- proc.time()
<your code here>
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds, Elapsed Time=", round(used.time[3],2), " seconds to generate features.", sep=""))
```

Можно попробовать использовать его с примером пользовательской функции SQL, чтобы узнать, как долго выполняется преобразование данных при вызове функции SQL. Кроме того, попробуйте переключить контексты вычислений с помощью rxSetComputeContext и сравнить временные показатели.

Время может значительно различаться в зависимости от скорости сети и конфигурации оборудования. В тестируемых конфигурациях функция [!INCLUDE[tsql](../../includes/tsql-md.md)] была быстрее, чем пользовательская функция R. Поэтому на последующих этапах мы использовали функцию [!INCLUDE[tsql](../../includes/tsql-md.md)] для этих вычислений.

> [!TIP]
> Зачастую формирование признаков с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)] выполняется быстрее, чем с помощью языка R. Например, T-SQL включает агрегатные и ранжирующие функции, которые можно применять в распространенных вычислениях для обработки и анализа данных, например вычисление скользящих средних и *n*-tile. Выберите наиболее эффективный способ в зависимости от особенностей данных и поставленной задачи.

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание модели R и ее сохранение в SQL](walkthrough-build-and-save-the-model.md)
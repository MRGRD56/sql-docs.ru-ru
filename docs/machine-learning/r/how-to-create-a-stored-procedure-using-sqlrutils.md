---
title: Создание функции из кода R, объединенного в пакет с помощью пакета sqlrutils
description: Используйте пакет R sqlrutils в SQL Server для объединения кода языка R в одну функцию, которая может быть передана в качестве аргумента хранимой процедуре.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 04bcac677a4cdbf3e96f60bb43244f2cc6a0eec7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336276"
---
# <a name="create-a-stored-procedure-using-sqlrutils"></a>Создание хранимой процедуры с помощью sqlrutils
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

В этой статье описывается преобразование кода R для запуска в качестве хранимой процедуры T-SQL. Для достижения наилучших результатов может потребоваться немного изменить код, чтобы обеспечить возможность параметризации всех входных данных.

## <a name="step-1-rewrite-r-script"></a><a name="bkmk_rewrite"></a>Шаг 1. Переписывание кода для сценария R

Для получения наилучших результатов следует переписать код R, чтобы инкапсулировать его как единую функцию.

Все переменные, используемые функцией, должны быть определены внутри функции или в качестве входных параметров. См. [пример кода](#samples) в этой статье.

Также поскольку входные параметры функции станут входными параметрами хранимой процедуры SQL, необходимо убедиться, что входные и выходные данные отвечают следующим требованиям к типам:

### <a name="inputs"></a>Входные данные

Среди входных параметров может быть не более одного кадра данных.

Объекты внутри кадра данных, а также все другие входные параметры функции должны относиться к следующим типам данных R:
- POSIXct
- NUMERIC
- character
- integer
- логические
- raw

Если тип входных данных не является одним из перечисленных выше, он должен быть сериализован и передан в функцию в виде *raw*. В этом случае функция также должна включать код для десериализации входных данных.

### <a name="outputs"></a>Выходные данные

Функция может иметь один из следующих вариантов вывода:

- Кадр данных, содержащий поддерживаемые типы данных. Все объекты в кадре данных должны использовать один из поддерживаемых типов данных.
- Именованный список, содержащий не более одного кадра данных. Все элементы в списке должны использовать один из поддерживаемых типов данных.
- Значение NULL, если функция не возвращает никакого результата.

## <a name="step-2-generate-required-objects"></a>Шаг 2. Создание необходимых объектов

После очистки кода R его можно вызвать как отдельную функцию, и теперь вам необходимо использовать функции из пакета **sqlrutils** для подготовки входных и выходных данных в форме, которая может быть передана в конструктор, который фактически создает хранимую процедуру.

Пакет **sqlrutils** предоставляет функции, которые определяют схему и тип входных данных, а также схему и тип выходных данных. Он также включает функции, которые могут преобразовывать объекты R к необходимому выходному типу. Можно выполнить несколько вызовов функций, чтобы создать необходимые объекты в зависимости от типов данных, используемых в вашем коде.

### <a name="inputs"></a>Входные данные

Если ваша функция принимает входные данные, для каждого входного значения вызывайте следующие функции:

- `setInputData`, если входные данные представляют собой кадр данных.
- `setInputParameter` для всех остальных типов входных данных.

При выполнении каждого вызова функции создается объект R, который позже будет передан в качестве аргумента в `StoredProcedure`, чтобы создать полную хранимую процедуру.

### <a name="outputs"></a>Выходные данные

**sqlrutils** предоставляет несколько функций для преобразования объектов R, таких как списки, в кадр данных (data.frame), необходимый для SQL Server.
Если функция выводит кадр данных напрямую, без первоначального объединения его в список, этот шаг можно пропустить.
Вы также можете пропустить это преобразование, если ваша функция возвращает значение NULL.

При преобразовании списка или получении определенного элемента из списка выберите следующие функции:

- Если переменная, которую необходимо получить из списка, является кадром данных, используйте `setOutputData`.
- Для всех остальных элементов списка используйте `setOutputParameter`.

При выполнении каждого вызова функции создается объект R, который позже будет передан в качестве аргумента в `StoredProcedure`, чтобы создать полную хранимую процедуру.

## <a name="step-3-generate-the-stored-procedure"></a>Шаг 3. Создание хранимой процедуры

Когда все входные и выходные параметры готовы, выполните вызов конструктора `StoredProcedure`.

**Использование**

`StoredProcedure (func, spName, ..., filePath = NULL ,dbName = NULL, connectionString = NULL, batchSeparator = "GO")`

Предположим, что необходимо создать хранимую процедуру с именем **sp_rsample** со следующими параметрами:

- Использует существующую функцию **foosql**. Функция была основана на существующем коде в функции R **foo**, но вы переписали функцию в соответствии с требованиями, описанными в [этом разделе](#bkmk_rewrite), и назвали обновленную функцию **foosql**.
- Использует кадр данных **queryinput** в качестве входных данных.
- Создает кадр данных с именем переменной R **sqloutput** в качестве выходных данных.
- Необходимо создать код T-SQL в виде файла в папке `C:\Temp`, чтобы позже его можно было запустить с помощью SQL Server Management Studio.

```R
StoredProcedure (foosql, sp_rsample, queryinput, sqloutput, filePath = "C:\\Temp")
```

> [!NOTE]
> Поскольку файл записывается в файловую систему, можно опустить аргументы, определяющие подключение к базе данных.

Выходные данные функции — это хранимая процедура T-SQL, которая может быть выполнена на экземпляре SQL Server 2016 (требуются Службы R) или SQL Server 2017 (требуется Службы машинного обучения с R). 

Дополнительные примеры см. в справке по пакету. Чтобы открыть ее, вызовите `help(StoredProcedure)` в среде R.

## <a name="step-4-register-and-run-the-stored-procedure"></a>Шаг 4. Регистрация и выполнение хранимой процедуры

Существует два способа выполнения хранимой процедуры:

- С помощью T-SQL из любого клиента, поддерживающего подключения к экземпляру SQL Server 2016 или SQL Server 2017
- В среде R

Для обоих методов требуется, чтобы хранимая процедура была зарегистрирована в базе данных, в которой предполагается использовать хранимую процедуру.

### <a name="register-the-stored-procedure"></a>Регистрация хранимой процедуры

Хранимую процедуру можно зарегистрировать с помощью языка R или выполнив инструкцию CREATE PROCEDURE в T-SQL.

- Использование T-SQL.  Если вы знакомы с T-SQL, откройте SQL Server Management Studio (или любой другой клиент, который может выполнять команды SQL DDL), и выполните инструкцию CREATE PROCEDURE, используя код, подготовленный функцией `StoredProcedure`.
- Использование языка R. Пока вы все еще находитесь в среде R, вы можете использовать функцию `registerStoredProcedure` в **sqlrutils** для регистрации хранимой процедуры в базе данных.

  Например, можно зарегистрировать хранимую процедуру **sp_rsample** в экземпляре и базе данных, определенных в *sqlConnStr*, выполнив следующий вызов R:

  ```R
  registerStoredProcedure(sp_rsample, sqlConnStr)
  ```


> [!IMPORTANT]
> Независимо от того, используете ли вы R или SQL, необходимо выполнить инструкцию от имени учетной записи, у которой есть разрешения на создание новых объектов базы данных.

### <a name="run-using-sql"></a>Выполнение с помощью SQL

После создания хранимой процедуры откройте подключение к базе данных SQL с помощью любого клиента, поддерживающего T-SQL, и передайте значения всех параметров, необходимых для хранимой процедуры.

### <a name="run-using-r"></a>Выполнение с помощью R

Если вы хотите выполнить хранимую процедуру из кода R, а не на SQL Server, требуется дополнительная подготовка. Например, если этой хранимой процедуре требуются входные значения, следует задать эти входные параметры до выполнения функции и передать эти объекты в хранимую процедуру в коде R.

Общий процесс вызова подготовленной хранимой процедуры SQL выглядит следующим образом:

1. Вызовите `getInputParameters` для получения списка объектов входных параметров.
2. Определение `$query` или задайте `$value` для каждого входного параметра.
3. Используйте `executeStoredProcedure` для выполнения хранимой процедуры из среды разработки R, передавая список заданных объектов входных параметров.

## <a name="example"></a><a name = "samples"></a>Пример

В этом примере показаны предыдущая и последующая версии сценария R, который получает данные из базы данных SQL Server, выполняет некоторые преобразования данных и сохраняет их в другой базе данных.

Этот простой пример служит лишь для демонстрации того, как можно изменить код R, чтобы упростить его для преобразования в хранимую процедуру.

### <a name="before-code-preparation"></a>Перед подготовкой кода


```R
sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineSrc;Trusted_Connection=Yes;"
  
sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineTest;Trusted_Connection=Yes;"
  
sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"
  
dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
xFunc <- function(data) {
    data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
    return(data)
    }
  
xVars <- c("CRSDepTime")
  
sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
rxOpen(dsSqlFrom)
rxOpen(dsSqlTo)
  
if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo))   {
    rxSqlServerDropTable("cleanData")}
  
rxDataStep(inData = dsSqlFrom, 
     outFile = dsSqlTo,
     transformFunc = xFunc,
     transformVars = xVars,
     overwrite = TRUE)
```

> [!NOTE]
> 
> При использовании соединения ODBC вместо вызова функции *RxSqlServerData* перед выполнением операций в базе данных следует открыть соединение с помощью функции *rxOpen*.


### <a name="after-code-preparation"></a>После подготовки кода

В обновленной версии первая строка определяет имя функции. Весь остальной код из исходного решения R становится частью этой функции.

```R
myetl1function <- function() { 
   sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=Airline01;Trusted_Connection=Yes;"
   sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer02;Database=Airline02;Trusted_Connection=Yes;"
    
   sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"

   dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
   dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
   xFunc <- function(data) {
     data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
     return(data)}
  
   xVars <- c("CRSDepTime")
  
   sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
   if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo)) {rxSqlServerDropTable("cleanData")}
  
   rxDataStep(inData = dsSqlFrom, 
        outFile = dsSqlTo,
        transformFunc = xFunc,
        transformVars = xVars,
        overwrite = TRUE)
   return(NULL)
}
```

> [!NOTE]
> 
> Хотя вам и не требуется явно открывать соединение ODBC внутри кода, оно по-прежнему должно использовать **sqlrutils**.

## <a name="see-also"></a>См. также:

[sqlrutils (SQL)](ref-r-sqlrutils.md)



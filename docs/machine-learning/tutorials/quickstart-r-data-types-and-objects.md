---
title: Краткое руководство. Структуры данных, типы данных и объекты R
titleSuffix: SQL machine learning
description: В этом кратком руководстве вы узнаете, как применять структуры данных, типы данных и объекты при использовании R и машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 392ab990d33d5686fa5cdd3e4bb2f6a39b592410
ms.sourcegitcommit: 9b41725d6db9957dd7928a3620fe4db41eb51c6e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2020
ms.locfileid: "88178505"
---
# <a name="quickstart-data-structures-data-types-and-objects-using-r-with-sql-machine-learning"></a>Краткое руководство. Использование структур данных, типов данных и объектов при работе с R и машинным обучением SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
В этом кратком руководстве вы узнаете, как использовать структуры и типы данных при применении R в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) или [Кластерах больших данных](../../big-data-cluster/machine-learning-services.md). Вы узнаете о том, как перемещать данные между R и SQL Server, и о типичных проблемах, возникающих при этом процессе.
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
В этом кратком руководстве вы узнаете, как использовать структуры и типы данных при работе с R в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md). Вы узнаете о том, как перемещать данные между R и SQL Server, и о типичных проблемах, возникающих при этом процессе.
::: moniker-end
::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
В этом кратком руководстве вы узнаете, как использовать структуры и типы данных при работе с R в службах [SQL Server R Services](../r/sql-server-r-services.md). Вы узнаете о том, как перемещать данные между R и SQL Server, и о типичных проблемах, возникающих при этом процессе.
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
В этом кратком руководстве вы узнаете, как использовать структуры и типы данных при работе с R в [Службах машинного обучения управляемого экземпляра SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Вы узнаете о том, как перемещать данные между R и Управляемым экземпляром SQL, и о типичных проблемах, возникающих при этом процессе.
::: moniker-end

Основные проблемы, о которых следует знать в самом начале, таковы:

- Иногда типы данных не совпадают
- Могут иметь место неявные преобразования
- Иногда требуются операции приведения и преобразования.
- R и SQL используют разные объекты данных.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством необходимо следующее.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
- Службы машинного обучения SQL Server. Сведения об установке Служб машинного обучения см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json). Можно также [включить Службы машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
- Службы машинного обучения SQL Server. Сведения об установке Служб машинного обучения см. в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md). 
::: moniker-end
::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
- SQL Server 2016 R Services. Сведения об установке служб R Services см. в [руководстве по установке для Windows](../install/sql-r-services-windows-install.md). 
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
- Службы машинного обучения в управляемом экземпляре SQL Azure. Сведения о регистрации см. в статье [Общие сведения о Службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

- Инструмент для выполнения SQL-запросов, содержащих сценарии R. В этом кратком руководстве используется [Azure Data Studio](../../azure-data-studio/what-is.md).

## <a name="always-return-a-data-frame"></a>Всегда возвращайте кадр данных

Когда скрипт возвращает результаты из R в SQL Server, он должен возвращать данные в виде **data.frame**. Любой другой тип объекта, создаваемого в сценарии (список, коэффициент, вектор или двоичные данные) нужно преобразовать в кадр данных, если необходимо вывести его в результатах хранимой процедуры. К счастью существует несколько функций R, поддерживающих изменение других объектов на кадр данных. Вы даже можете сериализовать двоичную модель и возвратить ее в кадре данных. Мы сделаем это позже в этом кратком руководстве.

Давайте сначала поэкспериментируем с некоторыми базовыми объектами R — векторами, матрицами и списками, и посмотрим, как преобразование в кадр данных изменяет выходные данные, передаваемые в SQL Server.

Сравните эти два сценария "Hello World" на языке R. Сценарии выглядят почти одинаково, однако первый сценарий возвращает один столбец с тремя значениями, а второй — три столбца, каждый из которых содержит по одному значению.

**Пример 1**

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
     , @script = N' mytextvariable <- c("hello", " ", "world");
       OutputDataSet <- as.data.frame(mytextvariable);'
     , @input_data_1 = N' ';
```

**Пример 2**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
      , @input_data_1 = N'  ';
```

## <a name="identify-schema-and-data-types"></a>Определение схемы и типов данных

Почему результаты настолько отличаются?

Обычно ответ можно найти, использовав команду R `str()`. Добавьте функцию `str(object_name)` в скрипте R для получения сведений о схеме данных возвращаемого объекта R в виде информационного сообщения.

Чтобы понять, почему результаты примера 1 и 2 так сильно отличаются, вставьте строку `str(OutputDataSet)` в конце определения переменной `@script` в каждой инструкции следующим образом:

**Пример 1 с добавленной функцией str**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' mytextvariable <- c("hello", " ", "world");
      OutputDataSet <- as.data.frame(mytextvariable);
      str(OutputDataSet);'
      , @input_data_1 = N'  '
;
```

**Пример 2 с добавленной функцией str**

```sql
EXECUTE sp_execute_external_script
  @language = N'R', 
  @script = N' OutputDataSet <- data.frame(c("hello"), " ", c("world"));
    str(OutputDataSet);' , 
  @input_data_1 = N'  ';
```

Теперь просмотрите текст на вкладке **Сообщения**, в котором можно определить причину такой разницы.

**Результаты примера 1**

```sql
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Результаты примера 2**

```sql
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Как видно, небольшие изменения в синтаксисе R сильно повлияли на схему результатов. Мы не будем объяснять причину. Подробные сведения о различиях в типах данных R представлены в статье *Структуры данных* в [учебнике "Advanced R" Хэдли Викхема (Hadley Wickham)](http://adv-r.had.co.nz).

Сейчас помните, что необходимо проверить ожидаемые результаты при приведении объектов R в кадры данных.

> [!TIP]
> Также можно использовать функции идентификации R, такие как `is.matrix` и `is.vector`, чтобы получить сведения о внутренней структуре данных.

## <a name="implicit-conversion-of-data-objects"></a>Неявное преобразование объектов данных

Каждый объект данных R имеет собственные правила обработки значений в сочетании с другими объектами данных, если объекты данных имеют одинаковое количество измерений или если любой объект данных содержит разнородные типы данных.

Сначала создайте небольшую таблицу тестовых данных.

```sql
CREATE TABLE RTestData (col1 INT NOT NULL)

INSERT INTO RTestData
VALUES (1);

INSERT INTO RTestData
VALUES (10);

INSERT INTO RTestData
VALUES (100);
GO
```

Например, предположим, что вы выполняете следующую инструкцию для умножения матриц с помощью R. Вы умножаете матрицу с одним столбцом и тремя значения на массив с четырьмя значениями и в результате ожидаете получить матрицу 4x3.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:15);
    OutputDataSet <- as.data.frame(x %*% y);'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (([Col1] int, [Col2] int, [Col3] int, Col4 int));
```

Один столбец с тремя значениями преобразуется в матрицу с одним столбцом. Так как матрица представляет собой особый вид массива в R, массив `y` неявно приводится к матрице с одним столбцом, чтобы обеспечить соответствие двух аргументов.

**Результаты**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1500|

Однако обратите внимание, что происходит при изменении размера массива `y`.

```sql
execute sp_execute_external_script
   @language = N'R'
   , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:14);
   OutputDataSet <- as.data.frame(y %*% x);'
   , @input_data_1 = N' SELECT [Col1]  from RTestData;'
   WITH RESULT SETS (([Col1] int ));
```

На этот раз код R вернет одно значение в качестве результата.

**Результаты**

|Col1|
|---|
|1542|

Почему? Так как в этом случае два аргумента могут обрабатываться как векторы одинаковой длины, код R возвращает скалярное произведение в виде матрицы.  Это ожидаемое поведение в соответствии с правилами линейной алгебры. Тем не менее могут возникнуть проблемы, если нисходящее приложение ожидает, что выходная схема вывода не должна изменяться.

> [!TIP]
>
> Получили сообщения об ошибках? Убедитесь, что хранимая процедура выполняется в контексте базы данных, содержащей таблицу, а не в контексте базы данных **master** или другой базы данных.
>
> Кроме того, мы не рекомендуем использовать временные таблицы для этих примеров. Некоторые клиенты R будут завершать подключение между обработкой пакетов, удаляя временные таблицы.

## <a name="merge-or-multiply-columns-of-different-length"></a>Слияние или перемножение столбцов разной длины

R обеспечивает большую гибкость для работы с векторами различных размеров и для объединения структур в виде столбцов в кадры данных. Списки векторов могут выглядеть как таблица, но они не подчиняются всем правилам, управляющим таблицами базы данных.

Например, следующий скрипт определяет числовой массив, длина которого равна 6, и сохраняет его в переменной R `df1`. Затем числовой массив объединяется с целыми числами из таблицы RTestData, которая содержит 3 (три) значения, для формирования нового кадра данных — `df2`.

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
               df1 <- as.data.frame( array(1:6) );
               df2 <- as.data.frame( c( InputDataSet , df1 ));
               OutputDataSet <- df2'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (( [Col2] int not null, [Col3] int not null ));
```

Чтобы заполнить кадр данных, R повторно использует элементы, полученные из RTestData, столько раз, сколько требуется в соответствии с числом элементов в массиве `df1`.

**Результаты**

|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Помните, что кадр данных только выглядит как таблица и на самом деле является списком векторов.

## <a name="cast-or-convert-data"></a>Приведение или преобразование данных

R и SQL Server используют разные типы данных, поэтому при выполнении запроса в SQL Server для получения данных и их передаче в среду выполнения R обычно выполняется неявное преобразование. Кроме того, преобразования выполняются при возвращении данных из R в SQL Server.

- SQL Server передает данные из запроса в процесс R, которым управляет служба панели запуска, и преобразует их во внутреннее представление для повышения эффективности.
- Среда выполнения R загружает данные в переменную data.frame и выполняет собственные операции с данными.
- Ядро СУБД возвращает данные в SQL Server по защищенному внутреннему соединению и представляет данные в контексте типов данных SQL Server.
- Вы можете получить данные путем подключения к SQL Server с помощью клиентской или сетевой библиотеки, которая может выполнять запросы SQL и обрабатывать табличные наборы данных. Это клиентское приложение может и по-другому влиять на данные.

Чтобы увидеть, как это работает, выполните подобный запрос в хранилище данных [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). Это представление возвращает данные продаж, используемые для создания прогнозов.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = 'M200 Europe'
           ORDER BY ReportingDate ASC
```

> [!NOTE]
>
> Можно использовать любую версию AdventureWorks или создать другой запрос, используя собственную базу данных. Задача заключается в том, чтобы обработать данные, содержащие текст, дату и время, а также числовые значения.

Теперь попробуйте выполнить этот запрос в качестве входных данных для хранимой процедуры.

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
      , @script = N' str(InputDataSet);
      OutputDataSet <- InputDataSet;'
      , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Если появляется сообщение об ошибке, возможно, необходимо внести некоторые изменения в текст запроса. Например, строковый предикат в предложении WHERE нужно заключать в два набора одинарных кавычек.

После того, как запрос начнет действовать, просмотрите результаты выполнения функции `str`, чтобы увидеть, как R обрабатывает входные данные.

**Результаты**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- Столбец datetime был обработан с использованием типа данных R **POSIXct**.
- Текстовый столбец "ProductSeries" определен в качестве **коэффициента**, то есть категориальной переменной. Строковые значения обрабатываются как коэффициенты по умолчанию. Если передать строку в R, она преобразуется в целое число для внутреннего использования и сопоставляется со строками на выходе.

### <a name="summary"></a>Сводка

Даже в этих коротких примерах можно увидеть, как следует проверять последствия преобразования данных при передаче запросов SQL в качестве входных данных. Так как некоторые типы данных SQL Server не поддерживаются в R, следуйте приведенным ниже рекомендациям, чтобы избежать ошибок:

- Проверяйте данные заранее, а также проверяйте столбцы и значения в схеме, которые могут вызвать проблему при передаче в код R.
- Указывайте столбцы из источника входных данных по отдельности. Не используйте `SELECT *`. Определите способ обработки каждого столбца.
- Во избежание непредвиденных ситуаций во время подготовки входных данных при необходимости выполняйте явные приведения.
- Избегайте передачи столбцов данных (например, идентификаторов GUID или идентификаторов GUID строк). Они вызывают ошибки и не требуются для моделирования.

Дополнительные сведения о поддерживаемых и неподдерживаемых типах данных см. в статье [Библиотеки и типы данных R](../r/r-libraries-and-data-types.md).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о написании расширенных функций R с использованием машинного обучения SQL см. в этом кратком руководстве:

> [!div class="nextstepaction"]
> [Написание расширенных функций R с использованием машинного обучения SQL](quickstart-r-functions.md)

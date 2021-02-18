---
title: Учебник по Python. Изучение и визуализация данных
titleSuffix: SQL machine learning
description: Изучите образцы данных и создайте несколько графиков, чтобы подготовиться к использованию двоичной классификации в Python с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: e36b8f6f2768d4de83c9f23bd264329f6f04d381
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339680"
---
# <a name="python-tutorial-explore-and-visualize-data"></a>Учебник по Python. Изучение и визуализация данных
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Во второй части этой серии руководств вы изучите образец данных и создадите несколько графиков. Далее вы узнаете, как сериализовать графические объекты на Python, а затем десериализовать эти объекты и создать графики.

Работая с этой статьей, вы узнаете о следующем.

> [!div class="checklist"]
> + Изучение образца данных
> + Создание диаграмм с помощью языка Python в T-SQL

В [первой части](python-taxi-classification-introduction.md) были установлены необходимые компоненты и восстановлена демонстрационная база данных.

В [третьей части](python-taxi-classification-create-features.md) вы узнаете, как создавать функции из необработанных данных с помощью функции Transact-SQL. Затем вы вызовите эту функцию из хранимой процедуры, чтобы создать таблицу, содержащую значения характеристик.

В [четвертой части](python-taxi-classification-train-model.md) вы научитесь загружать модули и вызывать необходимые функции для создания и обучения модели с помощью хранимой процедуры SQL Server.

Из [пятой части](python-taxi-classification-deploy-model.md) вы узнаете, как ввести в эксплуатацию модели, которые были обучены и сохранены в соответствии с инструкциями в четвертой части.

## <a name="review-the-data"></a>Изучение данных

Во-первых, уделите несколько минут для просмотра схемы данных, так как мы внесли некоторые изменения, чтобы упростить использование данных в базе данных работы такси в Нью-Йорке

+ В исходном наборе данных для идентификаторов такси и записей о поездках использовались отдельные файлы. Мы присоединили два исходных набора данных к столбцам _medallion_, _hack_license_ и _pickup_datetime_.  
+ Исходный набор данных имел много файлов и был довольно большим. Мы сократили выборку, чтобы получить 1% от первоначального количества записей. Текущая таблица данных содержит 1 703 957 строк и 23 столбца.

**Идентификаторы такси**

В столбце _medallion_ представлены уникальные идентификационные номера такси.

В столбце _hack_license_ содержатся номера лицензий таксистов (без указания имен).

**Записи о поездках и оплате**

Каждая запись о поездке включает сведения о местах посадки и высадки, а также о расстоянии поездки.

Каждая запись об оплате включает такие сведения, как тип оплаты, общий размер платежа и размер чаевых.

Последние три столбца можно использовать для различных задач машинного обучения.  Столбец _tip_amount_ содержит непрерывный ряд числовых значений и может использоваться в качестве столбца **меток** для регрессионного анализа. Столбец _tipped_ содержит значения "Да" или "Нет" и используется для двоичной классификации. Столбец _tip_class_ содержит несколько **меток классов** и поэтому может использоваться в качестве метки для задач многоклассовой классификации.

Все значения, используемые для столбцов меток, основаны на столбце `tip_amount` с применением следующих бизнес-правил:

+ Столбец меток `tipped` может принимать значения 0 или 1

    ЕСЛИ `tip_amount` > 0, ТО `tipped` = 1; ИНАЧЕ `tipped` = 0

+ Столбец меток `tip_class` может принимать значения от 0 до 4

    Класс 0: `tip_amount` = $0

    Класс 1: `tip_amount` > $0 И `tip_amount` < = $5
    
    Класс 2: `tip_amount` > $5 И `tip_amount` < = $10
    
    Класс 3: `tip_amount` > $10 И `tip_amount` < = $20
    
    Класс 4: `tip_amount` > $20

## <a name="create-plots-using-python-in-t-sql"></a>Создание диаграмм с помощью языка Python в T-SQL

Разработка решения для обработки и анализа данных обычно связана с большим числом операций по анализу и визуализации данных. Так как визуализация является эффективным средством для представления распределения данных и выбросов, в Python имеется много пакетов для визуализации данных. Модуль **matplotlib** является одной из наиболее популярных библиотек для визуализации и включает множество функций для создания гистограмм, точечных диаграмм, блочных диаграмм и других графических средств просмотра данных.

В этом разделе вы научитесь работать с графиками с использованием хранимых процедур. Вместо того чтобы открывать изображение на сервере, вы сохраняете объект Python `plot` как данные типа **varbinary**, а затем записываете их в файл, которым можно поделиться и просматривать в любом другом месте.

### <a name="create-a-plot-as-varbinary-data"></a>Создание графика в виде данных типа varbinary

Хранимая процедура возвращает сериализованный объект Python `figure` в виде потока данных **varbinary**. Двоичные данные нельзя просматривать напрямую, но можно использовать код Python на клиенте для десериализации и просмотра рисунков, а затем сохранения в файл изображения на клиентском компьютере.

1. Создайте хранимую процедуру **PyPlotMatplotlib**, если сценарий PowerShell еще не сделал этого.

    - Переменная `@query` определяет текст запроса `SELECT tipped FROM nyctaxi_sample`, который передается в блок кода Python в качестве аргумента входной переменной `@input_data_1`.
    - Сценарий Python довольно прост: объекты **matplotlib** `figure` используются для создания гистограммы и точечной диаграммы, и затем эти объекты сериализуются с помощью библиотеки `pickle`.
    - Графический объект Python сериализуется в тип данных DataFrame библиотеки **pandas** для вывода.
  
    ```sql
    DROP PROCEDURE IF EXISTS PyPlotMatplotlib;
    GO

    CREATE PROCEDURE [dbo].[PyPlotMatplotlib]
    AS
    BEGIN
        SET NOCOUNT ON;
        DECLARE @query nvarchar(max) =
        N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'
        EXECUTE sp_execute_external_script
        @language = N'Python',
        @script = N'
    import matplotlib
    matplotlib.use("Agg")
    import matplotlib.pyplot as plt
    import pandas as pd
    import pickle

    fig_handle = plt.figure()
    plt.hist(InputDataSet.tipped)
    plt.xlabel("Tipped")
    plt.ylabel("Counts")
    plt.title("Histogram, Tipped")
    plot0 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.tip_amount)
    plt.xlabel("Tip amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Tip amount")
    plot1 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.fare_amount)
    plt.xlabel("Fare amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Fare amount")
    plot2 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.scatter( InputDataSet.fare_amount, InputDataSet.tip_amount)
    plt.xlabel("Fare Amount ($)")
    plt.ylabel("Tip Amount ($)")
    plt.title("Tip amount by Fare amount")
    plot3 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    OutputDataSet = plot0.append(plot1, ignore_index=True).append(plot2, ignore_index=True).append(plot3, ignore_index=True)
    ',
    @input_data_1 = @query
    WITH RESULT SETS ((plot varbinary(max)))
    END
    GO
    ```

2. Теперь запустите хранимую процедуру без аргументов, чтобы создать график на основе данных, жестко запрограммированных в качестве входного запроса.

    ```sql
    EXEC [dbo].[PyPlotMatplotlib]
    ```

3. Результаты должны выглядеть примерно так:
  
    ```sql
    plot
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    ```

4. Из [клиента Python](../python/setup-python-client-tools-sql.md) теперь можно подключаться к экземпляру SQL Server, который создал двоичные объекты графиков, и просматривать их. 

    Для этого выполните следующий код Python, заменив имя сервера, имя базы данных и учетные данные соответствующим образом (для проверки подлинности Windows замените параметры `UID` и `PWD` на `Trusted_Connection=True`). Убедитесь, что версия Python одинакова на клиенте и на сервере. Также убедитесь, что библиотеки Python на вашем клиенте (например, matplotlib) имеют ту же или более позднюю версию по сравнению с версиями библиотек, установленных на сервере.
  
    ```python
    %matplotlib notebook
    import pyodbc
    import pickle
    import os
    cnxn = pyodbc.connect('DRIVER=SQL Server;SERVER={SERVER_NAME};DATABASE={DB_NAME};UID={USER_NAME};PWD={PASSWORD}')
    cursor = cnxn.cursor()
    cursor.execute("EXECUTE [dbo].[PyPlotMatplotlib]")
    tables = cursor.fetchall()
    for i in range(0, len(tables)):
        fig = pickle.loads(tables[i][0])
        fig.savefig(str(i)+'.png')
    print("The plots are saved in directory: ",os.getcwd())
    ```

5. Если подключение установлено успешно, должно отобразиться сообщение следующего вида:
  
   *Графики сохранены в каталоге: XXXX*
  
6. Выходной файл создается в рабочем каталоге Python. Чтобы просмотреть график, перейдите в рабочий каталог Python и откройте файл. На следующем рисунке показан график, сохраненный на клиентском компьютере.
  
   ![Зависимость суммы чаевых от суммы чека](media/sqldev-python-sample-plot.png "Зависимость суммы чаевых от суммы чека") 

## <a name="next-steps"></a>Дальнейшие шаги

Работая с этой статьей, вы выполните следующие задачи:

> [!div class="checklist"]
> + Изучен образец данных
> + Созданы графики с помощью языка Python в T-SQL

> [!div class="nextstepaction"]
> [Учебник по Python. Создание характеристик данных с помощью T-SQL](python-taxi-classification-create-features.md)
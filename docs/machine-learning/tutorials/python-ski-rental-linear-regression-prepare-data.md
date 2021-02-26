---
title: Учебник по Python. Подготовка данных
titleSuffix: SQL machine learning
description: Во второй части этого цикла учебников, состоящего из четырех частей, с помощью Python вы подготовите данные, чтобы спрогнозировать число прокатов лыж с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1cce65d71e686876370b6a978d660d0f75a1d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351469"
---
# <a name="python-tutorial-prepare-data-to-train-a-linear-regression-model-with-sql-machine-learning"></a>Учебник по Python. Подготовка данных для обучения модели линейной регрессии с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных с помощью Python. Далее в этом цикле вы будете использовать подготовленные данные для обучения и развертывания модели линейной регрессии в Python с помощью Служб машинного обучения SQL Server или в Кластерах больших данных.
::: moniker-end
::: moniker range="=sql-server-2017"
Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных с помощью Python. Далее в этой серии руководств вы будете использовать эти данные для обучения и развертывания модели линейной регрессии с использованием Python в Службах машинного обучения SQL Server.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных с помощью Python. Далее в этой серии руководств вы будете использовать эти данные для обучения и развертывания модели линейной регрессии с использованием Python в Службах машинного обучения Управляемого экземпляра SQL Azure.
::: moniker-end

В этой статье вы узнаете, как выполнять следующие задачи.

> [!div class="checklist"]
> * Загрузка данных из базы данных в кадр данных **pandas**.
> * Подготовка данных в Python путем удаления некоторых столбцов

В [первой части](python-ski-rental-linear-regression.md) вы узнали, как восстановить учебную базу данных.

В [третьей части](python-ski-rental-linear-regression-train-model.md) вы узнаете, как обучить модель машинного обучения линейной регрессии в Python.

В [четвертой части](python-ski-rental-linear-regression-deploy-model.md) вы узнаете, как сохранить модель в базе данных, а затем создать хранимые процедуры на основе сценариев Python, разработанных во второй и третьей частях. Хранимые процедуры будут запускаться на сервере, чтобы формировать прогнозы на основе новых данных.

## <a name="prerequisites"></a>Предварительные требования

* Во второй части этого учебника предполагается, что вы уже выполнили [первую часть](python-ski-rental-linear-regression.md) и описанные в ней предварительные требования.

## <a name="explore-and-prepare-the-data"></a>Анализ и подготовка данных

Чтобы использовать данные в Python, необходимо загрузить их из базы данных в кадр данных pandas.

Создайте записную книжку Python в Azure Data Studio и выполните приведенный ниже сценарий. 

Приведенный ниже сценарий Python импортирует набор данных из таблицы **dbo.rental_data** вашей базы данных в кадр данных pandas **df**.

В строке подключения при необходимости замените параметры подключения.

```python
import pyodbc
import pandas
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error

# Connection string to your SQL Server instance
conn_str = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server}; SERVER=<server>; DATABASE=TutorialDB;UID=<username>;PWD=<password>)

query_str = 'SELECT Year, Month, Day, Rentalcount, Weekday, Holiday, Snow FROM dbo.rental_data'

df = pandas.read_sql(sql=query_str, con=conn_str)

print("Data frame:", df)

# Get all the columns from the dataframe.
columns = df.columns.tolist()

# Filter the columns to remove ones we don't want to use in the training
columns = [c for c in columns if c not in ["Year"]]
```

Результат должен иметь следующий вид.

```results
Data frame:      Year  Month  Day  RentalCount  WeekDay  Holiday  Snow
0    2014      1   20          445        2        1     0
1    2014      2   13           40        5        0     0
2    2013      3   10          456        1        0     0
3    2014      3   31           38        2        0     0
4    2014      4   24           23        5        0     0
..    ...    ...  ...          ...      ...      ...   ...
448  2013      2   19           57        3        0     1
449  2015      3   18           26        4        0     0
450  2015      3   24           29        3        0     1
451  2014      3   26           50        4        0     1
452  2015     12    6          377        1        0     1

[453 rows x 7 columns]
```

## <a name="next-steps"></a>Дальнейшие действия

Во второй части этого учебника вы выполнили следующие действия.

* Загрузка данных из базы данных в кадр данных **pandas**.
* Подготовка данных в Python путем удаления некоторых столбцов

Чтобы обучить модель машинного обучения, которая использует данные из базы данных TutorialDB, перейдите к третьей части этого учебника:

> [!div class="nextstepaction"]
> [Учебник по Python. Обучение модели линейной регрессии](python-ski-rental-linear-regression-train-model.md)
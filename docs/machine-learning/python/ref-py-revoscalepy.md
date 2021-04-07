---
title: Пакет Python revoscalepy
description: revoscalepy — это пакет Python от корпорации Майкрософт, который поддерживает распределенные вычисления, удаленные контексты вычислений и высокопроизводительные алгоритмы обработки и анализа данных.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: 15f526feb973387c0f1bcd5bc4e6de0019a5774e
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105980939"
---
# <a name="revoscalepy-python-package-in-sql-server-machine-learning-services"></a>revoscalepy (пакет Python в Службах машинного обучения SQL Server)
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

**revoscalepy** — это пакет Python от корпорации Майкрософт, который поддерживает распределенные вычисления, удаленные контексты вычислений и высокопроизводительные алгоритмы обработки и анализа данных. Этот пакет входит в состав [Служб машинного обучения SQL Server](../sql-server-machine-learning-services.md).

Пакет предоставляет следующие функциональные возможности.

+ локальные и удаленные контексты вычислений в системах с одной версией **revoscalepy**;
+ функции преобразования и визуализации данных;
+ функции обработки и анализа данных, масштабируемые с помощью распределенной или параллельной обработки;
+ улучшенная производительность, включая использование библиотек Intel Math.

Источники данных и контексты вычислений, создаваемые в **revoscalepy**, можно также использовать в алгоритмах машинного обучения. Общие сведения об этих алгоритмах см. в статье [microsoftml Python module in SQL Server](ref-py-microsoftml.md) (Модуль Python microsoftml в SQL Server).

## <a name="full-reference-documentation"></a>Полная справочная документация

Пакет **revoscalepy** распространяется в нескольких продуктах Майкрософт, но его использование не зависит от того, получили ли вы его в SQL Server или в другом продукте. Благодаря сходству функций [документация по отдельным функциям revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) опубликована только в одном разделе в [справочнике по Python](/machine-learning-server/python-reference/introducing-python-package-reference) для Microsoft Machine Learning Server. Если для конкретных продуктов функции будут действовать иначе, выявленные расхождения будут приведены на странице справки по функциям.

## <a name="versions-and-platforms"></a>Версии и платформы

Модуль **revoscalepy** основан на Python 3.5 и доступен только при установке одного из следующих продуктов или скачиваемых файлов Майкрософт:

+ [Службы машинного обучения SQL Server](../install/sql-machine-learning-services-windows-install.md)
+ [Microsoft Machine Learning Server 9.2.0 или более поздней версии](/machine-learning-server/)
+ [Клиентские библиотеки Python для клиента обработки и анализа данных](setup-python-client-tools-sql.md)

> [!NOTE]
> В SQL Server 2017 полные версии выпусков продуктов доступны только для Windows. В [SQL Server 2019](../../linux/sql-server-linux-setup-machine-learning.md) и более поздних версий пакет **revoscalepy** поддерживает Windows и Linux.

## <a name="functions-by-category"></a>Функции по категориям

Чтобы можно было понять, как использовать каждую функцию, в этом разделе приводится описание функций по категориям. Для поиска функций в алфавитном порядке можно воспользоваться [оглавлением](/machine-learning-server/python-reference/introducing-python-package-reference).

## <a name="1-data-source-and-compute"></a>1\. Источники данных и вычисления

**revoscalepy** содержит функции для создания источников данных и настройки расположения или *контекста вычисления*, в котором выполняются вычисления. Функции, относящиеся к сценариям SQL Server, приведены в таблице ниже.

В некоторых случаях SQL Server и Python используют разные типы данных. Список сопоставлений между типами данных SQL и Python см. в разделе о [типах данных Python и SQL](python-libraries-and-data-types.md).

| Функция| Описание|
| ------- | ---------- |
| [RxInSqlServer](/machine-learning-server/python-reference/revoscalepy/rxinsqlserver) |  Создание объекта контекста вычисления SQL Server для отправки вычислений в удаленный экземпляр. Несколько функций **revoscalepy** принимают контекст вычисления в качестве аргумента. Пример переключения контекста см. в статье о [создание модели с помощью пакета revoscalepy](../tutorials/use-python-revoscalepy-to-create-model.md).|
| [RxSqlServerData](/machine-learning-server/python-reference/revoscalepy/rxsqlserverdata) | Создание объекта данных на основе запроса или таблицы SQL Server. |
| [RxOdbcData](/machine-learning-server/python-reference/revoscalepy/rxodbcdata)| Создание источника данных на основе соединения ODBC. |
| [RxXdfData](/machine-learning-server/python-reference/revoscalepy/rxxdfdata) | Создание источника данных на основе локального XDF-файла. XDF-файлы часто используются для разгрузки данных в памяти на диск. XDF-файл может быть полезен при работе с объемом данных, превышающим объем передаваемых данных из базы данных в одном пакете, или же превышающим допустимый объем данных в памяти. Например, если вам приходится часто перемещать большие объемы данных из базы данных на локальную рабочую станцию, то вместо того, чтобы выполнять запросы к базе данных для каждой операции R, вы можете использовать XDF-файл как кэш, чтобы сохранить данные локально, а затем работать с ними в рабочей области R. |

> [!TIP]
> Если вы не знакомы с концепциями источников данных или контекстов вычислений, рекомендуется начать с изучения [распределенных вычислений](/machine-learning-server/r/how-to-revoscaler-distributed-computing) в документации по Microsoft Machine Learning Server.

## <a name="2-data-manipulation-etl"></a>2\. Работа с данными (ETL)

| Функция | Описание |
|----------|-------------|
|[rx_import](/machine-learning-server/python-reference/revoscalepy/rx-import) | Импорт данных в XDF-файл или кадр данных.|
|[rx_data_step](/machine-learning-server/python-reference/revoscalepy/rx-data-step) | Преобразование данных из набора входных данных в набор выходных данных.|

<a name="bkmk_algorithms"></a>

## <a name="3-training-and-summarization"></a>3\. Обучение и формирование сводных данных

| Функция| Описание|
| ------- | ---------- |
|[rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) | Подбор деревьев принятия решений со стохастическим градиентным усилением|
|[rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) | Подбор лесов принятия решений по классификации и регрессии|
|[rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) | Подбор деревьев классификации и регрессии |
|[rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod) | создание модели линейной регрессии;|
|[rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) | создание модели логистической регрессии;|
|[rx_summary](/machine-learning-server/python-reference/revoscalepy/rx-summary) | Создание однозначных сводок по объектам в revoscalepy.|

Кроме того, для реализации дополнительных подходов следует изучить функции в [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package).

<a name="ml-scoring"></a>

## <a name="4-scoring-functions"></a>4\. Функции оценки

| Компонент| Описание|
| ------- | ---------- |
| [rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) | Создает прогнозы на основе обученной модели и может использоваться для оценки в реальном времени. |
|[rx_predict_default](/machine-learning-server/python-reference/revoscalepy/rx-predict-default) | Вычисление прогнозируемых значений и остатков с помощью объектов rx_lin_mod и rx_logit. |
|[rx_predict_rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-predict-rx-dforest) | Вычисление прогнозируемых или подогнанных значений для набора данных на основе объектов rx_dforest или rx_btrees. |
|[rx_predict_rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-predict-rx-dtree) | Вычисление прогнозируемых или подогнанных значений для набора данных на основе объекта rx_dtree. |

## <a name="how-to-work-with-revoscalepy"></a>Работа с revoscalepy

Функции в **revoscalepy** вызываются в коде Python, инкапсулированном в хранимые процедуры. Большинство разработчиков создают решения **revoscalepy** локально, а затем переносят готовый код Python в хранимые процедуры, отрабатывая, таким образом, процедуру развертывания.

При локальном выполнении скрипт Python обычно запускается из командной строки или из среды разработки Python с указанием контекста вычисления SQL Server с помощью одной из функций **revoscalepy**. Удаленный контекст вычисления можно использовать для всего кода или для отдельных функций. Например, может потребоваться разгрузить обучение модели на сервер, чтобы работать с актуальными данными и исключить перемещение данных.

Когда вы будете готовы инкапсулировать скрипт Python в хранимую процедуру [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), рекомендуется переписать код и оформить его в виде функции, которая содержит четко определенные входные и выходные данные. 

Входные и выходные данные должны быть кадрами данных **Pandas**. После этого вы сможете вызывать хранимую процедуру из любого клиента, который поддерживает T-SQL, без труда передавать запросы SQL в качестве входных данных и сохранять результаты в таблицах SQL. Пример см. в статье об [анализе данных с помощью Python для разработчиков SQL](../tutorials/python-taxi-classification-introduction.md).

### <a name="using-revoscalepy-with-microsoftml"></a>Использование revoscalepy с microsoftml

Функции Python для [microsoftml](ref-py-microsoftml.md) интегрированы с контекстами вычислений и источниками данных, предоставленными в revoscalepy. При вызове функций из microsoftml, например при определении и обучении модели, функции revoscalepy можно использовать для выполнения кода Python в локальной среде или в удаленном контексте вычисления SQL Server.

В следующем примере показан синтаксис импорта модулей в коде Python. Затем можно сослаться на нужные вам функции.

```python
from microsoftml.modules.logistic_regression.rx_logistic_regression import rx_logistic_regression
from revoscalepy.functions.RxSummary import rx_summary
from revoscalepy.etl.RxImport import rx_import_datasource
```

## <a name="see-also"></a>См. также раздел

+ [Учебники по Python](../tutorials/python-tutorials.md)
+ [Справочник по Python (Microsoft Machine Learning Server)](/machine-learning-server/python-reference/introducing-python-package-reference)
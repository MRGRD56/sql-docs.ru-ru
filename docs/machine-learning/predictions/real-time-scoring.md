---
title: Оценка в реальном времени с помощью sp_rxPredict
description: Сведения о получении оценки в реальном времени с помощью системной хранимой процедуры sp_rxPredict в SQL Server для создания высокопроизводительных прогнозов или оценки в рабочих нагрузках прогнозирования.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/05/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 7761957f1627d7fbf2e881c99f83ab38b8a808ac
ms.sourcegitcommit: ab0c654d924eeb5647e47444abb59d934345b205
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450195"
---
# <a name="real-time-scoring-with-sp_rxpredict-in-sql-server"></a>Оценка в реальном времени с помощью процедуры sp_rxPredict в SQL Server
[!INCLUDE[sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Сведения о получении оценки в реальном времени с помощью системной хранимой процедуры [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md) в SQL Server для создания высокопроизводительных прогнозов или оценки в рабочих нагрузках прогнозирования.

Оценка в реальном времени с помощью `sp_rxPredict` не зависит от языка и выполняется независимо от среды выполнения R или Python в [Службах машинного обучения](../sql-server-machine-learning-services.md) или [Machine Learning Server](../r/r-server-standalone.md). Работая с моделью, созданной и обученной с помощью функций Майкрософт, а затем сериализованной в двоичный формат в SQL Server, вы можете с помощью оценки в реальном времени создавать прогнозируемые результаты на основе новых входных данных в экземплярах SQL Server, в которых не установлена надстройка R или Python.

## <a name="how-real-time-scoring-works"></a>Принципы работы оценки в реальном времени

Оценка в реальном времени поддерживается для определенных типов моделей на основе функций в [RevoScaleR](../r/ref-r-revoscaler.md) и [MicrosoftML](../r/ref-r-microsoftml.md) в R, или [revoscalepy](../python/ref-py-revoscalepy.md) и [microsoftml](../python/ref-py-microsoftml.md) в Python. При этом используются собственные библиотеки C++ для создания оценок на основе данных, которые пользователь ввел в модель машинного обучения (в специальном двоичном формате).

Так как обученную модель можно использовать для оценки без необходимости вызывать внешнюю языковую среду выполнения в [Службах машинного обучения](../sql-server-machine-learning-services.md) или [Machine Learning Server](../r/r-server-standalone.md), затраты на процессы сокращаются.

Оценка в реальном времени — это многоэтапный процесс.

1. Хранимую процедуру, с помощью которой выполняется оценка, необходимо включить для каждой базы данных.
2. Предварительно обученная модель загружается в двоичном формате.
3. В модель вводятся входные данные для оценки. Это могут быть таблицы или отдельные строки.
4. Для формирования оценок вызывается хранимая процедура [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md).

## <a name="prerequisites"></a>Предварительные требования

+ [Включите интеграцию со средой CLR SQL Server](../../relational-databases/clr-integration/clr-integration-enabling.md).

+ [Включите оценку в реальном времени](#bkmk_enableRtScoring).

+ Модель должна быть заранее обучена с помощью одного из поддерживаемых алгоритмов **rx**. Дополнительные сведения см. в разделе [Поддерживаемые алгоритмы](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md?view=sql-server-ver15#supported-algorithms) для `sp_rxPredict`.

+ Сериализуйте модель с помощью [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) для R или [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) для Python. Эти функции сериализации оптимизированы для поддержки быстрой оценки.

+ Сохраните модель в экземпляре ядра СУБД, из которого она будет вызываться. В этом экземпляр не обязательно должно быть установлено расширение среды выполнения R или Python.

> [!Note]
> Оценка в реальном времени в настоящее время оптимизирована для быстрого прогнозирования на основе небольших наборов данных: от нескольких строк до сотен тысяч строк. Для больших наборов данных функция [rxPredict](/machine-learning-server/r-reference/revoscaler/rxpredict) может работать быстрее.

<a name ="bkmk_enableRtScoring"></a>

## <a name="enable-real-time-scoring"></a>Включение оценки в реальном времени

Включите эту функцию для каждой базы данных, которая будет использоваться для оценки. Администратор сервера должен запустить программу командной строки RegisterRExt.exe, которая входит в состав пакета RevoScaleR.

> [!CAUTION]
> Чтобы оценка в реальном времени работала, в экземпляре необходимо включить возможности среды CLR SQL. Кроме того, базу данных необходимо пометить как заслуживающую доверия. При запуске скрипта эти действия выполняются автоматически. Но перед этим обдумайте возможные последствия для безопасности.

1. Откройте командную строку с повышенными привилегиями и перейдите в папку, где находится программа RegisterRExt.exe. По умолчанию путь к ней следующий:

    `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\`

2. Выполните следующую команду, подставив имя своего экземпляра и целевую базу данных, в которой необходимо включить расширенные хранимые процедуры:

    `RegisterRExt.exe /installRts [/instance:name] /database:databasename`

    Например, чтобы включить расширенную хранимую процедуру в базе данных CLRPredict в экземпляре по умолчанию, введите следующую команду:

    `RegisterRExt.exe /installRts /database:CLRPRedict`

    Имя экземпляра является необязательным, если база данных находится в экземпляре по умолчанию. Если вы используете именованный экземпляр, укажите его имя.

3. Программа RegisterRExt.exe создает следующие объекты:

    + доверенные сборки;
    + хранимую процедуру `sp_rxPredict`;
    + новую роль базы данных `rxpredict_users`. Администратор базы данных может использовать эту роль для предоставления разрешения пользователям, которые используют функцию оценки в реальном времени.

4. Добавьте всех пользователей, которым требуется выполнять `sp_rxPredict`, в новую роль.

> [!NOTE]
>
> В SQL Server 2017 и более поздних версиях для предотвращения проблем с интеграцией со средой CLR применяются дополнительные меры безопасности. Они накладывают дополнительные ограничения на использование этой хранимой процедуры.

## <a name="disable-real-time-scoring"></a>Отключение оценки в реальном времени

Чтобы отключить функцию оценки в реальном времени, откройте командную строку с повышенными привилегиями и выполните следующую команду: `RegisterRExt.exe /uninstallrts /database:<database_name> [/instance:name]`

## <a name="example"></a>Пример

В этом примере описаны действия по подготовке и сохранению модели для прогнозирования **в реальном времени**, а также приведен пример вызова функции из T-SQL на языке R.

### <a name="step-1-prepare-and-save-the-model"></a>Шаг 1. Подготовка и сохранение модели

Двоичный формат, необходимый для sp\_rxPredict, аналогичен формату, требуемому для использования функции PREDICT. Поэтому в коде на языке R включите вызов [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) и укажите параметр `realtimeScoringOnly = TRUE`, как в следующем примере:

```R
model <- rxSerializeModel(model.name, realtimeScoringOnly = TRUE)
```

### <a name="step-2-call-sp_rxpredict"></a>Шаг 2. Вызов sp_rxPredict

Вызов `sp_rxPredict` осуществляется так же, как и любая другая хранимая процедура. В текущем выпуске хранимая процедура принимает только два параметра: _\@model_ для модели в двоичном формате и _\@inputData_ для данных, используемых при оценке, которые определены в виде допустимого SQL-запроса.

Так как двоичный формат аналогичен тому, который использует функция PREDICT, можно применить модели и таблицу данных из предыдущего примера.

```sql
DECLARE @irismodel varbinary(max)
SELECT @irismodel = [native_model_object] from [ml_models]
WHERE model_name = 'iris.dtree' 
AND model_version = 'v1''

EXEC sp_rxPredict
@model = @irismodel,
@inputData = N'SELECT * FROM iris_rx_data'
```

> [!NOTE]
> Вызов `sp_rxPredict` завершается ошибкой, если входные данные для оценки не включают в себя столбцы, соответствующие требованиям модели. В настоящее время поддерживаются только следующие типы данных .NET: double, float, short, ushort, long, ulong и string.
>
> Поэтому может потребоваться отфильтровать неподдерживаемые типы во входных данных, прежде чем использовать их для оценки в реальном времени.
>
> Сведения о соответствующих типах SQL см. в статье [Сопоставление типов SQL и CLR](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping) или [Сопоставление данных о параметрах CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

## <a name="next-steps"></a>Дальнейшие шаги

+ [Собственная оценка с использованием функции PREDICT T-SQL с помощью машинного обучения SQL](native-scoring-predict-transact-sql.md)
+ [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md)
+ [Машинное обучение SQL](../index.yml)
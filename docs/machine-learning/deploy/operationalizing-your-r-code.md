---
title: Развертывание кода на языке R в хранимых процедурах
description: Внедрите код R в хранимую процедуру SQL Server, чтобы сделать его доступным для любого клиентского приложения, имеющего доступ к базе данных SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 2bcb3b9eb38d0fa9b41563c3af5b5f2e4dad7c5f
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003991"
---
# <a name="operationalize-r-code-using-stored-procedures-in-sql-server-machine-learning-services"></a>Использование кода R с хранимыми процедурами в Службах машинного обучения SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

При использовании функций R и Python в Службах машинного обучения SQL Server наиболее распространенный подход к переносу решений в рабочую среду — внедрение кода в хранимые процедуры. В этой статье перечислены ключевые моменты, которые необходимо учитывать разработчику на SQL при вводе кода R в эксплуатацию с помощью SQL Server.

## <a name="deploy-production-ready-script-using-t-sql-and-stored-procedures"></a>Развертывание готового к использованию скрипта с помощью T-SQL и хранимых процедур

Интеграция решений для обработки и анализа данных обычно предполагает значительную переработку кода для обеспечения производительности и интеграции. Службы машинного обучения SQL Server упрощают эту задачу, так как код R и Python можно выполнять в SQL Server и вызывать с помощью хранимых процедур. Дополнительные сведения о механизме внедрения кода в хранимые процедуры см. в следующих статьях:

+ [Создание и выполнение простых скриптов R в SQL Server](../tutorials/quickstart-r-create-script.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

Более полный пример развертывания кода R в рабочей среде с помощью хранимых процедур можно найти в [руководстве по R: Прогнозирование стоимости поездки в нью-йоркском такси с использованием двоичной классификации](../tutorials/r-taxi-classification-introduction.md).

## <a name="guidelines-for-optimizing-r-code-for-sql"></a>Рекомендации по оптимизации кода R для SQL

Преобразовывать код R в SQL проще, если предварительно произвести некоторые оптимизации. К ним относятся исключение типов данных, вызывающих проблемы, предотвращение ненужных преобразований данных и переработка кода R в один вызов функции, который можно легко параметризировать. Дополнительные сведения см. в разделе:

+ [Библиотеки и типы данных R](../r/r-libraries-and-data-types.md)
+ [Использование вспомогательных функций sqlrutils](../r/ref-r-sqlrutils.md)

## <a name="integrate-r-and-python-with-applications"></a>Интеграция кода R и Python с приложениями

Так как код R или Python можно запускать из хранимой процедуры, скрипты можно выполнять из любого приложения, которое может отправить инструкцию T-SQL и обработать результаты. Например, можно переобучать модель по расписанию с помощью задачи [Выполнение скрипта T-SQL](../../integration-services/control-flow/execute-t-sql-statement-task.md) в Integration Services или с помощью другого планировщика заданий, который может выполнять хранимую процедуру.

Оценка — одна из важных задач, которую можно легко автоматизировать или запускать из внешних приложений. Модель предварительно обучается с помощью кода R или Python либо хранимой процедуры, а затем [сохраняется в двоичном формате](../tutorials/walkthrough-build-and-save-the-model.md) в таблице. После этого модель можно загрузить в переменную в процессе вызова хранимой процедуры и использовать один из следующих способов оценки в T-SQL:

+ оценка в реальном времени, оптимизированная для небольших пакетов;
+ оценка одной строки для вызова из приложения;
+ [собственная оценка](../predictions/native-scoring-predict-transact-sql.md) для быстрого пакетного прогнозирования из SQL Server без вызова R.

В этом учебнике приведены примеры оценки с помощью хранимой процедуры как в пакетном, так и в построчном режиме:

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [Полное пошаговое руководство по обработке и анализу данных с помощью R в SQL Server](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ [Учебник по R. Прогнозирование стоимости поездки в нью-йоркском такси с использованием двоичной классификации](../tutorials/r-taxi-classification-introduction.md)
::: moniker-end

## <a name="boost-performance-and-scale"></a>Повышение производительности и масштабируемости

Хотя язык R с открытым кодом имеет ограничения, связанные с большими наборами данных, [API пакета RevoScaleR](../r/ref-r-revoscaler.md), входящие в состав Служб машинного обучения SQL Server, могут работать с большими наборами данных и использовать преимущества многопоточных многоядерных вычислений в базе данных в нескольких процессах.

Если решение R использует сложные агрегаты или включает в себя большие наборы данных, можно использовать высокоэффективные хранящиеся в памяти агрегаты и индексы columnstore SQL Server и переложить на код R обработку статистических вычислений и оценок.

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"

## <a name="adapt-r-code-for-other-platforms-or-compute-contexts"></a>Адаптация кода R для других платформ или контекстов вычисления

Один и тот же код R, который применяется к данным [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], можно использовать повторно для других источников данных, например Spark на базе HDFS, если выбрать [изолированный сервер](../install/sql-machine-learning-standalone-windows-install.md) в программе установки SQL Server или установить сервер Microsoft Machine Learning Server (прежнее название — **Microsoft R Server**):

+ [Документация по Machine Learning Server](/r-server/)

::: moniker-end

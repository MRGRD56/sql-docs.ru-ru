---
title: Учебник по Python. Прогнозирование стоимости поездки в нью-йоркском такси с использованием двоичной классификации
titleSuffix: SQL machine learning
description: Узнайте, как внедрять код Python в хранимые процедуры SQL Server и функции T-SQL с использованием машинного обучения SQL для прогнозирования стоимости поездки в нью-йоркском такси с помощью двоичной классификации.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/28/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 1fc4ea656830eb779b80b22a15a74dfd799781aa
ms.sourcegitcommit: 9b41725d6db9957dd7928a3620fe4db41eb51c6e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2020
ms.locfileid: "88178581"
---
# <a name="python-tutorial-predict-nyc-taxi-fares-with-binary-classification"></a>Учебник по Python. Прогнозирование стоимости поездки в нью-йоркском такси с использованием двоичной классификации
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
В этой серии руководств для программистов SQL вы узнаете об интеграции Python в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) или в [кластерах больших данных](../../big-data-cluster/machine-learning-services.md).
::: moniker-end

::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
В этой серии (из пяти частей) руководств для программистов SQL вы узнаете об интеграции Python в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md).
::: moniker-end

::: moniker range=">=azuresqldb-mi-current||=sqlallproducts-allversions"
В этой серии (из пяти частей) руководств для программистов SQL вы узнаете об интеграции Python в [Службы машинного обучения в управляемом экземпляре Azure SQL (предварительная версия)](/azure/azure-sql/managed-instance/machine-learning-services-overview).
::: moniker-end

Вы создадите и развернете решение для машинного обучения на базе Python, используя образец базы данных на SQL Server. Вы будете использовать T-SQL, Azure Data Studio или SQL Server Management Studio, а также экземпляр СУБД с поддержкой машинного обучения SQL и языка Python.

В этой серии руководств описываются функции Python, используемые в рабочем процессе моделирования данных. Серия содержит следующие этапы: исследование данных, сборка и обучение модели двоичной классификации и развертывание модели. Вы будете использовать образец данных Комиссии по такси и лимузинам Нью‑Йорка. Модель, которую вы создадите, будет предсказывать вероятность получения чаевых в зависимости от времени суток, пройденного расстояния и места посадки пассажира.

В первой части этой серии вы установите необходимые компоненты и восстановите образец базы данных. Во второй и третьей частях вы создадите сценарии Python для подготовки данных и обучения модели машинного обучения. Затем в четвертой и пятой частях вы запустите эти скрипты Python в базе данных с помощью хранимых процедур T-SQL.

Работая с этой статьей, вы узнаете о следующем.

> [!div class="checklist"]
> + Установка необходимых компонентов
> + Восстановление примера базы данных

Во [второй части](python-taxi-classification-explore-data.md) вы ознакомитесь с образцом данных и создадите несколько графиков.

В [третьей части](python-taxi-classification-create-features.md) вы узнаете, как создавать функции из необработанных данных с помощью функции Transact-SQL. Затем вы вызовите эту функцию из хранимой процедуры, чтобы создать таблицу, содержащую значения характеристик.

В [четвертой части](python-taxi-classification-train-model.md) вы научитесь загружать модули и вызывать необходимые функции для создания и обучения модели с помощью хранимой процедуры SQL Server.

Из [пятой части](python-taxi-classification-deploy-model.md) вы узнаете, как ввести в эксплуатацию модели, которые были обучены и сохранены в соответствии с инструкциями в четвертой части.

> [!NOTE]
> Это руководство доступно как для языка R, так и для Python. Сведения о версии R см. в разделе [Учебник по R. Прогнозирование стоимости поездки в нью-йоркском такси с использованием двоичной классификации](r-taxi-classification-introduction.md).

## <a name="prerequisites"></a>Предварительные условия

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions"
+ Установите [Службы машинного обучения SQL Server с поддержкой Python](../install/sql-machine-learning-services-windows-install.md#verify-installation)
::: moniker-end

+ [Предоставление разрешений на выполнение скриптов Python](../security/user-permission.md)

+ Восстановление демонстрационной базы данных [нью-йоркского такси](demo-data-nyctaxi-in-sql.md)

Все задачи можно выполнять с помощью хранимых процедур [!INCLUDE[tsql](../../includes/tsql-md.md)] в Azure Data Studio или [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

В этой серии руководств предполагается, что вы уже знакомы с основными операциями с базой данных, такими как создание баз данных и таблиц, импорт данных и написание запросов SQL. Не предполагается, что вы знакомы с Python, и весь код Python предоставляется в готовом виде.

## <a name="background-for-sql-developers"></a>Пояснения для разработчиков на SQL

Процесс создания решения машинного обучения — это сложная задача, для которой может потребоваться использование нескольких средств, а также координация работы экспертов в различных областях, и которая состоит из нескольких этапов:

+ получение и очистка данных;
+ изучение данных и выявление характеристик, полезных для моделирования;
+ обучение и настройка модели;
+ развертывание в рабочей среде.

Разработку и тестирование написанного кода лучше выполнять в выделенной среде разработки. Однако после полного тестирования сценария его можно легко развернуть в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью хранимых процедур [!INCLUDE[tsql](../../includes/tsql-md.md)] в знакомой среде Azure Data Studio или [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Упаковка внешнего кода в хранимые процедуры является основным механизмом для эксплуатации кода в SQL Server.

После сохранения модели в базе данных, можно вызвать ее для получения прогноза из [!INCLUDE[tsql](../../includes/tsql-md.md)] с помощью хранимых процедур.

Независимо от того, являетесь ли вы программистом SQL, плохо знакомым с Python, или разработчиком Python, плохо знакомым с SQL, в этом учебнике, состоящем из пяти частей, вы найдете типичный рабочий процесс для ведения аналитики в базе данных с помощью Python и SQL Server.

## <a name="next-steps"></a>Дальнейшие шаги

Работая с этой статьей, вы выполните следующие задачи:

> [!div class="checklist"]
> + Установленные компоненты
> + Восстановлена демонстрационная база данных

> [!div class="nextstepaction"]
> [Учебник по Python. Изучение и визуализация данных](python-taxi-classification-explore-data.md)
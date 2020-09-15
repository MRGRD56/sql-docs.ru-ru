---
title: Руководство по развертыванию модели кластеризации в R
titleSuffix: SQL Machine Learning
description: В этом цикле учебников, состоящем из четырех частей, вы создадите модель для кластеризации в R с использованием машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: cawrites
ms.author: chadam
ms.reviewer: garye, davidph
ms.date: 05/26/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: a57b3daec82469f87f9a7905e0230587e742d967
ms.sourcegitcommit: 9b41725d6db9957dd7928a3620fe4db41eb51c6e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2020
ms.locfileid: "88178772"
---
# <a name="tutorial-develop-a-clustering-model-in-r-with-sql-machine-learning"></a>Руководство по развертыванию модели кластеризации в R с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R для разработки и развертывания модели кластеризации методом k-средних в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) или [Кластерах больших данных](../../big-data-cluster/machine-learning-services.md) для классификации данных клиентов.
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R для разработки и развертывания модели кластеризации методом k-средних в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md) для кластеризации данных клиентов.
::: moniker-end
::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R для разработки и развертывания модели кластеризации методом k-средних в службах [SQL Server R Services](../r/sql-server-r-services.md) для кластеризации данных клиентов.
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
В этом цикле учебников, состоящем из четырех частей, вы будете использовать R для разработки и развертывания модели кластеризации методом K-средних в [Службах машинного обучения управляемого экземпляра SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview) для кластеризации данных клиентов.
::: moniker-end

В первой части этого цикла учебников вы настроите необходимые компоненты, а затем восстановите пример набора данных в базе данных. Во второй и третьей частях вы создадите сценарии R в записной книжке Azure Data Studio для анализа и подготовки этого примера данных, а также для обучения модели машинного обучения. Затем, в четвертой части, вы запустите эти сценарии R в базе данных с помощью хранимых процедур.

*Кластеризацию* можно описать как организацию данных по группам, где члены группы каким-либо образом похожи друг на друга. В рамках этой серии руководств вы можете представить себя владельцем розничного предприятия. Вы будете использовать метод **k-средних** для кластеризации клиентов в наборе данных о покупках и возвратах продуктов. Благодаря кластеризации клиентов вы можете более эффективно осуществлять маркетинговую деятельность, ориентируясь на конкретные группы. Кластеризация методом k-средних — это алгоритм *неконтролируемого обучения*, который ищет закономерности в данных на основе сходства.

В этой статье вы узнаете, как выполнять следующие задачи.

> [!div class="checklist"]
> * Восстановление примера базы данных

Во [второй части](r-clustering-model-prepare-data.md) вы узнаете, как подготовить данные из базы данных для выполнения кластеризации.

В [третьей части](r-clustering-model-build.md) вы узнаете, как создать и обучить модель кластеризации на основе k-средних в R.

В [четвертой части](r-clustering-model-deploy.md) вы узнаете, как создать хранимую процедуру в базе данных, которая может выполнять кластеризацию в R на основе новых данных.

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
* [Службы машинного обучения SQL Server с языком Python](../sql-server-machine-learning-services.md) — следуйте инструкциям по установке в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md) или [руководстве по установке для Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-machine-learning?toc=%2fsql%2fmachine-learning%2ftoc.json&view=sql-server-linux-ver15). Можно также [включить Службы машинного обучения в кластерах больших данных SQL Server](../../big-data-cluster/machine-learning-services.md).
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
* [Службы машинного обучения SQL Server](../sql-server-machine-learning-services.md) с языком R — следуйте инструкциям по установке в [руководстве по установке для Windows](../install/sql-machine-learning-services-windows-install.md).
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
* Службы машинного обучения в Управляемом экземпляре SQL Azure. Сведения о регистрации см. в статье [Общие сведения о Службах машинного обучения в управляемом экземпляре SQL Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview).

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) для восстановления образца базы данных в Управляемый экземпляр SQL Azure.
::: moniker-end

* [Azure Data Studio](../../azure-data-studio/what-is.md). Записную книжку в Azure Data Studio вы будете использовать для SQL. Дополнительные сведения о записных книжках см. в статье [Использование записных книжек в Azure Data Studio](../../azure-data-studio/notebooks-guidance.md).

* IDE R. В этом учебнике используется [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

* RODBC — данный драйвер используется в сценариях R, разрабатываемых в рамках этого учебника. Установите его с помощью команды R `install.packages("RODBC")`, если этот драйвер еще не установлен. Дополнительные сведения о RODBC см. в разделе [CRAN - Package RODBC](https://CRAN.R-project.org/package=RODBC) (CRAN: пакет RODBC).

## <a name="restore-the-sample-database"></a>Восстановление примера базы данных

Пример набора данных, используемый в этом учебнике, был сохранен в файл резервной копии базы данных **BAK**, чтобы его можно было скачать и использовать. Этот набор данных является производным от набора данных [tpcx-bb](http://www.tpc.org/tpcx-bb/default5.asp), предоставляемого [Советом по оценке производительности обработки транзакций (TPC)](http://www.tpc.org/).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
> [!NOTE]
> Если вы используете Службы машинного обучения в Кластерах больших данных, ознакомьтесь со статьей [Восстановление базы данных на главном экземпляре кластера больших данных SQL Server](../../big-data-cluster/data-ingestion-restore-database.md).
::: moniker-end

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions"
1. Скачайте файл [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Следуйте инструкциям из раздела [Восстановление базы данных из файла резервной копии](../../azure-data-studio/tutorial-backup-restore-sql-server.md#restore-a-database-from-a-backup-file) в Azure Data Studio, используя следующие сведения:

   * Выполните импорт из скачанного файла **tpcxbb_1gb.bak**.
   * Назовите целевую базу данных "tpcxbb_1gb".

1. Чтобы убедиться, что набор данных существует после восстановления базы данных, выполните запрос к таблице **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
1. Скачайте файл [tpcxbb_1gb.bak](https://sqlchoice.blob.core.windows.net/sqlchoice/static/tpcxbb_1gb.bak).

1. Следуйте инструкциям в разделе [Восстановление базы данных в Управляемый экземпляр](/azure/sql-database/sql-database-managed-instance-get-started-restore) в SQL Server Management Studio, используя следующие сведения.

   * Выполните импорт из скачанного файла **tpcxbb_1gb.bak**.
   * Назовите целевую базу данных "tpcxbb_1gb".

1. Чтобы убедиться, что набор данных существует после восстановления базы данных, выполните запрос к таблице **dbo.customer**:

    ```sql
    USE tpcxbb_1gb;
    SELECT * FROM [dbo].[customer];
    ```
::: moniker-end

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь продолжать работу с этим учебником, удалите базу данных tpcxbb_1gb.

## <a name="next-steps"></a>Дальнейшие действия

В первой части этого учебника вы выполнили следующие действия:

* Установка необходимых компонентов
* Восстановленный пример базы данных

Чтобы подготовить данные из для модели машинного обучения, перейдите ко второй части этого учебника:

> [!div class="nextstepaction"]
> [Подготовка данных к выполнению кластеризации](r-clustering-model-prepare-data.md)

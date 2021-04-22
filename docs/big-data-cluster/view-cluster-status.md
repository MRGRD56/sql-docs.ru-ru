---
title: Ресурсы администрирования для Кластеров больших данных (BDC)
titleSuffix: SQL Server
description: В этой статье объясняется, как просмотреть состояние кластера больших данных с помощью Azure Data Studio, записных книжек и команд Azure Data CLI (azdata).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 04/15/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b41666e4f0474d0e0843e8a1b78d83fcfab474be
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2021
ms.locfileid: "107584004"
---
# <a name="administration-resources-for-big-data-clusters-bdc"></a>Ресурсы администрирования для Кластеров больших данных (BDC)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Статья описывает, как администрировать [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] извне.

## <a name="know-your-architecture"></a>Знание архитектуры

Начиная с версии SQL Server 2019 (15.x), [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] позволяют развертывать масштабируемые кластеры SQL Server, Spark и контейнеров HDFS, работающие на базе Kubernetes. Обзор того, что представляют собой [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], см. в статье [Что такое Кластеры больших данных SQL Server?](big-data-cluster-overview.md).

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] обеспечивают согласованную и последовательную процедуру авторизации и проверки подлинности. Общие сведения об их защите см. в статье [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: основные понятия безопасности](concept-security.md).

## <a name="manage-and-operate-with-tools"></a>Управление инструментами и работа с ними

В следующих статьях описывается, как управлять кластером больших данных и работать с ним. 

- [Подключение к кластеру больших данных SQL Server с помощью Azure Data Studio](connect-to-big-data-cluster.md)
- [Управление кластерами больших данных для информационной панели контроллера SQL Server](manage-with-controller-dashboard.md)
- [Управление кластерами больших данных SQL Server с помощью записных книжек Azure Data Studio ](notebooks-manage-bdc.md)
- [Управление Кластерами больших данных (BDC) в кластере с помощью записных книжек](cluster-manage-notebooks.md)
- [Запуск примера записной книжки с помощью Spark](notebooks-tutorial-spark.md)

## <a name="monitor-with-tools"></a>Мониторинг с помощью инструментов

В следующих статьях описываются способы мониторинга кластера больших данных. 

- [Выполнение мониторинга кластера BDC с помощью Azure Data Studio](cluster-monitor-ads.md)
- [Выполнение мониторинга кластера BDC с помощью служебной программы Azdata](cluster-monitor-cmdlet.md)
- [Выполнение мониторинга кластера BDC с помощью панели мониторинга Grafana](cluster-monitor-grafana.md)
- [Выполнение мониторинга кластера BDC с помощью записных книжек Juypter и Azure Data Studio](cluster-monitor-notebooks.md)

## <a name="monitor-and-inspect-logs-with-notebooks"></a>Мониторинг и проверка журналов с помощью записных книжек

Далее перечислены многие записные книжки Jupyter, доступные в Azure Data Studio.

- [Мониторинг кластера с помощью записных книжек](cluster-monitor-notebooks.md)
- [Сбор и анализ журналов в кластере с помощью записных книжек](cluster-logging-notebooks.md)

## <a name="big-data-clusters-troubleshooting-resources"></a>Ресурсы по устранению неполадок Кластеров больших данных

В следующих статьях описывается устранение неполадок кластера больших данных.

- [Устранение неполадок кластера BDC с помощью служебной программы kubectl](cluster-troubleshooting-commands.md) 
- [Устранение неполадок записной книжки pyspark](troubleshoot-pyspark-notebook.md)
- [Устранение неполадок кластера BDC с помощью записных книжек Juypter и Azure Data Studio (ADS)](cluster-troubleshooter-notebooks.md)
- [Восстановление разрешений HDFS](troubleshoot-hdfs-restore-admin.md)

В следующих статьях описывается устранение неполадок кластера больших данных, развернутого в режиме Active Directory.
- [Устранение неполадок кластера BDC в режиме Active Directory](troubleshoot-active-directory.md) 
- [Устранение неполадок с ошибкой при входе в режим Active Directory](troubleshoot-ad-login-failed-untrusted-domain.md)
- [Устранение неполадок при остановке развертывания в режиме AD BDC](troubleshoot-ad-reverse-lookup-zone.md)


## <a name="where-to-find-big-data-clusters-2019-administration-notebooks"></a>[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: где найти записные книжки по администрированию 

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] предоставляют широкие возможности администрирования в формате работы с записными книжками Jupyter. Предлагаются записные книжки по операциям кластера, управлению, мониторингу, ведению журналов и устранению неполадок. 


### <a name="access-to-local-notebooks"></a>Доступ к локальным записным книжкам 

После успешного подключения к кластеру больших данных с помощью Azure Data Studio (ADS) вы можете перейти на вкладку "[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]", где доступна прямая ссылка на все локальные записные книжки, как показано ниже: 

![Локальные руководства по кластерам больших данных](media/view-cluster-status/bdc-local-guides.png)

Записные книжки также доступны напрямую из Azure Data Studio (ADS). Нажмите клавиши CTRL+SHIFT+P или в меню "Вид" выберите "Палитра команд", чтобы увидеть пункт "Книги Jupyter. Получение локализованного руководства по SQL Server 2019". 


### <a name="add-remote-notebooks"></a>Добавление удаленных записных книжек

Записные книжки приходят с удаленных источников, поэтому вы можете получить любые нужные вам версии. Чтобы добавить удаленную записную книжку из Azure Data Studio (ADS), нажмите клавиши CTRL+SHIFT+P или в меню "Вид" выберите "Палитра команд", как показано ниже:

![Палитра ADS](media/view-cluster-status/bdc-ads-palette.png)

Вы увидите пункт "Книги Jupyter. Добавление удаленной книги" и панель, на которой можно выбрать записные книжки для нужной версии.

![Удаленные руководства по кластерам больших данных](media/view-cluster-status/bdc-remote-guides.png)

Щелкнув "Добавить", вы получите доступ ко всем записным книжкам нужной версии на вкладке "Записные книжки" в Azure Data Studio, как показано ниже: 

![Руководства по кластерам больших данных в ADS](media/view-cluster-status/bdc-ads-guides.png)


### <a name="how-to-use-these-notebooks"></a>Как использовать эти записные книжки 

Руководство по использованию этих записных книжек можно найти в следующих статьях:

- [Выполнение мониторинга кластера BDC с помощью записных книжек Juypter и Azure Data Studio](cluster-monitor-notebooks.md)
- [Сбор и анализ журналов в кластере с помощью записных книжек](cluster-logging-notebooks.md)
- [Устранение неполадок записной книжки pyspark](troubleshoot-pyspark-notebook.md)
- [Устранение неполадок кластера BDC с помощью записных книжек Juypter и Azure Data Studio (ADS)](cluster-troubleshooter-notebooks.md)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о кластерах больших данных см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).
---
title: Общие сведения о настройке кластеров больших данных SQL Server
titleSuffix: SQL Server big data clusters
description: Общие сведения о настройке кластеров больших данных
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cda6e61e8f5f13f5fd414879f888c7ed72a1bd0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343980"
---
# <a name="configure-a-sql-server-big-data-cluster"></a>Настройка кластера больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
Управление конфигурацией позволяет администраторам обеспечивать постоянную готовность кластеров больших данных к выполнению рабочих нагрузок. С помощью этой функции администраторы кластера больших данных могут изменять или настраивать различные его компоненты во время или после развертывания, а также получать более подробные сведения о конфигурациях, используемых в кластере больших данных. 

Управление конфигурацией позволяет администратору включать агент SQL, определять базовые ресурсы для заданий Spark организации и даже узнавать, какие параметры можно настроить в каждой области. Во время развертывания конфигурации можно настроить с помощью файла `bdc.json`, а после развертывания — с помощью интерфейса CLI azdata.

## <a name="configuration-scopes"></a>Области конфигурации
Конфигурация кластеров больших данных включает три уровня: `cluster`, `service` и `resource`. Иерархия параметров также следует этому порядку — от высшего к низшему. Компоненты Кластеров больших данных принимают значение параметра, определенное на самом низком уровне. Если параметр не определен в заданной области, он наследует значение из более высокой родительской области.

Например, вы можете определить число ядер по умолчанию для использования драйвером Spark в ресурсах пула носителей и `Sparkhead`. Чтобы определить число ядер по умолчанию, можно выполнить одно из указанных ниже действий.

- Задайте число ядер по умолчанию в области службы `Spark`.

- Задайте число ядер по умолчанию в области ресурсов `storage-0` и `sparkhead`.

В первом сценарии все ресурсы низшей области службы Spark (пул носителей и `Sparkhead`) *наследуют* число ядер по умолчанию из значения службы Spark по умолчанию.

Во втором сценарии каждый ресурс будет использовать значение, определенное в соответствующей области.

Если число ядер по умолчанию настроено как в области службы, так и в области ресурсов, значение из области ресурсов будет переопределять значение из области службы, так как это самая низкая **настроенная пользователем** область для заданного параметра.

## <a name="next-steps"></a>Дальнейшие действия

Конкретные сведения о конфигурации см. в соответствующих статьях:

- [Настройка развертывания кластеров больших данных](deployment-custom-configuration.md)
- [Настройка кластеров больших данных после развертывания](configure-bdc-postdeployment.md)
- [Настройка кластеров больших данных с накопительным пакетом обновления 8 или более ранней версии](configure-bdc-pre-configuration.md)

Справочные материалы. 
- [Свойства конфигурации кластеров больших данных SQL Server](reference-config-bdc-overview.md)
- [Свойства конфигурации Apache Spark и Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Свойства конфигурации главного экземпляра SQL Server (до накопительного пакета обновления 9)](reference-config-master-instance.md)
- [Интерфейс CLI azdata](../azdata/reference/reference-azdata.md)
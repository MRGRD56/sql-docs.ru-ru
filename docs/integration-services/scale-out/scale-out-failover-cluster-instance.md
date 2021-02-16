---
title: Поддержка горизонтального увеличения масштаба для обеспечения высокой доступности с помощью экземпляра отказоустойчивого кластера SQL Server | Документация Майкрософт
description: В этой статье описывается настройка высокой доступности для развертывания SSIS с горизонтальным увеличением масштаба с помощью экземпляра отказоустойчивого кластера SQL Server.
ms.custom: performance
ms.date: 04/10/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 444226f7e0f28a6f587a5f01b1e512f2392e9916
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351963"
---
# <a name="scale-out-support-for-high-availability-via-sql-server-failover-cluster-instance"></a>Поддержка горизонтального увеличения масштаба для обеспечения высокой доступности с помощью экземпляра отказоустойчивого кластера SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Чтобы обеспечить высокий уровень доступности на стороне мастера горизонтального увеличения масштаба с использованием экземпляра отказоустойчивого кластера SQL Server, выполните следующие действия:

## <a name="1-prerequisites"></a>1. Предварительные требования
Настроить отказоустойчивый кластер Windows. Инструкции см. в записи блога [Установка компонента и средств отказоустойчивого кластера для Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733). Установите компоненты и средства на всех узлах кластера.

## <a name="2-install-sql-server-failover-cluster"></a>2. Установка отказоустойчивого кластера SQL Server
Установите отказоустойчивый кластер SQL Server. Инструкции см. в разделе [Установка отказоустойчивого кластера SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md). Во время установки на странице "Выбор компонентов" щелкните "Службы ядра СУБД". Запишите сетевое имя SQL Server для последующей настройки.

![Сетевое имя SQL](media/sql-network-name.PNG)

Добавьте вторичный узел в отказоустойчивый кластер SQL Server.

## <a name="3-install-scale-out-master-on-the-primary-node"></a>3. Установка мастера горизонтального увеличения масштаба в основном узле
Установите службы Integration Services и мастер горизонтального увеличения масштаба в основном узле с помощью мастера некластеризованной установки. 

Во время установки включите сетевое имя SQL Server в общие имена сертификата мастера горизонтального увеличения масштаба.

> [!NOTE]
> Чтобы реализовать раздельную отработку отказа для служб SSISDB и мастера горизонтального увеличения масштаба, выполните шаг [2. Установка мастера горизонтального увеличения масштаба в основном узле](scale-out-support-for-high-availability.md#2-install-scale-out-master-on-the-primary-node) для конфигурации мастера горизонтального увеличения масштаба.

## <a name="4-install-scale-out-master-on-the-secondary-node"></a>4. Установка мастера горизонтального увеличения масштаба во вторичном узле
Выполните шаг [3. Установка мастера горизонтального увеличения масштаба во вторичном узле](scale-out-support-for-high-availability.md#3-install-scale-out-master-on-the-secondary-node)

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Обновление файла конфигурации для службы мастера горизонтального увеличения масштаба
Обновите файл конфигурации для службы мастера горизонтального увеличения масштаба (\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config) на основном и дополнительном узлах. Обновите **SqlServerName** на [сетевое имя SQL Server]//[имя экземпляра] или [сетевое имя SQL Server] для экземпляра по умолчанию.

## <a name="6-add-scale-out-master-service-to-sql-server-role-in-windows-failover-cluster"></a>6. Добавление службы мастера горизонтального увеличения масштаба в роль SQL Server в кластере Windows
В диспетчере отказоустойчивости кластеров установите подключение к кластеру для горизонтального увеличения масштаба. Выберите роли в обозревателе, щелкните правой кнопкой мыши роль SQL Server и выберите "Добавить ресурс универсальной службы". 

![Универсальная служба](media/generic-service.PNG)

В мастере создания ресурса выберите службу мастера горизонтального увеличения масштаба и завершите работу мастера. 

Включите службу мастера горизонтального увеличения масштаба.

![Включение службы](media/bring-online.PNG)

> [!NOTE]
> Чтобы реализовать раздельную отработку отказа для служб SSISDB и мастера горизонтального увеличения масштаба, выполните шаг [7. Настройка роли службы мастера горизонтального увеличения масштаба для отказоустойчивого кластера Windows](scale-out-support-for-high-availability.md#7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster)

## <a name="7-install-scale-out-workers"></a>7. Установка рабочих ролей горизонтального увеличения масштаба
Установите рабочую роль горизонтального увеличения масштаба на рабочие узлы. Во время установки укажите https://[сетевое имя SQL Server]:[порт мастера] в качестве конечной точки мастера. 

> [!NOTE]
> Чтобы реализовать раздельную отработку отказа для служб SSISDB и мастера горизонтального увеличения масштаба, укажите DNS-имя хоста службы мастера горизонтального увеличения масштаба вместо сетевого имени SQL Server.

## <a name="8-install-scale-out-worker-client-certificate"></a>8. Установка сертификата клиента рабочей роли горизонтального увеличения масштаба
Установите сертификат рабочей роли на всех узлах отказоустойчивого кластера SQL Server. См. раздел [Установка сертификата клиента рабочей роли горизонтального увеличения масштаба](walkthrough-set-up-integration-services-scale-out.md#InstallCert).

> [!NOTE]
> Диспетчер горизонтального увеличения масштаба не поддерживает отказоустойчивый кластер SQL Server. Если для добавления рабочей роли горизонтального увеличения масштаба вы используете диспетчер горизонтального увеличения масштаба, вам по-прежнему требуется вручную установить сертификат рабочей роли на все узлы мастера.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения см. в следующих статьях:
-   [Мастер горизонтального увеличения масштаба служб Integration Services (SSIS)](integration-services-ssis-scale-out-master.md)
-   [Рабочая роль горизонтального увеличения масштаба служб Integration Services (SSIS)](integration-services-ssis-scale-out-worker.md)

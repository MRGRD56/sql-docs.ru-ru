---
title: Настройка группы доступности для чтения и масштабирования (SQL Server на Linux)
description: Сведения о настройке группы доступности Always On SQL Server для рабочих нагрузок для чтения и масштабирования в Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 5cad1c63817bf55231c065123b2456590dbfebb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352746"
---
# <a name="configure-a-sql-server-availability-group-for-read-scale-on-linux"></a>Настройка группы доступности SQL Server для чтения и масштабирования в Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Группу доступности Always On SQL Server для рабочих нагрузок для чтения и масштабирования можно настроить в Linux. Существует два типа архитектур для групп доступности. Архитектура для высокого уровня доступности использует диспетчер кластера для улучшенного обеспечения непрерывности бизнес-процессов. Она может также содержать реплики для чтения и масштабирования. Сведения о создании архитектуры с высоким уровнем доступности см. в статье [Настройка группы доступности Always On SQL Server для обеспечения высокой доступности в Linux](sql-server-linux-availability-group-configure-ha.md). Другая архитектура поддерживает только рабочие нагрузки для чтения и масштабирования. В этой статье описывается создание группы доступности для рабочих нагрузок чтения и масштабирования без диспетчера кластеров. Эта архитектура обеспечивает только чтение и масштабирование. Она не поддерживает высокий уровень доступности.

> [!NOTE]
> В группу доступности с `CLUSTER_TYPE = NONE` могут входить реплики, размещенные на различных платформах операционных систем. Она не поддерживает высокий уровень доступности. 

[!INCLUDE [Create prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Создание группы доступности

Создайте группу доступности. Задайте `CLUSTER_TYPE = NONE`. Кроме того, задайте для каждой реплики `FAILOVER_MODE = MANUAL`. Клиентские приложения с рабочими нагрузками в области аналитики и отчетности могут напрямую подключаться к базам данных-получателям. Также можно создать список маршрутизации только для чтения. Подключения к первичной реплике будут переадресовывать запросы на подключение для чтения к каждой из содержащихся в нем вторичных реплик по принципу циклического перебора.

Следующий сценарий Transact-SQL создает группу доступности `ag1`. Сценарий настраивает реплики группы доступности с параметром `SEEDING_MODE = AUTOMATIC`. Если этот параметр задан, SQL Server будет автоматически создавать базы данных на каждом сервере-получателе после его добавления в группу доступности. Обновите следующий сценарий для своей среды. Замените значения `<node1>` и `<node2>` на имена экземпляров SQL Server, где размещаются реплики. Замените значение `<5022>` на порт, заданный для конечной точки. Выполните следующий сценарий Transact-SQL на первичной реплике SQL Server:

```SQL
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
        N'<node2>' WITH ( 
            ENDPOINT_URL = N'tcp://<node2>:<5022>', 
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
            SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            );
        
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-secondary-sql-servers-to-the-ag"></a>Присоединение дополнительных серверов SQL к группе доступности

Следующий сценарий Transact-SQL создает группу доступности `ag1`. Обновите сценарий для своей среды. На каждой вторичной реплике SQL Server выполните следующий сценарий Transact-SQL для присоединения группы доступности:

```SQL
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../includes/ss-linux-cluster-availability-group-create-post.md)]

У этой группы доступности нет конфигурации высокого уровня доступности. Если вам требуется высокий уровень доступности, следуйте инструкциям в статье [Настройка группы доступности Always On для SQL Server в Linux](sql-server-linux-availability-group-configure-ha.md). В частности, создайте группу доступности с параметром `CLUSTER_TYPE=WSFC` (в Windows) или `CLUSTER_TYPE=EXTERNAL` (в Linux). Затем выполните интеграцию с диспетчером кластеров с помощью отказоустойчивой кластеризации Windows Server в Windows или Pacemaker в Linux.

## <a name="connect-to-read-only-secondary-replicas"></a>Подключение к вторичным репликам только для чтения

Существует два способа подключения к вторичным репликам только для чтения. Приложения могут подключаться непосредственно к экземпляру SQL Server, на котором размещена вторичная реплика, и отправлять запросы к базам данных. Они также могут использовать маршрутизацию только для чтения, для которой требуется прослушиватель.

* [Доступные для чтения вторичные реплики](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)
* [Маршрутизация только для чтения](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary)

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Отработка отказа первичной реплики в группе доступности для чтения и масштабирования

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Дальнейшие действия

* [Настройка распределенной группы доступности](../database-engine/availability-groups/windows/distributed-availability-groups.md)
* [Дополнительные сведения о группах доступности](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
* [Выполнение принудительного перехода на другой ресурс вручную](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)
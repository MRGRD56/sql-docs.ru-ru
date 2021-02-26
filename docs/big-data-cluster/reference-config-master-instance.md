---
title: Свойства конфигурации главного экземпляра SQL Server
titleSuffix: SQL Server big data clusters
description: Справочная статья по свойствам конфигурации главного экземпляра SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2d986013374e7f69111288d2d0f50b09130a2d68
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343509"
---
# <a name="sql-server-master-instance-configuration-properties----pre-cu9-release"></a>Свойства конфигурации главного экземпляра SQL Server (до накопительного пакета обновления 9)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!NOTE]
> Следующие сведения относятся только к кластерам с накопительным пакетом обновления до версии 9, не поддерживающим конфигурации и требующим mssql-conf для настройки главного экземпляра SQL Server. В кластерах с накопительным пакетом обновления 9 и более поздних версий используется функция управления конфигурацией, поэтому файл mssql-conf больше не требуется. Доступные конфигурации для главного экземпляра SQL Server и других компонентов кластера больших данных см. [здесь](reference-config-bdc-overview.md).

## <a name="properties"></a>Свойства

Во время развертывания для главного экземпляра можно настроить следующие параметры SQL Server.

|Свойство|Параметры|
| --- | --- |
|`[sqlagent]`|`enabled = { true | false }` |
|`[telemetry]`|`customerfeedback = { true | false }` |
|`[telemetry]`|`userRequestedLocalAuditDirectory = </path/file>`|
|`[licensing]`| `pid = { Enterprise | Developer }`|
|`[traceflag]`|` traceflag<#> = <####>`|

### <a name="examples"></a>Примеры

В следующем примере показано включение агента SQL, телеметрии, установка PID для выпуска Enterprise и включение флага трассировки 1204.

```
[sqlagent]
enabled=true

[telemetry]
customerfeedback=true
userRequestedLocalAuditDirectory = /tmp/audit

[licensing]
pid = Enterprise

[traceflag]
traceflag0 = 1204
```

Инструкции см. в статье [Настройка главного экземпляра для [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md).

## <a name="next-steps"></a>Дальнейшие шаги

[Настройка главного экземпляра [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](configure-sql-server-master-instance.md)

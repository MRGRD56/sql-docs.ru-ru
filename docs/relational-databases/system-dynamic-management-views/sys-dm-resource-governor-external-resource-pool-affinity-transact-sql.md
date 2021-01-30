---
description: sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)
title: sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_resource_governor_external_resource_pool_affinity
- sys.dm_resource_governor_external_resource_pool_affinity_TSQL
- dm_resource_governor_external_resource_pool_affinity
- dm_resource_governor_external_resource_pool_affinity_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_external_resource_pool_affinity
- dm_resource_governor_external_resource_pool_affinity
ms.assetid: e32fac49-5161-47c0-8540-af3fe730c00c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1dbbbbfacabb47fc1c18bfce9930ae93bc0f266b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203374"
---
# <a name="sysdm_resource_governor_external_resource_pool_affinity-transact-sql"></a>sys.dm_resource_governor_external_resource_pool_affinity (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]
**Применимо к:** [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)]и [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)][!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]

Возвращает сведения о сходстве ЦП с текущей конфигурацией внешнего пула ресурсов.
  
|Имя столбца|Тип данных|Описание|
|----------------|---------------|-----------------|
|pool_id|**int**|ИДЕНТИФИКАТОР внешнего пула ресурсов. Не допускает значение NULL.|
|processor_group|**smallint**|Идентификатор логической группы процессоров Windows. Не допускает значение NULL.|
|cpu_mask|**bigint**|Двоичная маска, представляющая процессоры, связанные с этим пулом. Не допускает значение NULL.|
  
## <a name="remarks"></a>Замечания

Пулы, созданные с помощью сходства, `AUTO` не отображаются в этом представлении, так как они не имеют сходства. Дополнительные сведения см. в статьях [Создание пула внешних ресурсов &#40;Transact-sql&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md) и [Изменение внешнего пула ресурсов &#40;инструкций transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md) .

## <a name="permissions"></a>Разрешения

Требуется разрешение `VIEW SERVER STATE`.

## <a name="see-also"></a>См. также раздел

[Управление ресурсами для машинного обучения в SQL Server](../../machine-learning/administration/resource-governor.md)

[sys.dm_resource_governor_resource_pool_affinity &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pool-affinity-transact-sql.md)

[Параметр конфигурации сервера «external scripts enabled»](../../database-engine/configure-windows/external-scripts-enabled-server-configuration-option.md)

[ALTER EXTERNAL RESOURCE POOL (Transact-SQL)](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)

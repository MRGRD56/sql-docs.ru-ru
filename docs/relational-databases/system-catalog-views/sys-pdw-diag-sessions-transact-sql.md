---
description: sys.pdw_diag_sessions (Transact-SQL)
title: sys.pdw_diag_sessions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 4d23688a-cddb-4eed-8231-ecde2a0b0e65
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 2968fbd2fd9dc68b9130ec08225ecae7b268b3e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203744"
---
# <a name="syspdw_diag_sessions-transact-sql"></a>sys.pdw_diag_sessions (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Содержит сведения о различных диагностических сеансах, созданных в системе.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|**name**|**nvarchar(255)**|Имя сеанса диагностики.<br /><br /> Ключ для этого представления.||  
|**xml_data**|**nvarchar(4000)**|Полезные XML-данные, описывающие сеанс.||  
|**is_active**|**bit**|Флаг, указывающий, активен ли флаг.||  
|**host_address**|**nvarchar(255)**|Адрес компьютера, на котором размещается определение сеанса (управляющий узел).||  
|**principal_id**|**int**|Идентификатор пользователя, создавшего сеанс на уровне базы данных.||  
|**database_id**|**int**|Идентификатор базы данных, которая является областью диагностического сеанса.|  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога Azure Synapse Analytics и Parallel Data Warehouse](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  

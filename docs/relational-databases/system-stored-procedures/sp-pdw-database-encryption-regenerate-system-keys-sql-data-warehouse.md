---
title: sp_pdw_database_encryption_regenerate_system_keys (Azure синапсе Analytics) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: bb13e323-a984-4462-8b6d-6019c38ddd9d
author: ronortloff
ms.author: rortloff
ms.reviewer: ''
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 3d7a4efb9a629938b6cfc7d48c990351bc2a5cbc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271781"
---
# <a name="sp_pdw_database_encryption_regenerate_system_keys-azure-synapse-analytics"></a>sp_pdw_database_encryption_regenerate_system_keys (Azure синапсе Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Используйте **sp_pdw_database_encryption_regenerate_system_keys** для смены сертификата и ключа шифрования базы данных для внутренних баз данных, которые шифруются при включении TDE на устройстве. в том числе и `tempdb`. Это будет успешным, только если включен TDE.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption_regenerate_system_keys  ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Remarks  
 Процедура не имеет параметров.  
  
 Эта процедура должна использоваться, если трафик на устройстве имеет низкий уровень.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в предопределенной роли базы данных **sysadmin** или разрешение **Control Server** .  
  
## <a name="example"></a>Пример  
 В следующем примере повторно создаются ключи шифрования базы данных.  
  
```sql  
EXEC sys.sp_pdw_database_encryption_regenerate_system_keys;  
```  
  
## <a name="see-also"></a>См. также раздел  
 [sp_pdw_database_encryption &#40;Azure синапсе Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_log_user_data_masking &#40;Azure синапсе Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  

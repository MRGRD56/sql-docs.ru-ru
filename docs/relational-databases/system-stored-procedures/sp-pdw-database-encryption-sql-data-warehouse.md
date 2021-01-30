---
description: sp_pdw_database_encryption (Azure синапсе Analytics)
title: sp_pdw_database_encryption (Azure синапсе Analytics) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: f5ccb424-7a95-4557-b774-c69de33c1545
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 0c1d6af9bb11e4078314f27d9fd69346568204d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211870"
---
# <a name="sp_pdw_database_encryption-azure-synapse-analytics"></a>sp_pdw_database_encryption (Azure синапсе Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Используйте **sp_pdw_database_encryption** , чтобы включить прозрачное шифрование данных для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] устройства. Если **sp_pdw_database_encryption** установлен в значение 1, используйте инструкцию **ALTER DATABASE** для шифрования базы данных с помощью TDE.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption [ [ @enabled = ] enabled ] ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

#### <a name="parameters"></a>Параметры  
`[ @enabled = ] enabled` Определяет, включено ли прозрачное шифрование данных. *Enabled* имеет **тип int** и может принимать одно из следующих значений:  
  
-   0 = отключено  
  
-   1 = включено  
  
 При запуске **sp_pdw_database_encryption** без параметров возвращается текущее состояние TDE на устройстве в виде скалярного результирующего набора: 0 для отключенного или 1 для включенного.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 Если TDE включен с помощью **sp_pdw_database_encryption**, база данных tempdb удаляется, создается повторно и шифруется. По этой причине TDE нельзя включить на устройстве, пока имеются другие активные сеансы, использующие базу данных tempdb. Включение или отключение TDE на устройстве — это действие, которое изменяет состояние устройства. в большинстве случаев ожидается, что оно будет выполняться один раз в течение времени существования устройства и должно выполняться при отсутствии трафика на устройстве.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в предопределенной роли базы данных **sysadmin** или разрешение **Control Server** .  
  
## <a name="example"></a>Пример  
 В следующем примере показано включение TDE на устройстве.  
  
```sql  
EXEC sys.sp_pdw_database_encryption 1;  
```  
  
## <a name="see-also"></a>См. также раздел  
 [sp_pdw_database_encryption_regenerate_system_keys &#40;Azure синапсе Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)   
 [sp_pdw_log_user_data_masking &#40;Azure синапсе Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  

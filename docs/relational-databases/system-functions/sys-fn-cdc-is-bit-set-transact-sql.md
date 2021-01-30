---
description: sys.fn_cdc_is_bit_set (Transact-SQL)
title: sys.fn_cdc_is_bit_set (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_cdc_is_bit_set
- sys.fn_cdc_is_bit_set_TSQL
- sys.fn_cdc_is_bit_set
- fn_cdc_is_bit_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_cdc_is_bit_set
- fn_cdc_is_bit_set
ms.assetid: 792fe7cf-b3b8-4f25-8329-78d63f0e6921
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 77d06cc44b1c7c090e0da7cce360d0a7eed12a71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206115"
---
# <a name="sysfn_cdc_is_bit_set-transact-sql"></a>sys.fn_cdc_is_bit_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Указывает, был ли обновлен отслеживаемый столбец, проверяя установку его порядкового положения в битовой маске.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_cdc_is_bit_set ( position , update_mask )  
```  
  
## <a name="arguments"></a>Аргументы  
 *position*  
 Порядковое положение в битовой маске. *позицией* является **int**.  
  
 *update_mask*  
 Маска, идентифицирующая обновленные столбцы. *update_mask* имеет тип **varbinary (128)**.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 **bit**  
  
## <a name="remarks"></a>Замечания  
 Эта функция обычно используется как часть запроса изменения данных для определения изменения столбца. В этом сценарии функция [sys.fn_cdc_get_column_ordinal](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md) используется перед запросом, чтобы получить требуемый порядковый номер столбца. **sys.fn_cdc_is_bit_set** затем применяется к каждой строке возвращаемых данных об изменениях, предоставляя сведения о конкретном столбце как часть возвращаемого результирующего набора.  
  
 Мы советуем использовать эту функцию вместо функции, [sys.fn_cdc_has_column_changed](../../relational-databases/system-functions/sys-fn-cdc-has-column-changed-transact-sql.md) при определении того, изменились ли столбцы для всех строк возвращенного результирующего набора.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере функция `sys.fn_cdc_is_bit_set` используется для присоединения к результирующему набору, созданному функцией запроса `cdc.fn_cdc_get_all_changes_HR_Department`, столбца `IsGroupNmUpdated` с помощью предварительно вычисленного порядкового номера столбца и значения `__$update_mask` в качестве аргументов.  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @from_lsn binary(10), @to_lsn binary(10), @GroupNm_ordinal int;  
SET @from_lsn = sys.fn_cdc_get_min_lsn('HR_Department');  
SET @to_lsn = sys.fn_cdc_get_max_lsn();  
SET @GroupNm_ordinal = sys.fn_cdc_get_column_ordinal('HR_Department','GroupName');  
  
SELECT sys.fn_cdc_is_bit_set(@GroupNm_ordinal,__$update_mask) as 'IsGroupNmUpdated', *  
FROM cdc.fn_cdc_get_all_changes_HR_Department( @from_lsn, @to_lsn, 'all')  
WHERE __$operation = 4;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Функции системы отслеживания измененных данных (Transact-SQL)](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)   
 [sys.fn_cdc_get_column_ordinal &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md)   
 [sys.fn_cdc_has_column_changed &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-cdc-has-column-changed-transact-sql.md)   
 [cdc.fn_cdc_get_all_changes_&#60;capture_instance&#62;  &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-all-changes-capture-instance-transact-sql.md)   
 [cdc.fn_cdc_get_net_changes_&#60;capture_instance&#62; &#40;Transact-SQL&#41;](../../relational-databases/system-functions/cdc-fn-cdc-get-net-changes-capture-instance-transact-sql.md)   
 [Об отслеживании измененных данных (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)  
  
  

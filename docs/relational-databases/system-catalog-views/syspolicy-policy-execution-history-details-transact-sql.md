---
description: syspolicy_policy_execution_history_details (Transact-SQL)
title: syspolicy_policy_execution_history_details (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_policy_execution_history_details
- syspolicy_policy_execution_history_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policy_execution_history_details view
ms.assetid: 97ef6573-5e8b-4ba5-8ae0-7901e79a9683
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d10e65b6a701acaabc80418d59da019250acf24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171831"
---
# <a name="syspolicy_policy_execution_history_details-transact-sql"></a>syspolicy_policy_execution_history_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Показывает выполненные условные выражения, целевые объекты выполнения выражений, результат каждого выполнения и сведения об ошибках, возникших при выполнении. В следующей таблице приведено описание столбцов представления syspolicy_execution_history_details.  
  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|detail_id|**bigint**|Идентификатор записи. Каждая запись соответствует попытке выполнить или обеспечить одно условное выражение из политики. Если условие применялось к нескольким целевым объектам, для каждого условия будет выведено несколько детализированных записей — по одной на целевой объект.|  
|history_id|**bigint**|Идентификатор события в журнале. Каждое событие в журнале соответствует одной попытке выполнения политики. Поскольку одно условие может включать несколько условных выражений и несколько целевых объектов, history_id может создавать несколько детализированных записей. Используйте столбец history_id, чтобы присоединить это представление к представлению [syspolicy_policy_execution_history](../../relational-databases/system-catalog-views/syspolicy-policy-execution-history-transact-sql.md) .|  
|target_query_expression|**nvarchar(max)**|Целевой объект политики и представления syspolicy_policy_execution_history.|  
|execution_date|**datetime**|Дата и время создания этой детализированной записи.|  
|набор по|**bit**|Успешный или неуспешный результат вычисления условного выражения для данного целевого объекта:<br /><br /> 0 (успешное завершение) или 1 (неуспешное завершение)|  
|result_detail|**nvarchar(max)**|Результирующее сообщение. Генерируется только в том случае, когда обеспечивается аспектом.|  
|exception_message|**nvarchar(max)**|Сообщение, выданное в результате возникшего исключения.|  
|exception|**nvarchar(max)**|Описание возникшего исключения.|  
  
## <a name="remarks"></a>Замечания  
 При устранении неполадок управления на основе политик с помощью запроса представления syspolicy_policy_execution_history_details можно определить, для каких сочетаний целевого объекта и условного выражения результат был неуспешным, когда это произошло, и просмотреть соответствующие ошибки.  
  
 Далее приводится запрос, сочетающий представления `syspolicy_policy_execution_history_details`, `syspolicy_policy_execution_history_details` и `syspolicy_policies` и выводящий имя политики, имя условия и подробные сведения об ошибках.  
  
```  
SELECT Pol.name AS Policy,   
Cond.name AS Condition,   
PolHistDet.target_query_expression,   
PolHistDet.execution_date,   
PolHistDet.result,   
PolHistDet.result_detail,   
PolHistDet.exception_message,   
PolHistDet.exception   
FROM msdb.dbo.syspolicy_policies AS Pol  
JOIN msdb.dbo.syspolicy_conditions AS Cond  
    ON Pol.condition_id = Cond.condition_id  
JOIN msdb.dbo.syspolicy_policy_execution_history AS PolHist  
    ON Pol.policy_id = PolHist.policy_id  
JOIN msdb.dbo.syspolicy_policy_execution_history_details AS PolHistDet  
    ON PolHist.history_id = PolHistDet.history_id  
WHERE PolHistDet.result = 0 ;  
```  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в роли PolicyAdministratorRole базы данных msdb.  
  
## <a name="see-also"></a>См. также  
 [Администрирование серверов с помощью управления на основе политик](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Административные представления на основе политик (Transact-SQL)](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  

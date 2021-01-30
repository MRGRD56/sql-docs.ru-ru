---
description: sys.security_predicates (Transact-SQL)
title: sys.security_predicates (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.SECURITY_PREDICATES
- SECURITY_PREDICATES
- SECURITY_PREDICATES_TSQL
- SYS.SECURITY_PREDICATES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_predicates catalog view
- security_predicates catalog view
ms.assetid: c7a2f28c-98da-463d-8b8a-8e5619e2c6a6
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8220ca6cefc82682201e3b06244cbed5a7f27db1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182489"
---
# <a name="syssecurity_predicates-transact-sql"></a>sys.security_predicates (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Возвращает строку для каждого предиката безопасности в базе данных.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Идентификатор политики безопасности, содержащей этот предикат.|  
|security_predicate_id|**int**|Идентификатор предиката в этой политике безопасности.|  
|target_object_id|**int**|Идентификатор объекта, к которому привязан этот предикат безопасности.|  
|predicate_definition|**nvarchar(max)**|Полное имя функции, которая будет использоваться в качестве предиката безопасности, включая аргументы. Обратите внимание, что имя `schema.function` может быть нормализовано (т. е. экранировано), как и любой другой элемент в тексте, для обеспечения согласованности. Например:<br /><br /> `[dbo].[fn_securitypredicate]([wing], [startTime], [endTime])`|  
|predicate_type|**int**|Тип предиката, используемый политикой безопасности:<br /><br /> 0 = ПРЕДИКАТ ФИЛЬТРА<br /><br /> 1 = ПРЕДИКАТ БЛОКИРОВКИ|  
|predicate_type_desc|**nvarchar(60)**|Тип предиката, используемый политикой безопасности:<br /><br /> FILTER<br /><br /> ЗАБЛОКИРОВАТЬ|  
|Операция|**int**|Тип операции, указанной для предиката:<br /><br /> NULL = все применимые операции<br /><br /> 1 = ПОСЛЕ ВСТАВКИ<br /><br /> 2 = ПОСЛЕ ОБНОВЛЕНИЯ<br /><br /> 3 = ПЕРЕД ОБНОВЛЕНИЕМ<br /><br /> 4 = ПЕРЕД УДАЛЕНИЕМ|  
|operation_desc|**nvarchar(60)**|Тип операции, указанной для предиката:<br /><br /> NULL<br /><br /> ПОСЛЕ ВСТАВКИ<br /><br /> AFTER UPDATE<br /><br /> ПЕРЕД ОБНОВЛЕНИЕМ<br /><br /> ПЕРЕД УДАЛЕНИЕМ|  
  
## <a name="permissions"></a>Разрешения  
 Участники с разрешением **ALTER ANY Security Policy** имеют доступ ко всем объектам в этом представлении каталога, а также любому пользователю с **определением представления** для объекта.  
  
## <a name="see-also"></a>См. также:  
 [Безопасность на уровне строк](../../relational-databases/security/row-level-security.md)   
 [sys.security_policies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-security-policies-transact-sql.md)   
 [CREATE SECURITY POLICY (Transact-SQL)](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Участники (компонент Database Engine)](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

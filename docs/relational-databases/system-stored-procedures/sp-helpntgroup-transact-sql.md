---
description: sp_helpntgroup (Transact-SQL)
title: sp_helpntgroup (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpntgroup
- sp_helpntgroup_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpntgroup
ms.assetid: 02b4f7c1-480a-436c-8bae-7a2488be45d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc378f5c07c4aa24c37c0212fd9f84fb6ef39f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210875"
---
# <a name="sp_helpntgroup-transact-sql"></a>sp_helpntgroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Выводит сведения о группах Windows, имеющих учетные записи в текущей базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helpntgroup [ [ @ntname= ] 'name' ]   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @ntname = ] 'name'` — Имя группы Windows. Аргумент *Name* имеет тип **sysname** и значение по умолчанию NULL. *имя* должно быть допустимой группой Windows с доступом к текущей базе данных. Если параметр *Name* не указан, в выходные данные включаются все группы Windows с доступом к текущей базе данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**NTGroupName**|**sysname**|Имя группы Windows.|  
|**NTGroupId**|**smallint**|Идентификатор группы (ID).|  
|**SID**|**varbinary(85)**|Идентификатор безопасности (SID) **нтграупнаме**.|  
|**хасдбакцесс**|**int**|1 = группа Windows имеет разрешение на доступ к базе данных.|  
  
## <a name="remarks"></a>Замечания  
 Чтобы просмотреть список [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ролей в текущей базе данных, используйте **sp_helprole**.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере выводится список групп Windows, имеющих доступ к текущей базе данных.  
  
```  
EXEC sp_helpntgroup;  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_helprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_revokedbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

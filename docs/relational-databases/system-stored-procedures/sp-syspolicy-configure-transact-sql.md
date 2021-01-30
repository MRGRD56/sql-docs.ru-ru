---
description: sp_syspolicy_configure (Transact-SQL)
title: sp_syspolicy_configure (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_configure
- sp_syspolicy_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_configure
ms.assetid: 70c10922-9345-4190-ba69-808a43f760da
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: abf343b9230e8eeef0ce95ca8e2667c20c714cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201245"
---
# <a name="sp_syspolicy_configure-transact-sql"></a>sp_syspolicy_configure (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Настраивает параметры управления на основе политик, такие как параметр включения управления на основе политик.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_syspolicy_configure [ @name = ] 'name'  
    , [ @value = ] value  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @name = ] 'name'` — Это имя параметра, который необходимо настроить. Аргумент *Name* имеет тип **sysname**, является обязательным и не может быть пустой СТРОКОЙ или иметь значение null.  
  
 *имя* может иметь любое из следующих значений:  
  
-   Enabled — определяет, включено ли управление на основе политик.  
  
-   HistoryRetentionInDays — указывает число дней, в течение которых хранится журнал выполнения политик. Если это значение равно 0, то журнал не удаляется автоматически.  
  
-   LogOnSuccess — указывает, заносится ли успешное выполнение политик в журнал управления на основе политик.  
  
`[ @value = ] value` Значение, связанное с указанным значением для *Name*. *значение* равно **sql_variant** и является обязательным.  
  
-   Если для параметра *имя* указано значение Enabled, можно использовать любое из следующих значений.  
  
    -   0 = отключает управление на основе политик;  
  
    -   1 = включает управление на основе политик.  
  
-   Если для параметра *имя* указать значение "хисторирентентиониндайс", укажите число дней в виде целого числа.  
  
-   Если для *Name* задано значение "логонсукцесс", можно использовать любое из следующих значений:  
  
    -   0 = в журнал заносится только неуспешное выполнение политики;  
  
    -   1 = в журнал заносится и успешное, и неуспешное выполнение политики.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 Процедура sp_syspolicy_configure должна выполняться в контексте системной базы данных msdb.  
  
 Чтобы просмотреть текущие значения этих параметров, запросите системное представление msdb.dbo.syspolicy_configuration.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в предопределенной роли базы данных PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Возможное повышение уровня учетных данных. пользователи в роли PolicyAdministratorRole могут создавать серверные триггеры и планировать выполнение политик, которые могут повлиять на работу экземпляра [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Например, пользователи в роли PolicyAdministratorRole могут создать политику, которая может запретить создание большинства объектов в компоненте [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Вследствие возможного повышения прав учетных данных роль PolicyAdministratorRole должна предоставляться только пользователям, имеющим право изменять конфигурацию компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Примеры  
 В следующем примере включается управление на основе политик.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'Enabled'  
, @value = 1;  
  
GO  
```  
  
 В следующем примере устанавливается 14-дневный срок хранения журнала политик.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'HistoryRetentionInDays'  
, @value = 14;  
  
GO  
```  
  
 В следующем примере для управления на основе политик настраивается регистрация и успешного, и неуспешного выполнения политик.  
  
```  
EXEC msdb.dbo.sp_syspolicy_configure @name = N'LogOnSuccess'  
, @value = 1;  
  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры управления на основе политик &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_set_config_enabled &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-enabled-transact-sql.md)   
 [sp_syspolicy_set_config_history_retention &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-config-history-retention-transact-sql.md)   
 [sp_syspolicy_set_log_on_success &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-set-log-on-success-transact-sql.md)  
  
  

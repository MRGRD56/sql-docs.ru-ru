---
description: sp_replication_agent_checkup (Transact-SQL)
title: sp_replication_agent_checkup (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replication_agent_checkup_TSQL
- sp_replication_agent_checkup
helpviewer_keywords:
- sp_replication_agent_checkup
ms.assetid: 50357c2e-71aa-4e13-9e2e-0977a3655cc9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 77dd179aa4024f969d8e27a3d57cdc03374a4497
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204402"
---
# <a name="sp_replication_agent_checkup-transact-sql"></a>sp_replication_agent_checkup (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Проверяет каждую базу данных распространителя на наличие агентов репликации, которые выполняются, но не записывали сведения в журнал в пределах указанного тактового импульса. Эта хранимая процедура выполняется на распространителе в любой базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_replication_agent_checkup [ [ @heartbeat_interval = ] heartbeat_interval ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @heartbeat_interval = ] 'heartbeat_interval'` Максимальное количество минут, в течение которых агент может пройти без записи сообщения о ходе выполнения. *heartbeat_interval* имеет **тип int** и значение по умолчанию 10 минут.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **sp_replication_agent_checkup** вызывает ошибку 14151 для каждого агента, который обнаруживается как подозрительное. Она также записывает сообщение об агентах в журнал ошибок.  
  
## <a name="remarks"></a>Замечания  
 **sp_replication_agent_checkup** используется в репликации моментальных снимков, репликации транзакций и репликации слиянием.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_replication_agent_checkup**.  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

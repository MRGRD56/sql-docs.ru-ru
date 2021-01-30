---
description: sp_dropanonymousagent (Transact-SQL)
title: sp_dropanonymousagent (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropanonymousagent
- sp_dropanonymousagent_TSQL
helpviewer_keywords:
- sp_dropanonymousagent
ms.assetid: 4cb96efa-9358-44a3-a8ee-a7e181bed089
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: b75f8b4bc355fd71c469ee2c0593f56feaf9148a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200718"
---
# <a name="sp_dropanonymousagent-transact-sql"></a>sp_dropanonymousagent (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет анонимный агент монитора репликаций на распространителе от издателя. Эта хранимая процедура выполняется на подписчике в любой базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sp_dropanonymousagent [ @subid= ] sub_id    , [ @type= ] type  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @subid = ] sub_id` Глобальный идентификатор анонимной подписки. *sub_id* имеет тип **uniqueidentifier** и не имеет значения по умолчанию. Этот идентификатор может быть получен на подписчике с помощью **sp_helppullsubscription**. Значение в поле **subid** возвращенного результирующего набора — это глобальный идентификатор.  
  
`[ @type = ] type` Тип подписки. *Type имеет тип* **int** и не имеет значения по умолчанию. Допустимые значения: **1** или **2**. Укажите значение **1**, если репликация моментальных снимков или репликация транзакций используют агент распространения. Укажите значение **2**, если репликация слиянием использует агент слияния.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_dropanonymousagent** используется во всех типах репликации.  
  
 Эта хранимая процедура используется только для удаления агентов анонимных подписок и не может быть использована для удаления известных подписок.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли базы данных **db_owner** в базе данных распространителя могут выполнять **sp_dropanonymousagent**.  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

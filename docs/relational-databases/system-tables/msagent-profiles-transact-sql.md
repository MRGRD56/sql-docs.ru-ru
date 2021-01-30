---
description: MSagent_profiles (Transact-SQL)
title: MSagent_profiles (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSagent_profiles
- MSagent_profiles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSagent_profiles system table
ms.assetid: 4ab1b2ae-b6d9-42b7-9b31-98547dbb7f99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1ac49f809e023dfe08d289f746197f23d181c0a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160515"
---
# <a name="msagent_profiles-transact-sql"></a>MSagent_profiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSagent_profiles** содержит по одной строке для каждого определенного профиля агента репликации. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**profile_id**|**int**|Идентификатор профиля.|  
|**profile_name**|**sysname**|Уникальное имя профиля для типа агента.|  
|**agent_type**|**int**|Тип агента:<br /><br /> **1** = агент моментальных снимков<br /><br /> **2** = агент чтения журнала<br /><br /> **3** = агент распространения<br /><br /> **4** = агент слияния<br /><br /> **9** = агент чтения очереди|  
|**type**|**int**|Тип профиля:<br /><br /> **0** = система **1** = настраиваемая|  
|**description**|**nvarchar (3000)**|Описание профиля.|  
|**def_profile**|**bit**|Указывает на использование профиля по умолчанию для данного типа агента.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

---
description: MSSQL_ENG014121
title: MSSQL_ENG014121 | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014121 error
ms.assetid: c8595854-cce1-4566-ad64-d565555caded
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: cfec1cfbc231ad4a347bd6d674bbe88c812eb9e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198791"
---
# <a name="mssql_eng014121"></a>MSSQL_ENG014121
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Сведения о сообщении  
  
|attribute|Значение|  
|-|-|  
|Название продукта|SQL Server|  
|Идентификатор события|14121|  
|Источник события|MSSQLSERVER|  
|Компонент|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Символическое имя||  
|Текст сообщения|Не удалось удалить распространитель "%s". С этим распространителем связаны базы данных распространителя.|  
  
## <a name="explanation"></a>Объяснение  
 Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , настроенный как распространитель, не может быть удален из роли распространителя, т. к. имеются базы данных распространителя, связанные с этим экземпляром. Эта ошибка возникает при попытке удалить базу данных распространителя, связанную с одним или несколькими издателями.  
  
## <a name="user-action"></a>Действие пользователя  
 Чтобы найти имена издателей и баз данных, связанных с конкретным распространителем, выполните хранимую процедуру [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md) из любой базы данных в этом распространителе.  
  
 Выполните хранимую процедуру [sp_dropdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md) для всех баз данных, связанных с этим распространителем. После того, как будут удалены все связи баз данных распространителя, распространение можно будет отключить.  
  
## <a name="see-also"></a>См. также:  
 [Справочник по ошибкам и событиям (репликация)](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Настройка распространения](../../relational-databases/replication/configure-distribution.md)  
  
  

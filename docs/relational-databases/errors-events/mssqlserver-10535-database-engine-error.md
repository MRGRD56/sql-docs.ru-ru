---
description: MSSQLSERVER_10535
title: MSSQLSERVER_10535 | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10535 (Database Engine error)
ms.assetid: 478fd978-11d9-4155-8329-f599fdbec14b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 430773e5dcd0d96b8b22359f07fc1ec1a39ffedf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197380"
---
# <a name="mssqlserver_10535"></a>MSSQLSERVER_10535
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|SQL Server|  
|Идентификатор события|10535|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|PG_NO_PLAN|  
|Текст сообщения|Не удается создать структуру плана «%.*ls», поскольку план не найден в кэше планов, соответствующем указанному дескриптору плана. Укажите дескриптор плана в кэше. Список дескрипторов планов, находящихся в кэше, можно получить с помощью запроса к динамическому административному представлению sys.dm_exec_query_stats.|  
  
## <a name="explanation"></a>Объяснение  
План не был найден в кэше планов, который соответствует указанному дескриптору плана.  
  
## <a name="user-action"></a>Действие пользователя  
Укажите дескриптор плана в кэше. Список дескрипторов планов, находящихся в кэше, можно получить с помощью запроса к динамическому административному представлению sys.dm_exec_query_stats.  
  
## <a name="see-also"></a>См. также:  
[sp_create_plan_guide_from_handle (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
[sys.dm_exec_query_stats (Transact-SQL)](~/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  

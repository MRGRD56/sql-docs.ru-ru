---
title: Обратная совместимость репликации | Документация Майкрософт
description: Изучите эти ресурсы для обеспечения обратной совместимости в репликации перед обновлением или при наличии нескольких версий SQL Server в топологии репликации.
ms.custom: ''
ms.date: 03/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, backward compatibility
- backward compatibility [SQL Server replication]
- merge replication backward compatibility [SQL Server replication]
- replication [SQL Server], backward compatibility
- backward compatibility [SQL Server], replication
- snapshot replication [SQL Server], backward compatibility
- compatibility [SQL Server replication]
ms.assetid: 091c51dc-8b32-4b4f-847e-b317456c8394
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a48ca05acd680cfd89f4438a64cac83c5d00dd0f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468825"
---
# <a name="replication-backward-compatibility"></a>Обратная совместимость репликации
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

При обновлении или наличии нескольких версий SQL Server в топологии репликации важно понимать принцип обратной совместимости. 

Общие правила 

-   Версия распространителя не должна быть ниже версии издателя (во многих случаях распространителем и издателем является один и тот же экземпляр).    
-   Версия издателя не должна превышать версию распространителя.    
-   Версия подписчика зависит от типа публикации следующим образом.    
    - Подписчик на публикацию транзакций может иметь любую версию, отличающуюся от версии издателя, но не более, чем на две версии. Например: издатель SQL Server 2012 (11.x) может иметь подписчиков SQL Server 2014 (12.x) и SQL Server 2016 (13.x), а издатель SQL Server 2016 (13.x) — подписчиков SQL Server 2014 (12.x) и SQL Server 2012 (11.x).     
    - Подписчик на публикацию слиянием может иметь любую версию, которая не превышает версию издателя и поддерживается в соответствии с циклом поддержки жизненного цикла версий.  


## <a name="replication-matrix"></a>Матрица репликации
[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]


## <a name="additional-resources"></a>Дополнительные ресурсы
 [Устаревшие функции репликации в SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)  
 Возможности репликации, которые были сохранены в [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] в целях обратной совместимости, но которые будут удалены в будущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Критические изменения репликации в SQL Server](../../relational-databases/replication/breaking-changes-in-sql-server-replication.md)  
 Изменения функциональных возможностей репликации, которые могут потребовать изменений в приложениях. 

 [Обновление реплицируемых баз данных](../../database-engine/install-windows/upgrade-replicated-databases.md)  
 Инструкции и замечания по обновлению серверов SQL Server, участвующих в топологии репликации. 
  
  

---
title: Создание резервных копий реплицируемых баз данных и восстановление из них | Документация Майкрософт
description: Изучите вводные сведения и ссылки на дополнительные сведения о стратегиях резервного копирования и восстановления для различных типов репликации в SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- backups [SQL Server replication]
- administering replication, restoring
- backing up replicated databases
- backups [SQL Server replication], about backups
- restoring replicated databases [SQL Server replication]
- recovery [SQL Server replication], about recovery
- restoring databases [SQL Server], replicated databases
- backing up databases [SQL Server], replicated databases
- restoring [SQL Server replication], about restoring
- recovery [SQL Server replication]
- replication [SQL Server], administering
- distribution databases [SQL Server replication], backing up
- restoring [SQL Server replication]
- administering replication, backing up
ms.assetid: 04588807-21e7-4bbe-9727-b72f692cffa7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a5b27549630a5ac835e4e9b32f8ebcde5ac6e5d0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469125"
---
# <a name="back-up-and-restore-replicated-databases"></a>Создание резервных копий реплицируемых баз данных и восстановление из них
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  Реплицированные базы данных требуют особого внимания к резервному копированию и восстановлению данных. В данном разделе содержится вводные сведения и ссылки на дополнительные сведения о стратегиях резервного копирования и восстановления для различных типов репликации.  
  
 Служба репликации поддерживает восстановление реплицированной базы данных на том же сервере и в той же базе данных, где была создана ее резервная копия. Если восстановить резервную копию реплицированной базы данных на другом сервере или в другой базе данных, то станет невозможным сохранение настроек репликации. В этом случае после восстановления из резервной копии потребуется повторно создать все публикации и подписки.  
  
> [!NOTE]  
>  Реплицируемую базу данных можно восстановить на резервном сервере при использовании доставки журналов. Дополнительные сведения см. в статье [Репликация и доставка журналов (SQL Server)](../../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
 Необходимо регулярно выполнять резервное копирование реплицируемых баз данных и связанных с ними системных баз данных. Выполняйте резервное копирование следующих баз данных:  
  
-   База данных публикаций на издателе.  
  
-   База данных распространителя на распространителе.  
  
-   База данных подписок на каждом подписчике.  
  
-   Системные базы данных **master** и **msdb** на издателе, распространителе и на всех подписчиках. Резервные копии этих баз данных и копия соответствующей базы данных репликации должны быть сделаны одновременно. Например, создавайте резервную копию баз данных **master** и **msdb** на издателе одновременно с резервной копией базы данных публикаций. Если база данных публикаций восстановлена, убедитесь, что базы данных **master** и **msdb** согласованы с базой данных публикаций по настройке и конфигурации репликации.  
  
 Если резервное копирование журналов выполняется регулярно, любые изменения, касающиеся репликации, будут заноситься в резервные копии журнала. Если не выполняется резервное копирование журналов, то необходимо выполнить это резервное копирование при каждом изменении настройки, относящейся к репликации. Дополнительные сведения см. в статье [Common Actions Requiring an Updated Backup](../../../relational-databases/replication/administration/common-actions-requiring-an-updated-backup.md).  
  
## <a name="backup-and-restore-strategies"></a>Стратегии резервного копирования и восстановления  
 Стратегии для резервного копирования и восстановления каждого узла в топологии репликации различаются в соответствии с используемым типом репликации. Сведения о стратегиях резервного копирования и восстановления для каждого типа репликации см. в следующих разделах:  
  
-   [Стратегии резервного копирования и восстановления для моментальных снимков и репликации транзакций](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md)  
  
-   [Стратегии резервного копирования и восстановления для репликации слиянием](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-merge-replication.md)  
  
 В состав любой стратегии восстановления всегда должно входить сохранение текущего скрипта настроек репликации в безопасном расположении. В случае отказа сервера или необходимости создания тестовой среды можно изменить скрипт, изменив ссылки имен серверов, после чего он может использоваться для восстановления настроек репликации. В дополнение к созданию скрипта текущих настроек репликации нужно создать скрипт включения и отключения репликации. Сведения о создании скриптов для объектов репликации см. в разделе [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
## <a name="see-also"></a>См. также:  
 [Резервное копирование и восстановление баз данных SQL Server](../../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Best Practices for Replication Administration](../../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  

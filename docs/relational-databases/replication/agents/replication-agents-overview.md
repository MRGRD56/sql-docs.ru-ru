---
title: Обзор агентов репликации | Документация Майкрософт
description: Познакомьтесь с агентами, которые репликация SQL Server использует для выполнения задач, связанных с отслеживанием изменений и распространением данных.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Distribution Agent
- agents [SQL Server replication]
- Queue Reader Agent, about Queue Reader Agent
- Queue Reader Agent
- Merge Agent, about Merge Agent
- Log Reader Agent, about Log Reader Agent
- replication [SQL Server], agents and profiles
- Log Reader Agent
- Distribution Agent, about Distribution Agent
- agents [SQL Server replication], about agents
- Merge Agent
- Snapshot Agent, about Snapshot Agent
- Snapshot Agent
ms.assetid: a35ecd7d-f130-483c-87e3-ddc8927bb91b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 5181d54b5df6463405f689bc102ddf56336ae602
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467325"
---
# <a name="replication-agents-overview"></a>Обзор агентов репликации
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Репликация использует ряд отдельных программ, называемых агентами, для выполнения задач, связанных с отслеживанием изменений и распространением данных. По умолчанию агенты репликации выполняются как задания, запланированные агентом [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , причем для выполнения заданий агент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] должен быть запущен. Агенты репликации могут запускаться из командной строки или приложениями, которые используют объекты RMO (Replication Management Objects). Агенты репликации управляются из монитора репликации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и из [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
## <a name="sql-server-agent"></a>Агент SQL Server  
 Агент[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] служит для размещения и планирования работы агентов, используемых в репликации, а также предоставляет простой способ запуска агентов репликации. Агент[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] также управляет операциями за пределами репликации и осуществляет наблюдение за выполняемыми операциями. Дополнительные сведения см. в статье [Configure SQL Server Agent](../../../ssms/agent/configure-sql-server-agent.md).  
  
> [!IMPORTANT]  
>  Служба агента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию отключается при установке [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , если только во время установки не будет явно выбран режим автоматического запуска. Дополнительные сведения о запуске службы агента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] см. в разделе [Start, Stop, or Pause the SQL Server Agent Service](../../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md).  
  
## <a name="snapshot-agent"></a>агент моментальных снимков  
 Агент моментальных снимков используется, как правило, со всеми типами репликаций. Он готовит схему и файлы исходных данных опубликованных таблиц и другие объекты, хранит файлы моментальных снимков и записывает сведения о синхронизации в базе данных распространителя. Агент моментальных снимков выполняется на распространителе. Дополнительные сведения см. в статье [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md).  
  
## <a name="log-reader-agent"></a>Агент чтения журнала.  
 Агент чтения журнала используется с репликацией транзакций. Он перемещает транзакции, помеченные для репликации, из журнала транзакций на издатель в базу данных распространителя. Каждая база данных, публикуемая с использованием репликации транзакций, имеет свой собственный агент чтения журнала, который выполняется на распространителе и подключен к издателю (распространитель может находиться на том же компьютере, что и издатель). Дополнительные сведения см. в статье [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md).  
  
## <a name="distribution-agent"></a>Агент распространителя  
 Агент распространителя используется с репликацией моментальных снимков и репликацией транзакций. Он применяет исходный моментальный снимок к подписчику и перемещает транзакции, удерживаемые в базе данных распространителя, на подписчики. Агент распространителя выполняется либо на распространителе для принудительных подписок, либо на подписчике для подписок по запросу. Дополнительные сведения см. в статье [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
## <a name="merge-agent"></a>Агент слияния.  
 Агент слияния используется с репликацией слияния. Он применяет к подписчику исходный моментальный снимок, а также перемещает и согласовывает возникающие дополнительные изменения данных. Каждая подписка на публикацию слиянием имеет свой собственный агент слияния, который подключается как к издателю, так и к подписчику и обновляет и тот, и другой. Агент слияния выполняется либо на распространителе для принудительных подписок, либо на подписчике для подписок по запросу. По умолчанию агент слияния передает изменения с подписчика издателю, затем загружает изменения с издателя в подписчик. Дополнительные сведения см. в статье [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md).  
  
## <a name="queue-reader-agent"></a>Агент чтения очереди.  
 Агент чтения очереди используется для репликации транзакций с параметром обновления посредством очередей. Агент выполняется на распространителе и перемещает изменения, сделанные на подписчике, обратно в издатель. В отличие от агента распространителя и агента слияния существует только один экземпляр агента чтения очереди, который обслуживает все издатели и публикации для некоторой заданной базы данных распространителя. Дополнительные сведения об агенте чтения очереди см. в разделе [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md). Дополнительные сведения об обновляемых подписках см. в разделе [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="replication-maintenance-jobs"></a>Задания обслуживания репликации  
 Репликация имеет ряд заданий обслуживания, которые исполняют запланированное и выполняемое по запросу обслуживание. Дополнительные сведения см. в статье [Администрирование агента репликации](../../../relational-databases/replication/agents/replication-agent-administration.md).  
  
## <a name="see-also"></a>См. также:  
 [Запуск и остановка агента репликации (среда SQL Server Management Studio)](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [Запуск задания по обслуживанию репликаций (среда SQL Server Management Studio)](../../../relational-databases/replication/administration/run-replication-maintenance-jobs-sql-server-management-studio.md)   
 [Основные понятия исполняемых файлов агента репликации](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)   
 [Администрирование агента репликации](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  

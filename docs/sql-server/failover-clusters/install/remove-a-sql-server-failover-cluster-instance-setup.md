---
title: Удаление экземпляра отказоустойчивого кластера
description: Эта процедура используется для удаления экземпляра отказоустойчивого кластера Always On. В этой статье приводятся важные рекомендации, которые необходимо выполнить, прежде чем продолжать.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- clusters [SQL Server], removing failover cluster instance
- failover clustering [SQL Server], removing failover cluster instance
- uninstalling failover cluster instances
- removing failover cluster instances
ms.assetid: bf63353b-69cf-4c5c-98ea-7b151e36537f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7013df80c0bce5105128759f874deb2ec1fb0cca
ms.sourcegitcommit: 129f8574eba201eb6ade1f1620c6b80dfe63b331
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2020
ms.locfileid: "87435456"
---
# <a name="remove-a-failover-cluster-instance-setup"></a>Удаление экземпляра отказоустойчивого кластера (настройка)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Эта процедура используется для удаления экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On.  
  
> [!IMPORTANT]  
>  Чтобы обновить или удалить экземпляр отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], необходимо иметь разрешения локального администратора для входа в систему в качестве службы на все узлы отказоустойчивого кластера Windows Server.  
  
 **Before you begin**  
  
 Учтите следующие важные моменты перед удалением экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Если собственный клиент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] случайно удален, не смогут запуститься ресурсы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Чтобы переустановить собственный клиент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , запустите программу установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и установите предварительно необходимые компоненты [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   При удалении отказоустойчивого кластера, обладающего более чем одним кластерным ресурсом SQL IP, необходимо удалить дополнительные ресурсы SQL IP при помощи диспетчера отказоустойчивости кластеров или PowerShell.  
  
 Сведения о синтаксисе командной строки см. в разделе [Установка SQL Server 2016 из командной строки](../../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md).  
  
### <a name="to-uninstall-a-ssnoversion-failover-cluster-instance"></a>Удаление экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]
  
1.  Для удаления отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] используется функция удаления узла, предоставляемая программой установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , которая удаляет каждый узел по отдельности. Дополнительные сведения см. на странице [Добавление и удаление узлов в отказоустойчивом кластере Always On &#40;настройка&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  
  
## <a name="see-also"></a>См. также:  
 [Просмотр и чтение файлов журналов программы установки SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  

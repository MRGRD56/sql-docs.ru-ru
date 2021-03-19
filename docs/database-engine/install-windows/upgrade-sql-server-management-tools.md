---
title: Обновление средств управления SQL Server | Документы Майкрософт
description: В этой статье показано, как обновляются средства управления SQL Server и компоненты управления, например агент SQL Server.
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- management tools, upgrading
ms.assetid: 1dab50b9-d16c-49a1-9ecc-af72adb6c378
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 9b6d87b1b74e765dda1c03e6764a4d6e0ccfc097
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610912"
---
# <a name="upgrade-sql-server-management-tools"></a>Обновление средств управления SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] поддерживает обновление с версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] или более поздней. В этой статье описаны средства и процесс обновления средств управления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и компонентов управления, таких как агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], компонент Database Mail, планы обслуживания, XPStar и XPWeb.  
  
> [!IMPORTANT]  
>  Для локальных установок программу установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] нужно запускать с правами администратора. При установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из удаленной общей папки необходимо пользоваться учетной записью домена, имеющей разрешения на чтение и выполнение для удаленной общей папки.  
  
## <a name="known-upgrade-issues"></a>Известные проблемы при обновлении  
Прежде чем приступать к обновлению до [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], необходимо учесть следующие моменты.  
  
### <a name="for-all-upgrade-scenarios"></a>Для всех сценариев обновления  
  
- Перед обновлением главного сервера должны быть обновлены все целевые серверы. Дополнительные сведения о главных и целевых серверах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]см. в статье [Автоматизация администрирования в масштабах предприятия](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
-   Все компоненты экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должны быть обновлены одновременно. Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)], а также службы [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]и [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] должны иметь один и тот же номер версии для одного экземпляра [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
-   При обновлении до [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] к существующей установке [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]могут быть добавлены новые компоненты. Дополнительные сведения см. в статье [Обновление SQL Server 2016 с помощью мастера установки (программа установки)](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , такие как [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], помощник по настройке компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , sqlcmd и osql, до версии [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Клиентские средства запускаются параллельно с компонентами предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] поддерживает импорт параметров из предыдущих версий клиентских средств [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Проверка подлинности из агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет переключена из режима [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в режим Windows, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
-   После обновления до [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]данные заданий и предупреждений сохранятся.  
  
-   Если в обновляемом экземпляре используется программа SQLMail, то связанные с ней расширенные хранимые процедуры после обновления будут сохранены и включены. В противном случае они будут отключены.  
  
-   При обновлении [!INCLUDE[ssDE](../../includes/ssde-md.md)] компонент Database Mail (также называемый SQLiMail) обновляется одновременно с компонентом [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. По умолчанию после обновления компонент Database Mail отключается. Все изменения схемы после этого должны быть согласованы со скриптом обновления.  
  
## <a name="see-also"></a>См. также:  
 [Поддерживаемые обновления версий и выпусков](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [Обратная совместимость_удалено](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))   
 [Обновление SQL Server с помощью мастера установки (программа установки)](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  

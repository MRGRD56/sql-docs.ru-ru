---
description: Выбор учетной записи для службы агента SQL Server
title: Выбор учетной записи для службы агента SQL Server
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- roles [SQL Server], SQL Server Agent
- SQL Server Agent, accounts
- startup accounts [SQL Server]
- SQL Server Agent service, accounts
- accounts [SQL Server], SQL Server Agent
- Windows groups [SQL Server Agent]
- SQL Server Agent, permissions
- members [SQL Server], SQL Server Agent service
- Windows domain accounts [SQL Server]
- security [SQL Server], SQL Server Agent
ms.assetid: fe658e32-9e6b-4147-a189-7adc3bd28fe7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 05/04/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 6267141d71215834c6e25f9bb67581206e515e49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351710"
---
# <a name="select-an-account-for-the-sql-server-agent-service"></a>Выбор учетной записи для службы агента SQL Server

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Стартовая учетная запись службы определяет учетную запись [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows, с которой запускается агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , а также его сетевые разрешения. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется как заданная учетная запись пользователя. Диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет выбрать учетную запись службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из следующих вариантов:  
  
-   **Встроенная учетная запись**. Может быть выбрана из списка следующих встроенных учетных записей Windows:  
  
    -   Учетная запись **Локальная система** . Имя этой учетной записи — NT AUTHORITY\System. Эта учетная запись имеет неограниченный доступ ко всем локальным системным ресурсам. Она входит в группу **Администраторы** на локальном компьютере Windows и поэтому является членом предопределенной роли сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysadmin**.  
  
        > [!IMPORTANT]  
        > Параметр **С системной учетной записью** поддерживается для обратной совместимости. Локальная системная учетная запись обладает разрешениями, которые не нужны для работы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Старайтесь не запускать агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] от имени учетной записи локальной системы. В целях безопасности рекомендуется пользоваться учетной записью домена Windows с разрешениями, перечисленными в подразделе «Разрешения учетной записи домена Windows» ниже в этом разделе.  
  
-   **Указанная учетная запись**. Позволяет задать учетную запись домена Windows, с которой выполняется служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Рекомендуется выбирать учетную запись пользователя Windows, не входящего в группу **Администраторы** . Однако существуют ограничения при администрировании нескольких серверов, когда учетная запись службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не входит в локальную группу **Администраторы** . Дополнительные сведения см. в подразделе «Поддерживаемые типы учетных записей» далее в этом разделе.  
  
## <a name="windows-domain-account-permissions"></a>Разрешения учетной записи домена Windows  
В целях повышения безопасности выбирайте пункт **Указанная учетная запись**, соответствующий учетной записи домена Windows. Заданная учетная запись домена Windows должна обладать следующими разрешениями:  
  
-   Разрешение на вход в систему в качестве службы во всех версиях Windows (SeServiceLogonRight)  
  
> [!NOTE]  
> Служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должна быть членом группы «Доступ, совместимый с версиями Windows до 2000» контроллера домена. В противном случае задания, принадлежащие пользователям домена, которые не являются администраторами Windows, выполняться не будут.  
  
-   На серверах Windows учетной записи, с которой выполняется служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , для поддержки посредников агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимы следующие разрешения:  
  
    -   Разрешение на обход перекрестной проверки (SeChangeNotifyPrivilege)  
  
    -   Разрешение на замену токена уровня процесса (SeAssignPrimaryTokenPrivilege)  
  
    -   Разрешение на выделение процессам квот памяти (SeIncreaseQuotaPrivilege)  
  
    -   Разрешение на доступ к этому компьютеру из сети (SeNetworkLogonRight)  
  
> [!NOTE]  
> Если учетная запись не обладает разрешениями на поддержку посредников, то создавать задания могут только члены предопределенной роли сервера **sysadmin** .  
  
> [!NOTE]  
> Чтобы получать уведомление о предупреждении инструментария WMI, учетной записи службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должно быть предоставлено разрешение на пространство имен, содержащее события WMI и ALTER ANY EVENT NOTIFICATION.  
  
## <a name="sql-server-role-membership"></a>Членство в ролях SQL Server  
Учетная запись, от которой запускается служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , должна быть членом следующих ролей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Учетная запись должна быть членом предопределенной роли сервера **sysadmin** .  
  
-   Чтобы использовать обработку заданий в многосерверной среде, учетная запись должна быть членом роли базы данных **msdb****TargetServersRole** на главном сервере.  
  
## <a name="supported-service-account-types"></a>Поддерживаемые типы учетных записей  
В следующей таблице перечислены типы учетных записей Windows, которые могут быть использованы для службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Тип учетной записи|Некластеризованный сервер|Кластеризованный сервер|Контроллер домена (некластеризованный)|  
|------------------------|-------------------------|--------------------|--------------------------------------|  
|[!INCLUDE[msCoName](../../includes/msconame_md.md)] Учетная запись Windows (член группы "Администраторы" Windows)|Поддерживается|Поддерживается|Поддерживается|  
|Неадминистративная учетная запись домена Windows|Поддерживается<br /><br />См. ограничение № 1 ниже.|Поддерживается<br /><br />См. ограничение № 1 ниже.|Поддерживается<br /><br />См. ограничение № 1 ниже.|  
|Учетная запись сетевой службы (NT AUTHORITY\NetworkService)|Поддерживается<br /><br />См. ограничения № 1, 2 и 4 ниже.|Не поддерживается|Не поддерживается|  
|Неадминистративная учетная запись локального пользователя|Поддерживается<br /><br />См. ограничение № 1 ниже.|Не поддерживается|Неприменимо|  
|Учетная запись Local System (NT AUTHORITY\System)|Поддерживается<br /><br />См. ограничение № 2 ниже.|Не поддерживается|Поддерживается<br /><br />См. ограничение № 2 ниже.|  
|Учетная запись локальной службы (NT AUTHORITY\NetworkService)|Не поддерживается|Не поддерживается|Не поддерживается|  
  
### <a name="limitation-1-using-non-administrative-accounts-for-multiserver-administration"></a>Ограничение 1. Использование неадминистративных учетных записей для администрирования нескольких серверов  
Прикрепление целевого сервера к главному серверу может завершиться ошибкой, после чего появляется следующее сообщение: "Не удалось выполнить операцию прикрепления".  
  
Чтобы устранить эту ошибку, перезапустите [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в статье [Iniciar, parar, pausar, retomar e reiniciar os serviços SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
### <a name="limitation-2-using-the-local-system-account-for-multiserver-administration"></a>Ограничение 2. Использование учетной записи Local System для администрирования нескольких серверов  
Администрирование нескольких серверов поддерживается при выполнении службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] под учетной записью Local System только в том случае, если целевой и главный серверы расположены на одном и том же компьютере. При использовании этой конфигурации, при прикреплении целевого сервера к главному серверу, возвращается следующее сообщение:  
  
"Убедитесь, что стартовая учетная запись агента для *<имя_компьютера_целевого_сервера>* имеет права для входа на сервер targetServer".  
  
Данное сообщение можно пропустить. Операция прикрепления должна быть завершена успешно. Дополнительные сведения см. в статье [Создание многосерверной среды](../../ssms/agent/create-a-multiserver-environment.md).  
  
### <a name="limitation-3-using-the-network-service-account-when-it-is-a-sql-server-user"></a>Ограничение 3. Использование учетной записи сетевой службы, которая является учетной записью SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] При запуске агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может произойти сбой, если он запускается под учетной записью сетевой службы, которая уже явным образом получила доступ к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в качестве пользователя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Для решения этой проблемы перезагрузите компьютер, на котором работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Это действие необходимо выполнить однократно.  
  
### <a name="limitation-4-using-the-network-service-account-when-sql-server-reporting-services-is-running-on-the-same-computer"></a>Ограничение 4. Использование учетной записи сетевой службы при выполнении служб SQL Server Reporting Services на том же самом компьютере  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Агент не может быть запущен, если служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется под учетной записью сетевой службы, а службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] запущены на этом же самом компьютере.  
  
Для решения этой проблемы перезагрузите компьютер, на котором работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , а затем перезапустите службу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и службу агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Это действие необходимо выполнить однократно.  
  
## <a name="common-tasks"></a>Общие задачи  
**Указание стартовой учетной записи службы агента SQL Server**  
  
-   [Назначение стартовой учетной записи службы для агента SQL Server &#40;диспетчер конфигурации SQL Server&#41;](../../ssms/agent/set-service-startup-account-sql-server-agent-sql-server-configuration-manager.md)  
  
**Указание профиля электронной почты агента SQL Server**  
  
-   [Руководство. Настройка почты агента SQL Server на использование компонента Database Mail](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
> [!NOTE]  
> Запуск агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] во время старта операционной системы задается с помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
[Настройка учетных записей служб Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
[Управление службами с помощью SQL Computer Manager](../../database-engine/configure-windows/scm-services-connect-to-another-computer.md)  
[Обеспечение безопасности агента SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)  

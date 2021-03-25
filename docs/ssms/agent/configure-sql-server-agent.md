---
description: Configure SQL Server Agent
title: Configure SQL Server Agent
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, configuring
- accounts [SQL Server], SQL Server Agent
- SQL Server Agent, permissions
- security [SQL Server], SQL Server Agent
ms.assetid: 2e361a62-9e92-4fcd-80d7-d6960f127900
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1de20e6f195c9ad38a66eeec53f21c64cafb6cbf
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890775"
---
# <a name="configure-sql-server-agent"></a>Configure SQL Server Agent

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Включение и отключение агента SQL Server в настоящее время не поддерживается для Управляемого экземпляра SQL. Агент SQL работает всегда. Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

В этом разделе описывается задание некоторых параметров конфигурации для агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] во время установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Доступ к полному набору параметров конфигурации агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно получить с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], управляющих объектов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SMO) или хранимых процедур агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Подготовка к работе
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Ограничения
  
-   Выберите **Агент SQL Server** в обозревателе объектов [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] для администрирования заданий, операторов, оповещений и службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Однако с узлом агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в обозревателе объектов можно работать только при наличии соответствующего разрешения.
  
-   В экземплярах отказоустойчивых кластеров не должен быть включен автоматический перезапуск службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
### <a name="security"></a><a name="Security"></a>безопасность
  
#### <a name="permissions"></a><a name="Permissions"></a>Permissions
Для выполнения своих функций агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен быть настроен на использование учетных данных записи, которая является членом предопределенной роли сервера **sysadmin** в среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Эта учетная запись должна иметь следующие разрешения Windows.  
  
-   Вход в систему в качестве службы (SeServiceLogonRight)
  
-   Замена токена уровня процесса (SeAssignPrimaryTokenPrivilege)
  
-   Обход проходной проверки (SeChangeNotifyPrivilege)
  
-   Назначение квот памяти процессам (SeIncreaseQuotaPrivilege)
  
Дополнительные сведения о разрешениях Windows, необходимых для учетной записи службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , см. в разделах [Выбор учетной записи для службы агента SQL Server](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md) и [Настройка учетных записей служб Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).
  
## <a name="to-configure-sql-server-agent"></a>Настройка агента SQL Server

1. Нажмите кнопку **Пуск** и в меню **Пуск** выберите **Панель управления**.  
2. В Панели управления выберите **Система и безопасность**, **Администрирование**, **Локальная политика безопасности**.
3. В окне "Локальная политика безопасности" щелкните треугольник для развертывания папки **Локальные политики**, а затем выберите папку **Назначение прав пользователя**.
4. Щелкните правой кнопкой мыши разрешение, которое нужно настроить для использования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , и выберите пункт **Свойства**.
5. В диалоговом окне свойств разрешения убедитесь, что указана учетная запись, с которой работает агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Если это не так, нажмите **Добавить пользователя или группу**, введите учетную запись, с которой работает агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], в диалоговом окне **Выбор: "Пользователи", "Компьютеры", "Учетные записи служб" или "Группы"** , а затем нажмите кнопку **ОК**.
6. Повторите эту процедуру для каждого разрешения, которое нужно добавить для работы с агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Когда закончите, нажмите кнопку **ОК**.

---
description: Configure a User to Create and Manage SQL Server Agent Jobs
title: Configure a User to Create and Manage SQL Server Agent Jobs
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, user configuration
- jobs [SQL Server Agent], user configuration
- SQLAgentUserRole database role
- proxy accounts [SQL Server Agent]
ms.assetid: 67897e3e-b7d0-43dd-a2e2-2840ec4dd1ef
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e0aa22d8e76eaed8396291a19b6cc46a0a3651e4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355217"
---
# <a name="configure-a-user-to-create-and-manage-sql-server-agent-jobs"></a>Configure a User to Create and Manage SQL Server Agent Jobs

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

В этой статье объясняется, как настроить пользователя для создания или выполнения заданий агента [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

- **Перед началом работы**  [Безопасность](#Security)  
 
- **Настройка пользователя для создания заданий агента SQL Server и управления ими с помощью следующих средств:**  [SQL Server Management Studio](#SSMS)  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Перед началом  
  
### <a name="security"></a><a name="Security"></a>безопасность  
Чтобы разрешить пользователю создавать задания агента [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и управлять ими, необходимо сначала добавить существующее имя входа для SQL Server или роль базы данных msdb к одной из следующих предопределенных ролей базы данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в базе данных msdb: SQLAgentUserRole, SQLAgentReaderRole или SQLAgentOperatorRole.  
  
По умолчанию члены этих ролей базы данных могут создавать свои собственные шаги заданий, которые запускаются сами по себе. Если пользователи, не являющиеся администраторами, желают выполнить задания, которые выполняют другие типы шагов заданий, например пакеты [!INCLUDE[ssIS](../../includes/ssis_md.md)] , им необходимо будет получить доступ к учетной записи-посреднику. Все члены предопределенной роли сервера sysadmin имеют разрешения на создание, изменение и удаление учетных записей-посредников. Дополнительные сведения о разрешениях, связанных с этими предопределенными ролями баз данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , см. в разделе [Предопределенные роли базы данных агента SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permissions  
Дополнительные сведения см. в разделе [Обеспечение безопасности агента SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Использование среды SQL Server Management Studio  
**Добавление имени входа SQL или роли базы данных msdb к предопределенной роли базы данных агента SQL Server**  
  
1.  В **Обозревателе объектов** разверните сервер.  
  
2.  Разверните элемент **Безопасность**, а затем элемент **Имена входа**.  
  
3.  Щелкните правой кнопкой мыши имя входа, которое необходимо добавить к предопределенной роли базы данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , и выберите пункт **Свойства**.  
  
4.  На странице **Сопоставление пользователей** диалогового окна **Свойства имени входа** выберите строку, содержащую базу данных **msdb**.  
  
5.  На вкладке **Членство в роли базы данных для: msdb** выберите соответствующую предопределенную роль базы данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Настройка учетной записи-посредника для создания и управления шагами заданий агента SQL Server**  
  
1.  В **Обозревателе объектов** разверните сервер.  
  
2.  Разверните узел **Агент SQL Server**.  
  
3.  Щелкните правой кнопкой мыши элемент **Учетные записи-посредники** и выберите пункт **Создать учетную запись-посредник**.  
  
4.  На вкладке **Общие** диалогового окна **Создание учетной записи-посредника** укажите имя учетной записи-посредника, имя входа и описание. Обратите внимание, на то, что прежде чем создавать учетную запись-посредник агента SQL Server, необходимо создать учетные данные. Дополнительные сведения о создании учетных данных см. в разделах [Руководство. Создание учетных данных](../../relational-databases/security/authentication-access/create-a-credential.md) и [CREATE CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-credential-transact-sql.md).  
  
5.  Проверьте соответствующие подсистемы для этой учетной записи-посредника.
    1. [Операционная система (CmdExec)](create-a-cmdexec-job-step.md)
    1. [Запрос служб SQL Server Analysis Services](create-an-analysis-services-job-step.md#to-create-an-analysis-services-query-job-step)
    1. [Команда служб SQL Server Analysis Services](create-an-analysis-services-job-step.md#to-create-an-analysis-services-command-job-step-1)
    1. [Пакет служб SQL Server Integration Services](../../integration-services/packages/run-integration-services-ssis-packages.md)
    1. [PowerShell](../../powershell/run-windows-powershell-steps-in-sql-server-agent.md)
  
6.  На вкладке **Участники** добавьте или удалите имена входа или роли, чтобы предоставить или отменить доступ к учетной записи-посреднику.  

## <a name="see-also"></a>См. также
- [Обеспечение безопасности агента SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)
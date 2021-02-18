---
description: Изменение задания
title: Изменение задания
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], modifying
- modifying jobs
- SQL Server Agent jobs, modifying
ms.assetid: dd5e5f20-20c4-4ab9-a19a-db87577dcd43
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: ecbf1e66a5667f6461293e1ec54a662557842cc4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355177"
---
# <a name="modify-a-job"></a>Изменение задания

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

В этой статье описано, как изменять свойства заданий агента [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] или управляющих объектов SQL Server.  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Перед началом  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Ограничения  
Главное задание агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может быть ориентировано как на локальный, так и на удаленный сервер.  
  
### <a name="security"></a><a name="Security"></a>безопасность  
Если пользователь не является членом предопределенной роли сервера **sysadmin** , он может изменять только свои собственные задания. Дополнительные сведения см. в разделе [Обеспечение безопасности агента SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Использование среды SQL Server Management Studio  
  
#### <a name="to-modify-a-job"></a>Изменение задания  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]и разверните его.  
  
2.  Разверните узел **Агент SQL Server**, выберите раздел **Задания**, щелкните правой кнопкой мыши задание, которое нужно изменить, и выберите пункт **Свойства**.  
  
3.  В диалоговом окне **Свойства задания** обновите свойства, шаги, расписание, предупреждения и уведомления задания, воспользовавшись соответствующими страницами.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Использование Transact-SQL  
  
#### <a name="to-modify-a-job"></a>Изменение задания  
  
1.  В обозревателе объектов подключитесь к экземпляру компонента Database Engine и разверните его.  
  
2.  На панели инструментов нажмите кнопку **Создать запрос**.  
  
3.  В окне запроса используйте следующие системные хранимые процедуры для изменения задания.  
  
    -   Выполните процедуру [sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md) , чтобы изменить атрибуты задания.  
  
    -   Выполните процедуру [sp_update_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md) , чтобы изменить сведения о планировании для определения задания.  
  
    -   Выполните процедуру [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) , чтобы добавить новые шаги к заданию.  
  
    -   Выполните процедуру [sp_update_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md) , чтобы изменить существующие шаги задания.  
  
    -   Выполните процедуру [sp_delete_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md) , чтобы удалить шаг задания.  
  
    -   Дополнительные хранимые процедуры для изменения любого главного задания агента SQL Server.  
  
        -   Выполните процедуру [sp_delete_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md) , чтобы удалить сервер, сопоставленный с заданием.  
  
        -   Выполните процедуру [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) , чтобы сопоставить сервер с текущим заданием.  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Использование управляющих объектов SQL Server  
**Изменение задания**  
  
Воспользуйтесь классом **Job** на любом языке программирования, таком как Visual Basic, Visual C# или PowerShell. Дополнительные сведения см. в статье [Управляющие объекты SQL Server (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  

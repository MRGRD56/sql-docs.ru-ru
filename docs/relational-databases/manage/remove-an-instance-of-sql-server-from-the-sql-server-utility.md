---
title: Удаление экземпляра SQL Server из служебной программы SQL Server | Документация Майкрософт
description: Сведения об удалении экземпляра SQL Server с помощью служебной программы SQL Server. Для выполнения этой задачи можно запустить сценарий PowerShell или использовать SQL Server Management Studio.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.utility.remove.f1
ms.assetid: ae1d126a-46d2-47bf-b339-17c743df6491
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 3b316b0fdf68debf590d3946b24c6ae9c6c79ae9
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2020
ms.locfileid: "91810702"
---
# <a name="remove-an-instance-of-sql-server-from-the-sql-server-utility"></a>Удаление экземпляра SQL Server с помощью служебной программы SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Чтобы удалить управляемый экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполните следующие действия. Эта процедура используется для удаления экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из списка пункта управления программой и остановки сбора данных программой [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удален.  
  
> [!IMPORTANT]  
>  Перед тем как использовать эту процедуру для удаления экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , убедитесь, что на удаляемом экземпляре работают службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и агента SQL Server.  
  
1.  В обозревателе программы в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]щелкните узел **Управляемые экземпляры**. На панели содержимого обозревателя программы просмотрите список управляемых экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
2.  В столбце **Имя экземпляра SQL Server** списка выберите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , который требуется удалить из программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Щелкните правой кнопкой мыши удаляемый экземпляр и выберите команду **Удалить управляемый экземпляр…** .  
  
3.  Укажите учетные данные с правами администратора для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Нажмите кнопку **Подключить…** , проверьте данные в диалоговом окне **Подключение к серверу**, а затем нажмите кнопку **Подключить**. В диалоговом окне **Удаление управляемого экземпляра** появятся данные входа.  
  
4.  Чтобы подтвердить операцию, нажмите кнопку **ОК**. Чтобы прервать операцию, нажмите кнопку **Отмена**.  

## <a name="manually-remove-a-managed-instance-of-sql-server-from-a-sql-server-utility"></a>Удаление управляемого экземпляра SQL Server из программы SQL Server Utility вручную  
 Эта процедура используется для удаления экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из списка пункта управления программой и остановки сбора данных программой [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удален.  
  
 Удаление управляемого экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью PowerShell. Скрипт выполняет следующие операции.  
  
-   Возвращает пункт управления программой по имени экземпляра сервера.  
  
-   Удаляет управляемый экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```  
# Get Ucp connection  
$UcpServerInstanceName = "ComputerName\InstanceName";  
$UtilityInstance = new-object -Type Microsoft.SqlServer.Management.Smo.Server $UcpServerInstanceName;  
$UcpConnection = new-object -Type Microsoft.SqlServer.Management.Sdk.Sfc.SqlStoreConnection $UtilityInstance.ConnectionContext.SqlConnectionObject;  
$Utility = [Microsoft.SqlServer.Management.Utility.Utility]::Connect($UcpConnection);  
  
# Now remove the ManagedInstance from the SQL Server Utility  
$ServerInstanceName = "ComputerName\InstanceName";  
$Instance = new-object -Type Microsoft.SqlServer.Management.Smo.Server $ServerInstanceName;  
$InstanceConnection = new-object -Type Microsoft.SqlServer.Management.Sdk.Sfc.SqlStoreConnection $Instance.ConnectionContext.SqlConnectionObject;  
$ManagedInstance = $Utility.ManagedInstances[$ServerInstanceName];  
$ManagedInstance.Remove($InstanceConnection);  
```  
  
 Следует отметить, что важно указать точно такое же имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , какое хранится в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] учитывается регистр, имя экземпляра следует указывать точно в той форме, которую возвращает команда @@SERVERNAME. Чтобы получить имя управляемого экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните этот запрос на управляемом экземпляре:  
  
```  
select @@SERVERNAME AS instance_name  
```  
  
 В этот момент управляемый экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет полностью удален из пункта управления программой. При следующем обновлении данных для программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Utility он исчезнет из списка. Это состояние похоже на то, когда пользователь успешно выполнил операцию удаления управляемого экземпляра в пользовательском интерфейсе среды SSMS.  
  
## <a name="see-also"></a>См. также:  
 [Использование проводника служебных программ для управления служебной программой SQL Server](../../relational-databases/manage/use-utility-explorer-to-manage-the-sql-server-utility.md)   
 [Устранение неполадок служебной программы SQL Server](/previous-versions/sql/sql-server-2016/ee210592(v=sql.130))  
  

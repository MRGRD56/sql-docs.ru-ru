---
title: Загрузка сборки объектов SMO в Windows PowerShell | Документация Майкрософт
description: Узнайте, как загружать сборки управляющих объектов SQL Server (SMO) в скриптах Windows PowerShell, не использующих поставщик SQL Server для PowerShell.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 8ca42b69-da5a-47f4-9085-34e443f0e389
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 673e94da44cbdd46a8873468b4421a1a8ea4e037
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714232"
---
# <a name="load-the-smo-assemblies-in-windows-powershell"></a>Загрузка сборки объектов SMO в Windows PowerShell
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В этой статье описывается загрузка сборок управляющих объектов SQL Server (SMO) в скриптах Windows PowerShell, не использующих поставщик SQL Server для PowerShell.  
  
> [!NOTE]
> Существует два модуля SQL Server PowerShell — **SqlServer** и **SQLPS**. Модуль **SQLPS** входит в состав установки SQL Server (для обеспечения обратной совместимости), но больше не обновляется. Самым актуальным модулем PowerShell является модуль **SqlServer**. Модуль **SqlServer** содержит обновленные версии командлетов в **SQLPS**, а также новые командлеты для поддержки последних функций SQL.  
> Предыдущие версии модуля **SqlServer** *входили* в состав среды SQL Server Management Studio (SSMS), но только с SSMS версий 16.x. Для работы PowerShell с SSMS 17.0 и более поздних версий необходимо установить модуль **SqlServer** из коллекции PowerShell.
> Сведения об установке модуля **SqlServer** см. в статье [Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md).


Рекомендуемым механизмом загрузки сборок объектов SMO является загрузка модуля **SqlServer**. Поставщик [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , включенный в этот модуль, автоматически загружает сборки объектов SMO, а также реализует функции, расширяющие степень полезности объектов в скриптах PowerShell.
  
В двух следующих случаях, возможно, понадобится загрузить сборки объектов SMO напрямую.  
  
-   Если скрипт ссылается на объект SMO перед первой командой, ссылающейся на поставщика, или командлетами из оснасток [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
-   Код объектов SMO переносится из другого языка, такого как C# или Visual Basic, который не использует этот поставщик или командлеты.  
  
## <a name="example-loading-the-sql-server-management-objects"></a>Пример Загрузка управляющих объектов SQL Server  
 Следующий код загружает сборки объектов SMO:  
  
```  
#  
# Loads the SQL Server Management Objects (SMO)  
#  
  
$ErrorActionPreference = "Stop"  
  
$sqlpsreg="HKLM:\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.SqlServer.Management.PowerShell.sqlps"  
  
if (Get-ChildItem $sqlpsreg -ErrorAction "SilentlyContinue")  
{  
    throw "SQL Server Provider for Windows PowerShell is not installed."  
}  
else  
{  
    $item = Get-ItemProperty $sqlpsreg  
    $sqlpsPath = [System.IO.Path]::GetDirectoryName($item.Path)  
}  
  
$assemblylist =   
"Microsoft.SqlServer.Management.Common",  
"Microsoft.SqlServer.Smo",  
"Microsoft.SqlServer.Dmf ",  
"Microsoft.SqlServer.Instapi ",  
"Microsoft.SqlServer.SqlWmiManagement ",  
"Microsoft.SqlServer.ConnectionInfo ",  
"Microsoft.SqlServer.SmoExtended ",  
"Microsoft.SqlServer.SqlTDiagM ",  
"Microsoft.SqlServer.SString ",  
"Microsoft.SqlServer.Management.RegisteredServers ",  
"Microsoft.SqlServer.Management.Sdk.Sfc ",  
"Microsoft.SqlServer.SqlEnum ",  
"Microsoft.SqlServer.RegSvrEnum ",  
"Microsoft.SqlServer.WmiEnum ",  
"Microsoft.SqlServer.ServiceBrokerEnum ",  
"Microsoft.SqlServer.ConnectionInfoExtended ",  
"Microsoft.SqlServer.Management.Collector ",  
"Microsoft.SqlServer.Management.CollectorEnum",  
"Microsoft.SqlServer.Management.Dac",  
"Microsoft.SqlServer.Management.DacEnum",  
"Microsoft.SqlServer.Management.Utility"  
  
foreach ($asm in $assemblylist)  
{  
    $asm = [Reflection.Assembly]::LoadWithPartialName($asm)  
}  
  
Push-Location  
cd $sqlpsPath  
update-FormatData -prependpath SQLProvider.Format.ps1xml   
Pop-Location  
```  
  
## <a name="see-also"></a>См. также:  
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  

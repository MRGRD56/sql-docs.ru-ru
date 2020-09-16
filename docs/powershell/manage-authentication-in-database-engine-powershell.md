---
title: PowerShell. Управление аутентификацией
description: Узнайте, как использовать проверку подлинности SQL Server, а не проверку подлинности Windows (по умолчанию) при подключении к экземпляру ядра СУБД.
titleSuffix: SQL Server on Linux
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: ab9212a6-6628-4f08-a38c-d3156e05ddea
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cde441ebf659cfb558c21abd981a49079bcacb16
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714222"
---
# <a name="powershell-manage-authentication-to-sql-server"></a>PowerShell. Управление проверкой подлинности в SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

По умолчанию компоненты [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell используют при установлении соединения с компонентом [!INCLUDE[ssDE](../includes/ssde-md.md)]проверку подлинности Windows. Для использования проверки подлинности SQL Server необходимо либо определить виртуальный диск PowerShell, либо указать параметры **-Username** и **-Password** для **Invoke-Sqlcmd**.  
  
> [!NOTE]
> Существует два модуля SQL Server PowerShell — **SqlServer** и **SQLPS**. Модуль **SQLPS** входит в состав установки SQL Server (для обеспечения обратной совместимости), но больше не обновляется. Самым актуальным модулем PowerShell является модуль **SqlServer**. Модуль **SqlServer** содержит обновленные версии командлетов в **SQLPS**, а также новые командлеты для поддержки последних функций SQL.  
> Предыдущие версии модуля **SqlServer** *входили* в состав среды SQL Server Management Studio (SSMS), но только с SSMS версий 16.x. Для работы PowerShell с SSMS 17.0 и более поздних версий необходимо установить модуль **SqlServer** из коллекции PowerShell.
> Сведения об установке модуля **SqlServer** см. в статье [Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md).

  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Все действия, которые могут быть выполнены на экземпляре компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] , определяются разрешениями, предоставляемыми учетным данным, которые использовались при подключении к экземпляру. По умолчанию для подключения к компоненту [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с проверкой подлинности Windows поставщик [!INCLUDE[ssDE](../includes/ssde-md.md)]и командлеты используют учетную запись Windows, под которой они работают.  
  
 Для подключения с проверкой подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] необходимо указать идентификатор имени входа и пароль проверки подлинности SQL Server. При использовании поставщика [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] необходимо связать учетные данные для входа на [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с виртуальным диском, а затем с помощью команды перехода в каталог (**cd**) подключиться к этому диску. В Windows PowerShell учетные данные безопасности можно связывать только с виртуальными дисками.  
  
##  <a name="sql-server-authentication-using-a-virtual-drive"></a><a name="SQLAuthVirtDrv"></a> Проверка подлинности SQL Server с помощью виртуального диска  
 **Создание виртуального диска с именем входа для проверки подлинности SQL Server**  
  
1.  Создайте функцию, которая:  
  
    1.  Имеет параметры для имени, назначаемого виртуальному диску, идентификатора имени входа и пути поставщика, который будет связан с виртуальным диском.  
  
    2.  Использует **read-host** для запроса ввода пароля.  
  
    3.  Использует **new-object** для создания объекта учетных данных.  
  
    4.  Использует **new-psdrive** для создания виртуального диска с указанными учетными данными.  
  
2.  Вызовите функцию, чтобы создать виртуальный диск с указанными учетными данными.  
  
### <a name="example-virtual-drive"></a>Пример (виртуальный диск)  
 В этом примере показано создание функции **sqldrive** для создания виртуального диска, который затем будет связан с указанным именем входа для проверки подлинности и экземпляром [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Функция **sqldrive** запрашивает ввод пароля для имени входа, скрывая символы пароля при их вводе. При каждом последующем использовании команды перехода в другой каталог (**cd**) для подключения к пути с помощью имени виртуального диска все операции выполняются с использованием учетных данных для проверки подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , которые были указаны при создании этого диска.  
  
```  
## Create a function that specifies the login and prompts for the password.  
  
function sqldrive  
{  
    param( [string]$name, [string]$login = "MyLogin", [string]$root = "SQLSERVER:\SQL\MyComputer\MyInstance" )  
    $pwd = read-host -AsSecureString -Prompt "Password"  
    $cred = new-object System.Management.Automation.PSCredential -argumentlist $login,$pwd  
    New-PSDrive $name -PSProvider SqlServer -Root $root -Credential $cred -Scope 1  
}  
  
## Use the sqldrive function to create a SQLAuth virtual drive.  
sqldrive SQLAuth  
  
## CD to the virtual drive, which invokes the supplied authentication credentials.  
cd SQLAuth  
```  
  
##  <a name="sql-server-authentication-using-invoke-sqlcmd"></a><a name="SQLAuthInvSqlCmd"></a> Проверка подлинности SQL Server с использованием Invoke-Sqlcmd  
 **Использование Invoke-Sqlcmd для проверки подлинности SQL Server**  
  
1.  Укажите идентификатор имени входа с помощью параметра **-Username**, а связанный с ним пароль — с помощью параметра **-Password**.  
  
### <a name="example-invoke-sqlcmd"></a>Пример (Invoke-Sqlcmd)  
 В этом примере командлет read-host используется для запроса ввода пароля с последующим подключением с проверкой подлинности SQL Server.  
  
```  
## Prompt the user for their password.  
$pwd = read-host -AsSecureString -Prompt "Password"  
  
Invoke-Sqlcmd -Query "SELECT GETDATE() AS TimeOfQuery;" -ServerInstance "MyComputer\MyInstance" -Username "MyLogin" -Password $pwd  
```  
  
## <a name="see-also"></a>См. также:  
 [SQL Server PowerShell](sql-server-powershell.md)   
 [SQL Server PowerShell, поставщик](sql-server-powershell-provider.md)   
 [Invoke-Sqlcmd, командлет](invoke-sqlcmd-cmdlet.md)  
  
  

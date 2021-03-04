---
title: Управление проверкой подлинности для SQL Server в PowerShell
description: Узнайте, как использовать проверку подлинности SQL Server, а не проверку подлинности Windows (по умолчанию) при подключении к экземпляру ядра СУБД.
titleSuffix: SQL Server on Linux
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 10/14/2020
ms.openlocfilehash: 233c456a2e4c7f41efd6038efccc6b00b68eb72d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838068"
---
# <a name="manage-authentication-to-sql-server-in-powershell"></a>Управление проверкой подлинности для SQL Server в PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

По умолчанию компоненты [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell используют при установлении соединения с компонентом [!INCLUDE[ssDE](../includes/ssde-md.md)]проверку подлинности Windows. Для использования проверки подлинности SQL Server необходимо либо определить виртуальный диск PowerShell, либо указать параметры **-Username** и **-Password** для **Invoke-Sqlcmd**.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="permissions"></a>Разрешения

Все действия, которые могут быть выполнены на экземпляре компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] , определяются разрешениями, предоставляемыми учетным данным, которые использовались при подключении к экземпляру. По умолчанию для подключения к компоненту [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с проверкой подлинности Windows поставщик [!INCLUDE[ssDE](../includes/ssde-md.md)]и командлеты используют учетную запись Windows, под которой они работают.  

Для подключения с проверкой подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] необходимо указать идентификатор имени входа и пароль проверки подлинности SQL Server. При использовании поставщика [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] необходимо связать учетные данные для входа на [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] с виртуальным диском, а затем с помощью команды перехода в каталог (**cd**) подключиться к этому диску. В Windows PowerShell учетные данные безопасности можно связывать только с виртуальными дисками.  

## <a name="sql-server-authentication-using-a-virtual-drive"></a>Проверка подлинности SQL Server с помощью виртуального диска

### <a name="to-create-a-virtual-drive-associated-with-a-sql-server-authentication-login"></a>Создание виртуального диска с именем входа для проверки подлинности SQL Server

1. Создайте функцию, которая:

    1. Имеет параметры для имени, назначаемого виртуальному диску, идентификатора имени входа и пути поставщика, который будет связан с виртуальным диском.

    2. Использует **read-host** для запроса ввода пароля.  

    3. Использует **new-object** для создания объекта учетных данных.  

    4. Использует **new-psdrive** для создания виртуального диска с указанными учетными данными.  

2. Вызовите функцию, чтобы создать виртуальный диск с указанными учетными данными.  

#### <a name="example-virtual-drive"></a>Пример (виртуальный диск)

В этом примере показано создание функции **sqldrive** для создания виртуального диска, который затем будет связан с указанным именем входа для проверки подлинности и экземпляром [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Функция **sqldrive** запрашивает ввод пароля для имени входа, скрывая символы пароля при их вводе. При каждом последующем использовании команды перехода в другой каталог (**cd**) для подключения к пути с помощью имени виртуального диска все операции выполняются с использованием учетных данных для проверки подлинности [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , которые были указаны при создании этого диска.  
  
```powershell
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
  
## Set-Location to the virtual drive, which invokes the supplied authentication credentials.  
sl SQLAuth:
```

## <a name="sql-server-authentication-using-invoke-sqlcmd"></a>Проверка подлинности SQL Server с использованием Invoke-Sqlcmd

### <a name="to-use-invoke-sqlcmd-with-sql-server-authentication"></a>Использование Invoke-Sqlcmd для проверки подлинности SQL Server

1. Укажите идентификатор имени входа с помощью параметра **-Username**, а связанный с ним пароль — с помощью параметра **-Password**.  

#### <a name="example-invoke-sqlcmd"></a>Пример (Invoke-Sqlcmd)

В этом примере командлет read-host используется для запроса ввода пароля с последующим подключением с проверкой подлинности SQL Server.  

```powershell
## Prompt the user for their password.  
$pwd = read-host -AsSecureString -Prompt "Password"  
  
Invoke-Sqlcmd -Query "SELECT GETDATE() AS TimeOfQuery;" -ServerInstance "MyComputer\MyInstance" -Username "MyLogin" -Password $pwd  
```

## <a name="see-also"></a>См. также:

- [SQL Server PowerShell](sql-server-powershell.md)
- [Поставщик SQL Server PowerShell](sql-server-powershell-provider.md)
- [Invoke-Sqlcmd, командлет](/powershell/module/sqlserver/invoke-sqlcmd)
- [Использование PowerShell с Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)

---
title: Указание экземпляров в поставщике SQL Server PowerShell
description: Узнайте, как указать экземпляры при использовании поставщика SQL Server PowerShell.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.openlocfilehash: 4e99faa32b1dfa071a29e3e5b37ee513fa326b5e
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839466"
---
# <a name="specify-instances-in-the-sql-server-powershell-provider"></a>Указание экземпляров в поставщике SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Пути, указанные для поставщика SQL Server для PowerShell, должны идентифицировать экземпляр компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] и компьютер, на котором он запущен. Синтаксическая конструкция для указания компьютера и экземпляра должна соответствовать правилам для идентификаторов SQL Server и путям Windows PowerShell.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="before-you-begin"></a>Перед началом  

Первый узел, следующий за SQLSERVER:\SQL в пути поставщика SQL Server, является именем компьютера, на котором выполняется экземпляр компонента [!INCLUDE[ssDE](../includes/ssde-md.md)], например:  
  
```powershell
SQLSERVER:\SQL\MyComputer  
```  
  
 Если среда Windows PowerShell установлена на одном компьютере с экземпляром компонента [!INCLUDE[ssDE](../includes/ssde-md.md)], то вместо имени компьютера можно использовать localhost или (local). Скрипты, в которых указано localhost или (local), можно выполнять на любом компьютере, при этом изменять их путем внесения имен разных компьютеров не требуется.  
  
 На одном и том же компьютере можно запустить несколько экземпляров исполняемой программы компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] . Узел после имени компьютера в пути поставщика SQL Server определяет экземпляр, например:  
  
```  
SQLSERVER:\SQL\MyComputer\MyInstance  
```  
  
 На каждом компьютере может быть только один экземпляр компонента [!INCLUDE[ssDE](../includes/ssde-md.md)]по умолчанию. При установке экземпляра по умолчанию указывать для него имя не нужно. При указании в строке соединения только имени компьютера происходит подключение к экземпляру по умолчанию на этом компьютере. Все прочие экземпляры на компьютере должны быть именованными. Имя экземпляра указывается во время установки, а в строке подключения необходимо указывать и имя компьютера, и имя экземпляра.  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Ограничения  
 Для указания локального компьютера в скриптах PowerShell нельзя использовать точку (.). Точка не поддерживается, так как PowerShell интерпретирует точку как команду.  
  
 Windows PowerShell обычно обрабатывает символы скобок в (local) как команды. Необходимо либо закодировать, либо экранировать их при использовании в пути, или заключить путь в двойные кавычки. Дополнительные сведения см. в разделе «Шифрование и расшифровка идентификаторов SQL Server».  
  
 Для поставщика [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] всегда необходимо указывать имя экземпляра. Для экземпляров по умолчанию необходимо указывать имя DEFAULT.  
  
##  <a name="examples-computer-and-instance-names"></a><a name="Examples"></a> Примеры; имена компьютеров и экземпляров  
 В этом примере серверный объект устанавливается на экземпляр по умолчанию на локальном компьютере:  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT   
```  
  
 Windows PowerShell обычно обрабатывает символы скобок в (local) как команды. Необходимо:  
  
-   заключить строку пути в кавычки;  
  
    ```  
    Set-Location "SQLSERVER:\SQL\(local)\DEFAULT"  
    ```  
  
-   экранировать скобки при помощи символа обратной кавычки (`);  
  
    ```  
    Set-Location SQLSERVER:\SQL\`(local`)\DEFAULT  
    ```  
  
-   зашифровать скобки с помощью шестнадцатеричного представления:  
  
    ```  
    Set-Location SQLSERVER:\SQL\%28local%29\DEFAULT  
    ```  
  
## <a name="see-also"></a>См. также:  
 [Идентификаторы SQL Server в PowerShell](sql-server-identifiers-in-powershell.md)   
 [SQL Server PowerShell, поставщик](sql-server-powershell-provider.md)   
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  

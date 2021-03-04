---
title: Применение escape-символов к идентификаторам SQL Server
description: Некоторые символы, отображаемые в идентификаторах SQL Server с разделителями, не поддерживаются в путях Windows PowerShell. Узнайте, как можно экранировать некоторые из них с помощью символа обратной кавычки.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 105acf1e3e67d558ccbb0ae450afba9468de3a5f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838038"
---
# <a name="escape-sql-server-identifiers"></a>Применение escape-символов к идентификаторам SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Часто можно использовать экранирующий символ обратной кавычки (`) для escape-символов, применение которых допускается в идентификаторах с разделителями [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], но не в именах путей Windows PowerShell. Тем не менее экранирование некоторых символов невозможно. Например, в среде Windows PowerShell нельзя экранировать символ двоеточия (:). Идентификаторы с этим символом должны быть закодированы. Кодировка более надежна, чем экранирование, поскольку действует для всех символов.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Символ обратной кавычки (`) обычно расположен на клавише в верхнем левом углу клавиатуры, под клавишей ESC.  

## <a name="examples"></a>Примеры

Ниже приведен пример экранирования символа #:  

```powershell
cd SQLSERVER:\SQL\MyComputer\MyInstance\MyDatabase\MySchema\`#MyTempTable  
```

Это пример экранирования скобок при указании (local) в качестве имени компьютера:  

```powershell
Set-Location SQLSERVER:\SQL\`(local`)\DEFAULT  
```

## <a name="see-also"></a>См. также:

- [Идентификаторы SQL Server в PowerShell](sql-server-identifiers-in-powershell.md)
- [Поставщик SQL Server PowerShell](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)
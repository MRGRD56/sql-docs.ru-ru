---
title: Шифрование и расшифровка идентификаторов SQL Server
description: Некоторые символы, отображаемые в идентификаторах SQL Server с разделителями, не поддерживаются в путях Windows PowerShell. Узнайте, как их можно включить, представив в виде шестнадцатеричных значений.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 46eb3f29ce21aada716458d554dd329b1bd10dff
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981190"
---
# <a name="encode-and-decode-sql-server-identifiers"></a>Шифрование и расшифровка идентификаторов SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Идентификаторы SQL Server с разделителями иногда содержат символы, не поддерживаемые в путях Windows PowerShell. Эти символы можно задавать путем кодирования их шестнадцатеричных значений.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Символы, неподдерживаемые в именах путей Windows PowerShell, могут быть представлены или закодированы в виде символа «%», за которым следует шестнадцатеричное значение для битового шаблона, представляющего символ, например « **%** xx». Для обработки символов, неподдерживаемых в обозначениях путей Windows PowerShell, всегда можно использовать кодировку.

Командлет **Encode-SqlName** принимает в качестве входных данных идентификатор [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Он возвращает строку, в которой все символы, не поддерживаемые языком Windows PowerShell, закодированы в виде «%xx». Командлет **Decode-SqlName** принимает в качестве входных данных закодированный идентификатор [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] и возвращает исходный идентификатор.  

## <a name="limitations-and-restrictions"></a>Ограничения

Командлеты **Encode-Sqlname** и **Decode-Sqlname** обеспечивают только кодирование или декодирование символов, допустимых в идентификаторах SQL Server с разделителями, но не поддерживаемых в путях PowerShell. Символы, кодируемые командлетом **Encode-SqlName** и декодируемые командлетом **Decode-SqlName**, перечислены ниже.

|**Символ**|\ |/|:|%|\<|>|*|?|[|]|&#124;|  
|-|-|-|-|-|-|-|-|-|-|-|-|
|**Шестнадцатеричная кодировка**|%5C|%2F|%3A|%25|%3C|%3E|%2A|%3F|%5B|%5D|%7C|

## <a name="encoding-an-identifier"></a>кодирование идентификатора  

### <a name="to-encode-a-sql-server-identifier-in-a-powershell-path"></a>Кодирование идентификатора SQL Server в пути PowerShell

- Используйте один из двух методов для кодирования идентификатора SQL Server:
    - Укажите шестнадцатеричный код для неподдерживаемого символа, используя синтаксис %XX, где XX — шестнадцатеричный код.
    - Передайте идентификатор в виде строки, заключенной в кавычки, в командлет **Encode-Sqlname** .

### <a name="examples-encoding"></a>Примеры (кодирование)

В этом примере указана закодированная версия символа «:» (%3A):

```powershell
Set-Location Table%3ATest
```

Можно также использовать **Encode-SqlName** для формирования имени, поддерживаемого Windows PowerShell:

```powershell
Set-Location (Encode-SqlName "Table:Test")
```

## <a name="decoding-an-identifier"></a>декодирование идентификатора

### <a name="to-decode-a-sql-server-identifier-from-a-powershell-path"></a>Декодирование идентификатора SQL Server из пути PowerShell

Используйте командлет **Decode-Sqlname** для замены шестнадцатеричных кодов символами, представленными этими кодами.

### <a name="examples-decoding"></a>Примеры (декодирование)

В этом примере происходит возврат строки Table:Test:

```powershell
Decode-SqlName "Table%3ATest"
```

## <a name="see-also"></a>См. также:

- [Идентификаторы SQL Server в PowerShell](sql-server-identifiers-in-powershell.md)
- [Поставщик SQL Server PowerShell](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)  

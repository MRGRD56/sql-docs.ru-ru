---
description: Шаг 3. Сервер получает набор записей (учебник по RDS)
title: Шаг 3. сервер получает набор записей (учебник по RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], server obtains Recordset
ms.assetid: 9c6779c9-1208-4696-ac51-c39f3a6d9240
author: rothja
ms.author: jroth
ms.openlocfilehash: d031c1af5719b711c5555af0db1bc1e420e2fc8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036104"
---
# <a name="step-3-server-obtains-a-recordset-rds-tutorial"></a>Шаг 3. Сервер получает набор записей (учебник по RDS)
Серверная программа использует строку подключения и текст команды, чтобы запросить источник данных для нужных строк. ADO обычно используется для получения этого **набора записей**, хотя можно использовать и другие интерфейсы доступа к данным Майкрософт, например OLE DB.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
 Пользовательская серверная программа может выглядеть следующим образом:  
  
```vb
Public Function ServerProgram(cn as String, qry as String) as Object  
Dim rs as New ADODB.Recordset  
   rs.CursorLocation = adUseClient  
   rs.Open qry, cn   
   rs.ActiveConnection = Nothing  
   Set ServerProgram = rs  
End Function  
```  
  
## <a name="see-also"></a>См. также:  
 [Шаг 4. сервер возвращает набор записей (учебник по RDS)](./step-4-server-returns-the-recordset-rds-tutorial.md)   
 [Учебник по RDS (VBScript)](./rds-tutorial-vbscript.md)
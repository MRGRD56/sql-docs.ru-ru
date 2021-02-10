---
description: Шаг 1. Укажите программу сервера (учебник по RDS)
title: Шаг 1. Указание серверной программы (учебник по RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], specifying server program
ms.assetid: d8bb35b1-c02a-4231-8d55-016e56e53b95
author: rothja
ms.author: jroth
ms.openlocfilehash: d0e59c52ee311c5172b3cdc15f58c5e9448f7110
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036404"
---
# <a name="step-1-specify-a-server-program-rds-tutorial"></a>Шаг 1. Укажите программу сервера (учебник по RDS)
В общем случае используйте [RDS. ](../../reference/rds-api/dataspace-object-rds.md) Метод [CreateObject](../../reference/rds-api/createobject-method-rds.md) объекта Space для указания программы сервера по умолчанию, [RDSServer.](../../reference/rds-api/datafactory-object-rdsserver.md)DataObject или собственной пользовательской серверной программы (бизнес-объект). На сервере создается экземпляр серверной программы, и возвращается ссылка на серверную программу или *прокси*.  
  
 В этом руководстве используется серверная программа по умолчанию:  
  
```vb
Sub RDSTutorial1()  
   Dim DS as New RDS.DataSpace  
   Dim DF as Object  
   Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
...  
```  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>См. также:  
 [Шаг 2. вызов серверной программы (учебник по RDS)](./step-2-invoke-the-server-program-rds-tutorial.md)   
 [Учебник по RDS (VBScript)](./rds-tutorial-vbscript.md)
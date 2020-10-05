---
description: Свойство InternetTimeout (служба удаленных рабочих столов)
title: Свойство InternetTimeout (RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- InternetTimeout property [ADO]
ms.assetid: 4d1c8892-4bbc-4e71-bf4b-ba52c0ea9549
author: rothja
ms.author: jroth
ms.openlocfilehash: 43ab46eefcb897511a2990655362ecb10527be19
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91724505"
---
# <a name="internettimeout-property-rds"></a>Свойство InternetTimeout (служба удаленных рабочих столов)
Указывает количество миллисекунд ожидания перед истечением времени ожидания запроса.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="settings-and-return-values"></a>Параметры и возвращаемые значения  
 Задает или возвращает значение **типа Long** , представляющее количество миллисекунд до истечения времени ожидания запроса.  
  
## <a name="remarks"></a>Комментарии  
 Это свойство применяется только к запросам, отправленным с помощью протоколов HTTP или HTTPS.  
  
 Выполнение запросов в трехуровневой среде может занять несколько минут. Используйте это свойство, чтобы указать дополнительное время для долго выполняющихся запросов.  
  
## <a name="applies-to"></a>Применение  

:::row:::
    :::column:::
        [Объект DataControl (служба удаленных рабочих столов)](./datacontrol-object-rds.md)  
    :::column-end:::
    :::column:::
        [Объект DataSpace (служба удаленных рабочих столов)](./dataspace-object-rds.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>См. также  
 [Пример свойства InternetTimeout (Visual Basic)](./internettimeout-property-example-vb.md)   
 [Пример свойства InternetTimeout (Visual C++)](./internettimeout-property-example-vc.md)
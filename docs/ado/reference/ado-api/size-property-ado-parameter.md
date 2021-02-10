---
description: Свойство Size (объект Parameter ADO)
title: Свойство Size (параметр ADO) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Size
helpviewer_keywords:
- Size property [ADO Parameter]
ms.assetid: e6bad449-ebdb-4dd3-886a-9e6f1e7ee5d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 57682be31c8640dc68b149495df2b0b36639d419
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051445"
---
# <a name="size-property-ado-parameter"></a>Свойство Size (объект Parameter ADO)
Указывает максимальный размер объекта [параметра](./parameter-object.md) в байтах или символах.  
  
## <a name="settings-and-return-values"></a>Параметры и возвращаемые значения  
 Задает или возвращает значение **типа Long** , указывающее максимальный размер в байтах или символах значения в объекте **параметра** .  
  
## <a name="remarks"></a>Remarks  
 Свойство **size** используется для определения максимального размера значений, записываемых или считываемых из свойства [value](./value-property-ado.md) объекта **Parameter** .  
  
 Если для объекта **параметра** задан тип данных переменной длины (например, любой **строковый** тип, например **адварчар**), необходимо задать свойство **size** объекта перед его добавлением в коллекцию [Parameters](./parameters-collection-ado.md) . в противном случае возникает ошибка.  
  
 Если объект **параметра** уже добавлен в коллекцию **Parameters** объекта [Command](./command-object-ado.md) и вы измените его тип на тип данных переменной длины, необходимо задать свойство **size** объекта **Parameter** перед выполнением объекта **команды** . в противном случае возникает ошибка.  
  
 Если вы используете метод [Refresh](./refresh-method-ado.md) для получения сведений о параметрах от поставщика и возвращает один или несколько объектов **параметров** типа данных переменной длины, ADO может выделить память для параметров на основе максимального возможного размера, что может вызвать ошибку во время выполнения. Чтобы предотвратить возникновение ошибки, необходимо явно задать свойство **size** для этих параметров перед выполнением команды.  
  
 Свойство **size** доступно для чтения и записи.  
  
## <a name="applies-to"></a>Применение  
 [Объект Parameter](./parameter-object.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойств ActiveConnection, CommandText, CommandTimeout, CommandType, Size и Direction (Visual Basic)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Пример свойств ActiveConnection, CommandText, CommandTimeout, CommandType, Size и Direction (Visual c++)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Пример свойств ActiveConnection, CommandText, CommandTimeout, CommandType, Size и Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Свойство Size (объект Stream ADO)](./size-property-ado-stream.md)
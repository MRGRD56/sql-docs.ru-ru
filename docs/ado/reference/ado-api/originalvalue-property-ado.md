---
description: Свойство OriginalValue (ADO)
title: Свойство OriginalValue (ADO) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::OriginalValue
helpviewer_keywords:
- OriginalValue property [ADO]
ms.assetid: 6e33c6ec-14d9-4b1d-ba9b-cb99862e7bac
author: rothja
ms.author: jroth
ms.openlocfilehash: dd53935335a4f5df694a5a5175f2bd7d3505a108
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170645"
---
# <a name="originalvalue-property-ado"></a>Свойство OriginalValue (ADO)
Указывает значение [поля](./field-object.md) , которое существовало в записи до внесения каких-либо изменений.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Возвращает значение **типа Variant** , представляющее значение поля до любого изменения.  
  
## <a name="remarks"></a>Замечания  
 Используйте свойство **originalValue** , чтобы вернуть исходное значение поля для поля из текущей записи.  
  
 В *режиме немедленного обновления* (в котором поставщик записывает изменения в базовый источник данных после вызова метода [Update](./update-method.md) ) свойство **originalValue** возвращает значение поля, существовавшее до любых изменений (то есть с момента последнего вызова метода **обновления** ). Это то же значение, которое метод [CancelUpdate](./cancelupdate-method-ado.md) использует для замены свойства [value](./value-property-ado.md) .  
  
 В *режиме пакетного обновления* (в котором поставщик кэширует несколько изменений и записывает их в базовый источник данных только при вызове метода [UpdateBatch](./updatebatch-method.md) ), свойство **originalValue** возвращает значение поля, существовавшее до любых изменений (то есть с момента последнего вызова метода **UpdateBatch** ). Это то же значение, которое метод [CancelBatch](./cancelbatch-method-ado.md) использует для замены свойства **value** . При использовании этого свойства со свойством [UnderlyingValue](./underlyingvalue-property.md) можно разрешить конфликты, возникающие в пакетных обновлениях.  
  
## <a name="record"></a>Record  
 Для объектов [записи](./record-object-ado.md) свойство **originalValue** будет пустым для полей, добавленных до вызова метода [Update](./update-method.md) .  
  
## <a name="applies-to"></a>Применение  
 [Объект Field](./field-object.md)  
  
## <a name="see-also"></a>См. также:  
 [Примеры свойств OriginalValue и UnderlyingValue (Visual Basic)](./originalvalue-and-underlyingvalue-properties-example-vb.md)   
 [Пример свойств OriginalValue и UnderlyingValue (Visual c++)](./originalvalue-and-underlyingvalue-properties-example-vc.md)   
 [Свойство UnderlyingValue](./underlyingvalue-property.md)
---
description: Свойство UnderlyingValue
title: UnderlyingValue, свойство | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::GetUnderlyingValue
- Field20::get_UnderlyingValue
- Field20::UnderlyingValue
helpviewer_keywords:
- UnderlyingValue property
ms.assetid: 00a0c8b8-8b63-433f-95b8-020ab05874a0
author: rothja
ms.author: jroth
ms.openlocfilehash: d96f48d796e3eef7b41467ea4f5b5b14bfc66d36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056379"
---
# <a name="underlyingvalue-property"></a>Свойство UnderlyingValue
Указывает текущее значение объекта [поля](./field-object.md) в базе данных.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Возвращает значение **типа Variant** , указывающее значение **поля**.  
  
## <a name="remarks"></a>Remarks  
 Используйте свойство **UnderlyingValue** , чтобы вернуть текущее значение поля из базы данных. Значение поля в свойстве **UnderlyingValue** является значением, видимым для транзакции и может быть результатом последнего обновления другой транзакции. Это может отличаться от свойства [originalValue](./originalvalue-property-ado.md) , которое отражает значение, первоначально возвращенное [набору записей](./recordset-object-ado.md).  
  
 Это похоже на использование метода повторной [синхронизации](./resync-method.md) , но свойство **UnderlyingValue** возвращает только значение для определенного поля из текущей записи. Это то же значение, которое метод [Resync](./resync-method.md) использует для замены свойства [value](./value-property-ado.md) .  
  
 При использовании этого свойства со свойством **originalValue** можно разрешить конфликты, возникающие в пакетных обновлениях.  
  
## <a name="record"></a>Record  
 Для объектов [записи](./record-object-ado.md) это свойство будет пустым для полей, добавленных до вызова метода [Update](./update-method.md) .  
  
## <a name="applies-to"></a>Применение  
 [Объект Field](./field-object.md)  
  
## <a name="see-also"></a>См. также:  
 [Примеры свойств OriginalValue и UnderlyingValue (Visual Basic)](./originalvalue-and-underlyingvalue-properties-example-vb.md)   
 [Пример свойств OriginalValue и UnderlyingValue (Visual c++)](./originalvalue-and-underlyingvalue-properties-example-vc.md)   
 [Свойство OriginalValue (ADO)](./originalvalue-property-ado.md)   
 [Метод Resync](./resync-method.md)
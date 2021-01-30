---
description: Свойство Type (объект Stream ADO)
title: Свойство Type (поток ADO) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::Type
- _Stream::get_Type
- _Stream::put_Type
helpviewer_keywords:
- Type property [ADO Stream]
ms.assetid: f6a17e8c-7a28-48d0-bded-76b9e0cf7639
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e170cd368771fc75c2c6e552d4465dfe51866c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166378"
---
# <a name="type-property-ado-stream"></a>Свойство Type (объект Stream ADO)
Указывает тип данных, содержащихся в [потоке](./stream-object-ado.md) (двоичный или текстовый).  
  
## <a name="settings-and-return-values"></a>Параметры и возвращаемые значения  
 Задает или возвращает значение [стреамтипинум](./streamtypeenum.md) , указывающее тип данных, содержащихся в объекте **потока** . Значение по умолчанию — **адтипетекст**. Однако, если двоичные данные изначально записываются в новый пустой **поток**, **Тип** изменится на **адтипебинари**.  
  
## <a name="remarks"></a>Замечания  
 Свойство **Type** доступно только для чтения и записи, если текущая координата находится в начале **потока** (значение [позиции](./position-property-ado.md) равно 0), а в любой другой позиции — только для чтения.  
  
 Свойство **Type** определяет, какие методы следует использовать для чтения и записи **потока**. Для текстовых **потоков** используйте [ReadText](./readtext-method.md) и [WriteText](./writetext-method.md). Для двоичных **потоков** используйте [Чтение](./read-method.md) и [запись](./write-method.md).  
  
## <a name="applies-to"></a>Применение  
 [Объект Stream (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>См. также:  
 [Свойство RecordType (ADO)](./recordtype-property-ado.md)   
 [Свойство Type (ADO)](./type-property-ado.md)
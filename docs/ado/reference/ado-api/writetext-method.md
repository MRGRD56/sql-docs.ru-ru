---
description: Метод WriteText
title: Метод WriteText | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Stream::raw_WriteText
- _Stream::WriteText
helpviewer_keywords:
- WriteText method [ADO]
ms.assetid: 7a669048-13f4-4574-a2b1-985e089729d5
author: rothja
ms.author: jroth
ms.openlocfilehash: b4c05ab892ce442db5eefefddf3eb0796af5dec9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166244"
---
# <a name="writetext-method"></a>Метод WriteText
Записывает указанную текстовую строку в объект [потока](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
Stream.WriteText Data, Options  
```  
  
#### <a name="parameters"></a>Параметры  
 *Данные*  
 **Строковое** значение, содержащее текст в символах для написания.  
  
 *Параметры*  
 Необязательный параметр. Значение [стреамвритинум](./streamwriteenum.md) , указывающее, должен ли символ разделителя строки записываться в конце указанной строки.  
  
## <a name="remarks"></a>Замечания  
 Указанные строки записываются в объект **потока** без промежуточных пробелов или символов между строками.  
  
 Текущей [позиции](./position-property-ado.md) присваивается символ, следующий за записанными данными. Метод **WriteText** не усекает остальные данные в потоке. Если вы хотите усечь эти символы, вызовите [сетеос](./seteos-method.md).  
  
 Если вы пишете после текущей позиции [EOS](./eos-property.md) , [Размер](./size-property-ado-stream.md) **потока** будет увеличен, чтобы вместить новые символы, а **EOS** перейдет к новому байтовому байту в **потоке**.  
  
> [!NOTE]
>  Метод **WriteText** используется с потоками текста ([Type](./type-property-ado-stream.md) — **адтипетекст**). Для двоичных потоков (**Type** — **Адтипебинари**) используйте [Write](./write-method.md).  
  
## <a name="applies-to"></a>Применение  
 [Объект Stream (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>См. также:  
 [Метод Write](./write-method.md)
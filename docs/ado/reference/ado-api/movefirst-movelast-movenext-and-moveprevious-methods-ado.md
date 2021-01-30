---
description: Методы MoveFirst, MoveLast, MoveNext и MovePrevious (ADO)
title: Методы MoveFirst, MoveLast, MoveNext и MovePrevious (ADO) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::MoveLast
- Recordset15::MoveNext
- Recordset15::raw_MoveNext
- Recordset15::raw_MovePrevious
- Recordset15::MoveFirst
- Recordset15::raw_MoveLast
- Recordset15::MovePrevious
- Recordset15::raw_MoveFirst
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- MovePrevious method [ADO]
ms.assetid: a61a01a7-5b33-4150-9126-21dfa63654cb
author: rothja
ms.author: jroth
ms.openlocfilehash: 2c0f227b7c66928d04980a454828e8d7a80f064f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167093"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-ado"></a>Методы MoveFirst, MoveLast, MoveNext и MovePrevious (ADO)
Переходит к первой, последней, следующей или предыдущей записи в указанном объекте [набора записей](./recordset-object-ado.md) и делает запись текущей записью.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
## <a name="remarks"></a>Remarks  
 Используйте метод **MoveFirst** , чтобы переместить текущую запись в первую запись в **наборе записей**.  
  
 Используйте метод **MoveLast** , чтобы переместить текущую запись в последнюю запись в **наборе записей**. Объект **Recordset** должен поддерживать закладки или обратное перемещение курсора; в противном случае вызов метода выдаст ошибку.  
  
 Вызов метода **MoveFirst** или **MoveLast** , если **набор записей** пуст (значение **BOF** и **EOF** равно true), выдает ошибку.  
  
 Используйте метод **MoveNext** для перемещения текущей записи, расположенной на одну запись вперед (в нижней части **набора записей**). Если последняя запись является текущей и вызывается метод **MoveNext** , ADO устанавливает текущую запись в качестве текущей записи после последней записи в **наборе записей** (значение [EOF](./bof-eof-properties-ado.md) равно **true**). Попытка перемещения вперед, если свойство **EOF** уже имеет **значение true** , приводит к ошибке.  
  
 В ADO 2,5 и более поздних версиях, когда **набор записей** фильтруется или сортируется и изменяются данные текущей записи, вызов метода **MoveNext** перемещает курсор на две записи вперед от текущей записи. Это происходит потому, что при изменении текущей записи Следующая запись становится новой текущей записью. Вызов **MoveNext** после изменения перемещает курсор на одну запись вперед от новой текущей записи. Это отличается от поведения в ADO 2,1 и более ранних версиях. В этих более ранних версиях изменение данных текущей записи в отсортированном или фильтруемом **наборе записей** не изменяет позицию текущей записи, а **MoveNext** перемещает курсор на следующую запись сразу после текущей записи.  
  
 Используйте метод **MovePrevious** , чтобы переместить текущую запись в обратном направлении (в верхнюю часть **набора записей**). Объект **Recordset** должен поддерживать закладки или обратное перемещение курсора; в противном случае вызов метода выдаст ошибку. Если первая запись является текущей и вызывается метод **MovePrevious** , ADO устанавливает текущую запись в точку перед первой записью в **наборе записей** ([BOF](./bof-eof-properties-ado.md) имеет **значение true**). Попытка перемещения назад, если свойство **BOF** уже имеет **значение true** , приводит к ошибке. Если объект **Recordset** не поддерживает ни закладки, ни обратное перемещение курсора, метод **MovePrevious** выдаст ошибку.  
  
 Если **набор записей** предназначен только для переадресации и требуется поддержка прямой и обратной прокрутки, можно использовать свойство [CacheSize](./cachesize-property-ado.md) для создания кэша записей, который будет поддерживать обратное перемещение курсора через метод [Move](./move-method-ado.md) . Поскольку кэшированные записи загружаются в память, следует избегать кэширования большего количества записей, чем требуется. Метод **MoveFirst** можно вызвать в объекте **набора записей** последовательного доступа. Это может привести к тому, что поставщик повторно выполнит команду, создавшую объект **набора записей** .  
  
## <a name="applies-to"></a>Применение  
 [Объект Recordset (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>См. также:  
 [Примеры методов MoveFirst, MoveLast, MoveNext и MovePrevious (Visual Basic)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vb.md)   
 [Пример методов MoveFirst, MoveLast, MoveNext и MovePrevious (VBScript)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vbscript.md)   
 [Пример методов MoveFirst, MoveLast, MoveNext и MovePrevious (Visual c++)](./movefirst-movelast-movenext-and-moveprevious-methods-example-vc.md)   
 [Метод Move (ADO)](./move-method-ado.md)   
 [Методы MoveFirst, MoveLast, MoveNext и MovePrevious (RDS)](../rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)   
 [Метод MoveRecord (ADO)](./moverecord-method-ado.md)
---
description: Свойство Parent (многомерные объекты ADO)
title: Свойство Parent (объекты данных ActiveX (MD)) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parent
- Member::Parent
helpviewer_keywords:
- Parent property [ADO MD]
ms.assetid: 32c278c1-d8e1-4bb7-9ecd-2fbfdffee34b
author: rothja
ms.author: jroth
ms.openlocfilehash: 0674b3455611e8700e8859d4ced5d4052c51135c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050955"
---
# <a name="parent-property-ado-md"></a>Свойство Parent (многомерные объекты ADO)
Указывает элемент, являющийся родительским по отношению к текущему [элементу](./member-object-ado-md.md) в иерархии.  
  
## <a name="return-values"></a>Возвращаемые значения  
 Возвращает объект [члена](./member-object-ado-md.md) и доступен только для чтения.  
  
## <a name="remarks"></a>Remarks  
 Элемент, расположенный на верхнем уровне иерархии (корень), не имеет родителя. Это свойство поддерживается только для объектов- **членов** , принадлежащих объекту [уровня](./level-object-ado-md.md) . Ошибка возникает, когда на это свойство ссылаются объекты- **члены** , принадлежащие объекту- [положению](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>Применение  
 [Объект Member (многомерные объекты ADO)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>См. также:  
 [Свойство Children (многомерные объекты ADO)](./children-property-ado-md.md)
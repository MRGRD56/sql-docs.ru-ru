---
description: Коллекция Members (многомерные объекты ADO)
title: Коллекция Members (объекты данных ActiveX (MD)) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Level::Members
- Members
- Position::Members
helpviewer_keywords:
- Members collection [ADO MD]
ms.assetid: 3a647cde-efdc-4394-b1b9-8cbb1b9d689f
author: rothja
ms.author: jroth
ms.openlocfilehash: d628481607e05c2278e5dd1beede08a12b93d740
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054499"
---
# <a name="members-collection-ado-md"></a>Коллекция Members (многомерные объекты ADO)
Содержит объекты- [члены](./member-object-ado-md.md) уровня или расположения вдоль оси.  
  
## <a name="remarks"></a>Remarks  
 Коллекция **Members** используется для хранения следующих типов членов:  
  
-   Элементы, составляющие уровень в Кубе. Они содержатся в коллекции **Members** объекта [Level](./level-object-ado-md.md) . Например, используя пример из [обзора многомерных схем и данных](../../guide/multidimensional/overview-of-multidimensional-schemas-and-data.md), четыре элемента уровня «страны» — это канада, США, Великобритания и Германия.  
  
-   Элементы, являющиеся дочерними элементами определенного элемента в иерархии. Эти члены возвращаются свойством [Children](./children-property-ado-md.md) родительского объекта **member** . Например, опять же, используя тот же пример, два дочерних элемента в Канаде — Canada-East и Канада-Запад.  
  
-   Элементы, определяющие определенную позицией вдоль оси набора [ячеек](./cellset-object-ado-md.md). Используя набор ячеек для [работы с многомерными данными](../../guide/multidimensional/working-with-multidimensional-data.md) в качестве примера, два элемента первой должности на оси x — любимая и Сиэтл. Эти элементы содержатся в коллекции **Members** объекта [позиционирования](./position-object-ado-md.md) .  
  
 **Members** — это стандартная коллекция ADO. С помощью свойств и методов коллекции можно выполнять следующие действия.  
  
-   Получите количество объектов в коллекции со свойством [Count](../ado-api/count-property-ado.md) .  
  
-   Возврат объекта из коллекции со свойством [элемента](../ado-api/item-property-ado.md) по умолчанию.  
  
-   Обновите объекты в коллекции от поставщика с помощью метода [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Этот раздел содержит следующий раздел.  
  
-   [Свойства, методы и события](./members-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример членов (VBScript)](./members-example-vbscript.md)   
 [Объект Member (многомерные объекты ADO)](./member-object-ado-md.md)
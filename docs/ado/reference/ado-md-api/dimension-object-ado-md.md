---
description: Объект Dimension (многомерные объекты ADO)
title: Объект Dimension (объекты данных ActiveX (MD)) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Dimension
helpviewer_keywords:
- Dimension object [ADO MD]
ms.assetid: 66adbbd2-23a3-4c19-a91b-84c31309aa1b
author: rothja
ms.author: jroth
ms.openlocfilehash: 8a8738c6e5143a8fc23da2c008a01b28db36ee04
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174280"
---
# <a name="dimension-object-ado-md"></a>Объект Dimension (многомерные объекты ADO)
Представляет одно из измерений многомерного куба, содержащего одну или несколько иерархий элементов.  
  
## <a name="remarks"></a>Замечания  
 С помощью коллекций и свойств объекта **Dimension** можно выполнять следующие действия.  
  
-   Определяет **измерение** с помощью свойств [Name](./name-property-ado-md.md) и [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Возвращает осмысленную строку, описывающую **измерение** со свойством [Description](./description-property-ado-md.md) .  
  
-   Возвращает объекты [иерархии](./hierarchy-object-ado-md.md) , составляющие **измерение** , с помощью коллекции [иерархий](./hierarchies-collection-ado-md.md) .  
  
-   Используйте коллекцию стандартных [свойств](../ado-api/properties-collection-ado.md) ADO для получения дополнительных сведений об объекте **измерения** .  
  
 Коллекция **Properties** содержит свойства, предоставляемые поставщиком. В следующей таблице перечислены свойства, которые могут быть доступны. Фактический список свойств может отличаться в зависимости от реализации поставщика. Более полный список доступных свойств см. в документации поставщика.  
  
|name|Описание|  
|----------|-----------------|  
|CatalogName|Имя каталога, которому принадлежит куб.|  
|CubeName|Имя куба.|  
|DefaultHierarchy|Уникальное имя иерархии по умолчанию.|  
|Описание|Понятное описание Куба.|  
|дименсионкаптион|Метка или заголовок, связанный с измерением.|  
|дименсионкардиналити|Число элементов в измерении.|  
|дименсионгуид|Идентификатор GUID измерения.|  
|DimensionName|Имя размерности.|  
|дименсионординал|Порядковый номер измерения в группе измерений, образующих куб.|  
|DimensionType|Тип измерения.|  
|дименсионуникуенаме|Однозначное имя измерения.|  
|SchemaName|Имя схемы, к которой принадлежит куб.|  
  
 Этот раздел содержит следующий раздел.  
  
-   [Свойства, методы и события](./dimension-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Объект CubeDef (объекты данных ActiveX (MD))](./cubedef-object-ado-md.md)   
 [Коллекция Dimensions (объекты данных ActiveX (MD))](./dimensions-collection-ado-md.md)   
 [Коллекция иерархий (объекты данных ActiveX (MD))](./hierarchies-collection-ado-md.md)   
 [Коллекция Properties (ADO)](../ado-api/properties-collection-ado.md)
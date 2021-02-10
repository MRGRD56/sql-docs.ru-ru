---
description: Объект CubeDef (многомерные объекты ADO)
title: Объект CubeDef (объекты данных ActiveX (MD)) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CubeDef
helpviewer_keywords:
- CubeDef object [ADO MD]
ms.assetid: feb2581c-fc41-471c-bb69-29f8a55fda70
author: rothja
ms.author: jroth
ms.openlocfilehash: b9f25f238fd8c0637f86ff7fdf5d9fc17320fdf7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054819"
---
# <a name="cubedef-object-ado-md"></a>Объект CubeDef (многомерные объекты ADO)
Представляет куб из многомерной схемы, содержащий набор связанных измерений.  
  
## <a name="remarks"></a>Remarks  
 С помощью коллекций и свойств объекта **CubeDef** можно выполнять следующие действия.  
  
-   Найдите **CubeDef** с помощью свойства [Name](./name-property-ado-md.md) .  
  
-   Возвращает строку, описывающую куб со свойством [Description](./description-property-ado-md.md) .  
  
-   Возвращает измерения, которые составляют куб, с помощью коллекции [измерений](./dimensions-collection-ado-md.md) .  
  
-   Получите дополнительные сведения о **CubeDef** со стандартной коллекцией [свойств](../ado-api/properties-collection-ado.md) ADO.  
  
 Коллекция **Properties** содержит свойства, предоставляемые поставщиком. В следующей таблице перечислены свойства, которые могут быть доступны. Фактический список свойств может отличаться в зависимости от реализации поставщика. Более полный список доступных свойств см. в документации поставщика.  
  
|Имя|Описание|  
|----------|-----------------|  
|CatalogName|Имя каталога, которому принадлежит куб.|  
|CreatedOn|Дата и время создания куба.|  
|кубегуид|GUID Куба.|  
|CubeName|Имя куба.|  
|кубетипе|Тип куба.|  
|датаупдатедби|Идентификатор пользователя, выполняющего Последнее обновление данных.|  
|Описание|Понятное описание Куба.|  
|LastSchemaUpdate|Дата и время последнего обновления схемы.|  
|SchemaName|Имя схемы, к которой принадлежит куб.|  
|счемаупдатедби|Идентификатор пользователя, выполняющего Последнее обновление схемы.|  
  
 Этот раздел содержит следующий раздел.  
  
-   [Свойства, методы и события](./cubedef-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Объект каталога (объекты данных ActiveX (MD))](./catalog-object-ado-md.md)   
 [Коллекция Кубедефс (объекты данных ActiveX (MD))](./cubedefs-collection-ado-md.md)   
 [Коллекция Dimensions (объекты данных ActiveX (MD))](./dimensions-collection-ado-md.md)   
 [Коллекция Properties (ADO)](../ado-api/properties-collection-ado.md)
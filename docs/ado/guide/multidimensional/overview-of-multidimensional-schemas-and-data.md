---
description: Общие сведения о многомерных схемах и данных
title: Общие сведения о многомерных схемах и данных | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- multidimensional schemas and data
ms.assetid: ce37fa06-c581-4d80-9a9b-c3aa66408909
author: rothja
ms.author: jroth
ms.openlocfilehash: 2dcd54dfb8ba5797588c615f987830dcb8aa5736
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032186"
---
# <a name="overview-of-multidimensional-schemas-and-data"></a>Общие сведения о многомерных схемах и данных
## <a name="understanding-multidimensional-schemas"></a>Основные сведения о многомерных схемах  
 Центральным объектом метаданных в объекты данных ActiveX (MD) является *куб*, состоящий из структурированного набора связанных измерений, иерархий, уровней и элементов.  
  
 *Измерение* — это независимая категория данных из многомерной базы данных, производная от ваших бизнес-сущностей. Измерение обычно содержит элементы, которые используются в качестве критериев запроса для мер базы данных.  
  
 *Иерархия* — это путь статистической обработки измерения. Измерение может иметь несколько уровней гранулярности, имеющих связи типа «родители-потомки». Иерархия определяет, как эти уровни связаны.  
  
 *Уровень* — это шаг статистической обработки в иерархии. Для измерений с несколькими слоями данных каждый уровень является уровнем.  
  
 Элемент *— это* элемент данных в измерении. Как правило, вы создаете заголовок или описываете меру базы данных с помощью элементов.  
  
 Кубы представлены объектами [CubeDef](../../reference/ado-md-api/cubedef-object-ado-md.md) в объекты данных ActiveX (MD). Измерения, иерархии, уровни и элементы также представлены соответствующими объекты данных ActiveX (MD) объектами: [измерение](../../reference/ado-md-api/dimension-object-ado-md.md), [Иерархия](../../reference/ado-md-api/hierarchy-object-ado-md.md), [уровень](../../reference/ado-md-api/level-object-ado-md.md)и [элемент](../../reference/ado-md-api/member-object-ado-md.md).  
  
### <a name="dimensions"></a>Измерения  
 Измерения куба зависят от бизнес-сущностей и типов данных, которые должны быть смоделированы в базе данных. Как правило, каждое измерение является независимой точкой входа или механизмом для выбора данных.  
  
 Например, куб, содержащий данные о продажах, имеет следующие пять измерений: продавец, география, время, продукты и меры. Измерение Measures содержит фактические значения данных о продажах, а другие измерения — способы категоризации и группирования значений данных о продажах.  
  
 Измерение Geography имеет следующий набор элементов:  
  
```console
{All, North America, Europe, Canada, USA, UK, Germany, Canada-West,  
Canada-East, USA-NW, USA-SW, USA-NE, USA-SE, England, Scotland,   
Wales,Ireland, Germany-North, Germany-South, Ottawa, Toronto,   
Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston,   
Shreveport, Miami, Boston, New York, London, Dover, Glasgow,   
Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin,   
Hamburg, Munich, Stuttgart}  
```  
  
### <a name="hierarchies"></a>Иерархии  
 Иерархии определяют способы, с помощью которых уровни измерения могут быть сведены или сгруппированы. Измерение может иметь более одной иерархии. Естественная иерархия существует в измерении Geography:  
  
### <a name="levels"></a>Уровни  
 В примере измерения Geography, изображенном на предыдущем рисунке, каждое поле представляет уровень в иерархии.  
  
 Каждый уровень имеет набор элементов следующим образом:  
  
-   Мир `= {All}`  
  
-   Континентов `= {North America, Europe}`  
  
-   Таких `= {Canada, USA, UK, Germany}`  
  
-   Регионах `= {Canada-East, Canada-West, USA-NE, USA-NW, USA-SE, USA-SW, England, Ireland, Scotland, Wales, Germany-North, Germany-South}`  
  
-   Городов `= {Ottawa, Toronto, Vancouver, Calgary, Seattle, Boise, Los Angeles, Houston, Shreveport, Miami, Boston, New York, London, Dover, Glasgow, Edinburgh, Cardiff, Pembroke, Belfast, Derry, Berlin, Hamburg, Munich, Stuttgart}`  
  
### <a name="members"></a>Участники  
 Элементы на конечном уровне иерархии не имеют дочерних элементов, а элементы на корневом уровне не имеют родителя. Все остальные члены имеют по крайней мере один родительский элемент и хотя бы один дочерний элемент. Например, частичный обход дерева иерархии в измерении Geography дает следующие связи типа «родители-потомки»:  
  
-   `{All} (parent of) {Europe, North America}`  
  
-   `{North America} (parent of) {Canada, USA}`  
  
-   `{USA} (parent of) {USA-NE, USA-NW, USA-SE, USA-SW}`  
  
-   `{USA-NW} (parent of) {Boise, Seattle}`  
  
 Элементы можно консолидировать по одной или нескольким иерархиям на измерение. Рассмотрим измерение времени, где есть два способа свертки на уровень "год" с уровня "дни":  
  
 В этом примере также иллюстрируется другая характеристика: некоторые элементы уровня «неделя» Year-Week иерархии не отображаются на любом уровне иерархии Year-Quarter. Таким же иерархия не должна включать все элементы измерения.  
  
## <a name="see-also"></a>См. также:  
 [Объектная модель объекты данных ActiveX (MD)](../../reference/ado-md-api/ado-md-object-model.md)   
 [ADO (многомерные) (объекты данных ActiveX (MD))](./ado-multidimensional-ado-md.md)   
 [Программирование с помощью объекты данных ActiveX (MD)](./programming-with-ado-md.md)   
 [Использование ADO с объекты данных ActiveX (MD)](./using-ado-with-ado-md.md)   
 [Работа с многомерными данными](./working-with-multidimensional-data.md)
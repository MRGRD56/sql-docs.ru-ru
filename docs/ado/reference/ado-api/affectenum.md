---
description: AffectEnum
title: Аффектенум | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- AffectEnum
helpviewer_keywords:
- AffectEnum enumeration [ADO]
ms.assetid: 1ab921a0-6c57-43b4-9291-701b2599f3e8
author: rothja
ms.author: jroth
ms.openlocfilehash: 797baaaa554e75fa11287e2288f9a9ffc610b6be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035624"
---
# <a name="affectenum"></a>AffectEnum
Указывает, какие записи затрагиваются операцией.  
  
|Константа|Значение|Описание|  
|--------------|-----------|-----------------|  
|**adAffectAll**|3|Если к **набору записей** не применен [Фильтр](./filter-property.md) , затрагивает все записи.<br /><br /> Если для свойства **Filter** задано строковое условие (например, "author = ' Иванов '"), операция затрагивает видимые записи в текущей главе.<br /><br /> Если для свойства **Filter** задан элемент [филтерграупенум](./filtergroupenum.md) или массив закладок, операция повлияет на все строки **набора записей**. **Примечание. адаффекталл** скрывается в обозревателе объектов Visual Basic.|  
|**адаффекталлчаптерс**|4|Затрагивает все записи во всех соседних главах **набора записей**, включая те, которые не видны через применяемый в данный момент **Фильтр** .|  
|**adAffectCurrent**|1|Влияет только на текущую запись.|  
|**adAffectGroup**|2|Затрагивает только те записи, которые соответствуют текущему значению свойства [фильтра](./filter-property.md) . Для использования этого параметра необходимо задать для свойства **Filter** значение **Филтерграупенум** или массив **закладок** .|  
  
## <a name="adowfc-equivalent"></a>Эквивалент ADO/WFC  
 Пакет: **com. MS. WFC. Data**  
  
|Константа|  
|--------------|  
|Адоенумс. влияет на. ALL|  
|Адоенумс. влияет на. АЛЛЧАПТЕРС|  
|Адоенумс. влияет на. CURRENT|  
|Адоенумс. влияет на. GROUP|  
  
## <a name="applies-to"></a>Применение  

:::row:::
    :::column:::
        [Метод CancelBatch (ADO)](./cancelbatch-method-ado.md)  
        [Метод Delete (объект Recordset ADO)](./delete-method-ado-recordset.md)  
    :::column-end:::
    :::column:::
        [Метод Resync](./resync-method.md)  
        [Метод UpdateBatch](./updatebatch-method.md)  
    :::column-end:::
:::row-end:::
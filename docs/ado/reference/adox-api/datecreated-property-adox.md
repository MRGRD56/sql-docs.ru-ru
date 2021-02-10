---
description: Свойство DateCreated (ADOX)
title: Свойство DateCreated (ADOX) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Table::get_DateCreated
- _Table::DateCreated
- _Table::GetDateCreated
helpviewer_keywords:
- DateCreated property [ADOX]
ms.assetid: 2bf4b00d-045c-444e-8af7-8af6297ed418
author: rothja
ms.author: jroth
ms.openlocfilehash: 5fff28639b996e05e2e0eed14eed357fe62e5e0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050164"
---
# <a name="datecreated-property-adox"></a>Свойство DateCreated (ADOX)
Указывает дату создания объекта.  
  
## <a name="return-values"></a>Возвращаемые значения  
 Возвращает значение **типа Variant** , указывающее дату создания. Значение равно null, если **DateCreated** не поддерживается поставщиком.  
  
## <a name="remarks"></a>Remarks  
 Для вновь добавляемых объектов свойство **DateCreated** имеет значение null. После добавления нового [представления](./view-object-adox.md) или [процедуры](./procedure-object-adox.md)необходимо вызвать метод [Refresh](../ado-api/refresh-method-ado.md) коллекции [представлений](./views-collection-adox.md) или [процедур](./procedures-collection-adox.md) , чтобы получить значения для свойства **DateCreated** .  
  
## <a name="applies-to"></a>Применение  

:::row:::
    :::column:::
        [Объект Procedure (ADOX)](./procedure-object-adox.md)  
    :::column-end:::
    :::column:::
        [Объект Table (ADOX)](./table-object-adox.md)  
    :::column-end:::
    :::column:::
        [Объект View (ADOX)](./view-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>См. также:  
 [Примеры свойств DateCreated и DateModified (Visual Basic)](./datecreated-and-datemodified-properties-example-vb.md)   
 [Свойство DateModified (ADOX)](./datemodified-property-adox.md)
---
description: Коллекция Keys (ADOX)
title: Коллекция Keys (ADOX) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Table::Keys
- Keys
helpviewer_keywords:
- Keys collection [ADOX]
ms.assetid: cdb31c76-e559-475c-b33a-aac24f73e70e
author: rothja
ms.author: jroth
ms.openlocfilehash: 6e3369e181577214a9e4b430dded19110744eb0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169316"
---
# <a name="keys-collection-adox"></a>Коллекция Keys (ADOX)
Содержит все [Ключевые](./key-object-adox.md) объекты [таблицы](./table-object-adox.md).  
  
## <a name="remarks"></a>Замечания  
 Метод [append](./append-method-adox-keys.md) для [коллекции Keys]() уникален для ADOX. Вы можете выполнить следующие действия:  
  
-   Добавьте новый ключ в коллекцию с помощью метода [append](./append-method-adox-keys.md) .  
  
 Остальные свойства и методы являются стандартными для коллекций ADO. Вы можете выполнить следующие действия:  
  
-   Доступ к ключу в коллекции со свойством [Item](../ado-api/item-property-ado.md) .  
  
-   Возвращает количество ключей, содержащихся в коллекции, со свойством [Count](../ado-api/count-property-ado.md) .  
  
-   Удалите ключ из коллекции с помощью метода [Delete](./delete-method-adox-collections.md) .  
  
-   Обновите объекты в коллекции, чтобы отразить схему текущей базы данных методом [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Этот раздел содержит следующий раздел.  
  
-   [Свойства, методы и события коллекции Indexes](./indexes-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойств для добавления ключей, типа ключа, RelatedColumn, RelatedTable и UpdateRule (Visual Basic)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Свойства, методы и события коллекции Keys](./keys-collection-properties-methods-and-events.md)   
 [Объект Key (ADOX)](./key-object-adox.md)
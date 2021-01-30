---
description: Коллекция Tables (ADOX)
title: Коллекция Tables (ADOX) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Tables
- Tables
helpviewer_keywords:
- Tables collection [ADOX]
ms.assetid: 38d750e7-f3fb-426e-b4b4-55eea4f1a654
author: rothja
ms.author: jroth
ms.openlocfilehash: e30bb754332d52f02d2857dc80d572e227e819ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164003"
---
# <a name="tables-collection-adox"></a>Коллекция Tables (ADOX)
Содержит все [табличные](./table-object-adox.md) объекты каталога.  
  
## <a name="remarks"></a>Замечания  
 Метод [append](./append-method-adox-tables.md) для коллекции **ТАБЛИЦ** уникален для ADOX. Вы можете выполнить следующие действия:  
  
-   Добавьте новую таблицу в коллекцию с помощью метода **append** .  
  
 Остальные свойства и методы являются стандартными для коллекций ADO. Вы можете выполнить следующие действия:  
  
-   Доступ к таблице в коллекции со свойством [Item](../ado-api/item-property-ado.md) .  
  
-   Возвращает количество таблиц, содержащихся в коллекции, с помощью свойства [Count](../ado-api/count-property-ado.md) .  
  
-   Удалите таблицу из коллекции с помощью метода [Delete](./delete-method-adox-collections.md) .  
  
-   Обновите объекты в коллекции, чтобы отразить текущую схему базы данных методом [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Некоторые поставщики могут возвращать другие объекты схемы, например представления, в коллекцию **таблиц** . Таким образом, некоторые коллекции ADOX могут содержать несколько ссылок на один и тот же объект. Если объект удаляется из одной коллекции, это изменение не будет отображаться в другой коллекции, ссылающейся на удаленный объект, до тех пор, пока для коллекции не вызывается метод **Refresh** . Например, при использовании поставщика OLE DB для Microsoft Jet представления возвращаются с помощью коллекции **таблиц** . При удалении представления необходимо обновить коллекцию **Tables** , прежде чем коллекция будет отражать изменение.  
  
 Этот раздел содержит следующий раздел.  
  
-   [Свойства, методы и события коллекции Tables](./tables-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойства ActiveConnection каталога (Visual Basic)](./catalog-activeconnection-property-example-vb.md)   
 [Методы добавления столбцов и таблиц, пример свойства Name (Visual Basic)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Пример метода Close соединения, свойство типа таблицы (Visual Basic)](./connection-close-method-table-type-property-example-vb.md)   
 [Пример свойств для добавления ключей, типа ключа, RelatedColumn, RelatedTable и UpdateRule (Visual Basic)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Объект каталога (ADOX)](./catalog-object-adox.md)   
 [Объект Table (ADOX)](./table-object-adox.md)
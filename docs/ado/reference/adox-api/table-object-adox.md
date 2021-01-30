---
description: Объект Table (ADOX)
title: Объект Table (ADOX) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Table
helpviewer_keywords:
- Table object [ADOX]
ms.assetid: a6d74000-0828-49ba-850a-63da865f8802
author: rothja
ms.author: jroth
ms.openlocfilehash: ba2064096c36bf6490daa8d472801de7594cfdcc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164018"
---
# <a name="table-object-adox"></a>Объект Table (ADOX)
Представляет таблицу базы данных, включая столбцы, индексы и ключи.  
  
## <a name="remarks"></a>Замечания  
 Следующий код создает новую **таблицу**:  
  
```  
Dim obj As New Table  
```  
  
 Свойства и коллекции объекта **Table** позволяют:  
  
-   Найдите таблицу с помощью свойства [Name (ADOX)](./name-property-adox.md) .  
  
-   Определите тип таблицы с помощью свойства [свойства типа (таблица) (ADOX)](./type-property-table-adox.md) .  
  
-   Получите доступ к столбцам базы данных таблицы со коллекцией [Columns Collection (ADOX)](./columns-collection-adox.md) .  
  
-   Получите доступ к индексам таблицы с помощью [коллекции indexes (ADOX)](./indexes-collection-adox.md).  
  
-   Получите доступ к ключам таблицы с помощью [коллекции Keys (ADOX)](./keys-collection-adox.md).  
  
-   Укажите каталог, которому принадлежит таблица со свойством [ParentCatalog (ADOX)](./parentcatalog-property-adox.md) .  
  
-   Возвращает сведения о дате с помощью свойств [DateCreated (ADOX)](./datecreated-property-adox.md) и свойства [DateModified (ADOX)](./datemodified-property-adox.md) .  
  
-   Доступ к зависящим от поставщика свойствам таблицы с помощью коллекции [свойств (ADO)](../ado-api/properties-collection-ado.md) .  
  
> [!NOTE]
>  Поставщик данных может не поддерживать все свойства объектов **Table** . Если задано значение для свойства, которое не поддерживается поставщиком, возникнет ошибка. Для новых объектов **таблицы** ошибка возникает при добавлении объекта в коллекцию. Для существующих объектов при задании свойства возникнет ошибка.  
>   
>  При создании объектов **таблицы** существование соответствующего значения по умолчанию для необязательного свойства не гарантирует, что поставщик поддерживает свойство. Дополнительные сведения о свойствах, поддерживаемых поставщиком, см. в документации поставщика.  
  
 Этот раздел содержит следующий раздел.  
  
-   [Свойства, методы и события объекта Table](./table-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойства ActiveConnection каталога (Visual Basic)](./catalog-activeconnection-property-example-vb.md)   
 [Методы добавления столбцов и таблиц, пример свойства Name (Visual Basic)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Пример метода Close соединения, свойство типа таблицы (Visual Basic)](./connection-close-method-table-type-property-example-vb.md)   
 [Пример свойств для добавления ключей, типа ключа, RelatedColumn, RelatedTable и UpdateRule (Visual Basic)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Пример свойства ParentCatalog (Visual Basic)](./parentcatalog-property-example-vb.md)   
 [Коллекция Columns (ADOX)](./columns-collection-adox.md)   
 [Коллекция indexes (ADOX)](./indexes-collection-adox.md)   
 [Коллекция Keys (ADOX)](./keys-collection-adox.md)   
 [Коллекция Properties (ADO)](../ado-api/properties-collection-ado.md)   
 [Коллекция Tables (ADOX)](./tables-collection-adox.md)
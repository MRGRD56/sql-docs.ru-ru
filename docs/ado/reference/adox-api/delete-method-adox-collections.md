---
description: Метод Delete (коллекции ADOX)
title: Метод Delete (коллекции ADOX) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Views::Delete
- Groups::Delete
- Indexes::raw_Delete
- Columns::raw_Delete
- Tables::Delete
- Keys::Delete
- Users::Delete
- Users::raw_Delete
- Keys::raw_Delete
- Procedures::raw_Delete
- Views::raw_Delete
- Indexes::Delete
- Procedures::Delete
- Groups::raw_Delete
- Tables::raw_Delete
- Columns::Delete
helpviewer_keywords:
- delete method [ADOX]
ms.assetid: e6b6e3a4-8952-4d79-81f4-51019c338374
author: rothja
ms.author: jroth
ms.openlocfilehash: 67d50c34c55543d0ddb870ad6ba5a1fd8cbd5f11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054249"
---
# <a name="delete-method-adox-collections"></a>Метод Delete (коллекции ADOX)
Удаляет объект из коллекции.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
Collection.Delete Name  
```  
  
#### <a name="parameters"></a>Параметры  
 *Имя*  
 **Значение типа Variant** , указывающее имя или порядковый номер объекта, который нужно удалить.  
  
## <a name="remarks"></a>Remarks  
 Если *имя* не существует в коллекции, возникнет ошибка.  
  
 Для коллекций [таблиц](./tables-collection-adox.md) и [пользователей](./users-collection-adox.md) возникнет ошибка, если поставщик не поддерживает удаление таблиц или пользователей соответственно. Для [процедур](./procedures-collection-adox.md) и коллекций [представлений](./views-collection-adox.md) команда **Delete** завершится ошибкой, если поставщик не поддерживает сохранение команд.  
  
## <a name="applies-to"></a>Применение  

:::row:::
    :::column:::
        [Коллекция Columns (ADOX)](./columns-collection-adox.md)  
        [Коллекция Groups (ADOX)](./groups-collection-adox.md)  
        [Коллекция Indexes (ADOX)](./indexes-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Коллекция Keys (ADOX)](./keys-collection-adox.md)  
        [Коллекция Procedures (ADOX)](./procedures-collection-adox.md)  
        [Коллекция Tables (ADOX)](./tables-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Коллекция Users (ADOX)](./users-collection-adox.md)  
        [Коллекция Views (ADOX)](./views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>См. также:  
 [Пример метода Delete процедур (Visual Basic)](./procedures-delete-method-example-vb.md)   
 [Пример метода Delete коллекции Views (Visual Basic)](./views-delete-method-example-vb.md)
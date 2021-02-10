---
description: Свойство Optimize (динамическое) (ADO)
title: Оптимизация Property-Dynamic (ADO) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Optimize property [ADO]
ms.assetid: a491c4ce-2b04-4c84-be83-3846bde8d16b
author: rothja
ms.author: jroth
ms.openlocfilehash: 60fb0e98e2adbd57ad9fa3a98d4d04f7def8d30d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041294"
---
# <a name="optimize-property-dynamic-ado"></a>Свойство Optimize (динамическое) (ADO)
Указывает, должен ли быть создан индекс для [поля](./field-object.md).  
  
## <a name="settings-and-return-values"></a>Параметры и возвращаемые значения  
 Задает или возвращает **логическое** значение, указывающее, должен ли быть создан индекс.  
  
## <a name="remarks"></a>Remarks  
 Индекс может повысить производительность операций, которые находят или сортируют значения в [наборе записей](./recordset-object-ado.md). Индекс является внутренним по отношению к ADO; Вы не можете явно получить доступ к нему или использовать его в своем приложении.  
  
 Чтобы создать индекс для поля, задайте для свойства **optimize** значение **true**. Чтобы удалить индекс, присвойте этому свойству значение **false**.  
  
 **Optimize** — это динамическое свойство, добавленное к коллекции [свойств](./properties-collection-ado.md) объекта [field](./field-object.md) , если свойство [CursorLocation](./cursorlocation-property-ado.md) имеет значение **адусеклиент**.  
  
## <a name="usage"></a>Использование  
  
```  
Dim rs As New Recordset  
Dim fld As Field  
rs.CursorLocation = adUseClient      'Enable index creation  
rs.Fields.Append "Field1", adChar, 35, adFldIsNullable  
rs.Open  
Set fld = rs.Fields(0)  
fld.Properties("Optimize") = True    'Create an index  
fld.Properties("Optimize") = False   'Delete an index  
```  
  
## <a name="applies-to"></a>Применение  
 [Объект Field](./field-object.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример свойства optimize (Visual Basic)](./optimize-property-example-vb.md)   
 [Пример свойства optimize (Visual c++)](./optimize-property-example-vc.md)   
 [Свойство Filter](./filter-property.md)   
 [Метод Find (ADO)](./find-method-ado.md)   
 [Свойство Sort](./sort-property.md)
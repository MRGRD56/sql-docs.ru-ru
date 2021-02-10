---
description: Границы набора записей
title: Границы набора записей | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- EOF property [ADO], boundaries of a Recordset
- Recordset object [ADO], boundaries of a Recordset
- BOF property [ADO], boundaries of a Recordset
ms.assetid: c0dd4a0f-478d-4c5e-b5d5-7535f211d064
author: rothja
ms.author: jroth
ms.openlocfilehash: f737ad11af65045a1d923f427c7487e4b751aaa8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028019"
---
# <a name="boundaries-of-a-recordset"></a>Границы набора записей
**Набор записей** поддерживает свойства **BOF** и **EOF** для отделения начала и конца набора данных соответственно. **BOF** и **EOF** можно считать «фантомными» записями, расположенными в начале и в конце **набора записей**. Учитывая **BOF** и **EOF**, наш пример **набора записей** теперь будет выглядеть следующим образом:  
  
|ProductID|ProductName|UnitPrice|  
|---------------|-----------------|---------------|  
|BOF|||  
|7|Дядюшканые груши Высохнутьы Боба|30,0000|  
|14|тофу|23,2500|  
|28|Рссле квашеной|45,6000|  
|51|Манжимуп высохнуть яблоки|53,0000|  
|74|Лонглифе тофу|10,0000|  
|EOF|||  
  
 Когда курсор перемещается после последней записи, для **EOF** задается **значение true**; в противном случае его значение равно **false**. Аналогично, когда курсор перемещается перед первой записью, **BOF** имеет значение **true**. в противном случае его значение равно **false**. Эти свойства обычно используются для перечисления записей в наборе данных, как показано в следующем фрагменте кода JScript.  
  
```  
while (objRecordset.EOF != true)   
{  
   // Work on the current record.  
   ...  
   // Advance the cursor forward to the next record.  
   objRecordset.MoveNext();  
}  
or  
while (objRecordset.BOF != true)   
{  
   // Work on the current record.  
   ...  
   // Move the cursor to the previous record.  
   objRecordset.MovePrevious();  
}  
```  
  
 Если **BOF** и **EOF** имеют **значение true**, то объект **Recordset** пуст. Оба свойства будут иметь **значение false** для вновь открытого, непустого объекта **Recordset** . Свойства **BOF** и **EOF** можно использовать совместно, чтобы определить, является ли объект **набора записей** пустым, как показано в следующем фрагменте кода JScript.  
  
```  
if (objRecordset.EOF == true && objRecordset.BOF == true)  
{  
   WScript.Echo("we got an empty dataset.");  
}  
else  
{  
   WScript.Echo("we got a full dataset.");  
}  
```  
  
 Эта схема работает для всех типов курсора и не зависит от базовых поставщиков. Если вы попытаетесь определить очистку объекта **набора записей** , проверив, имеет ли его значение свойства **RecordCount** ноль (0), необходимо предпринять меры предосторожности, чтобы использовать соответствующий курсор и поставщик, которые поддерживают возврат количества записей в результате.  
  
 При удалении последней оставшейся записи в объекте **набора записей** курсор остается в неопределенном состоянии. Свойства **BOF** и **EOF** могут оставаться **ложными** , пока вы не попытаетесь Переместить текущую запись в зависимости от поставщика. Дополнительные сведения см. [в разделе Удаление записей с помощью метода Delete](./deleting-records-using-the-delete-method.md).
---
description: ObjectTypeEnum
title: Обжекттипинум | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ObjectTypeEnum
helpviewer_keywords:
- ObjectTypeEnum enumeration [ADOX]
ms.assetid: 3fdecfca-aa91-4596-ad98-610f1b7f840b
author: rothja
ms.author: jroth
ms.openlocfilehash: 9a5ff3f235a01bb84a05b9c4efdff5c1fd15b7c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049894"
---
# <a name="objecttypeenum"></a>ObjectTypeEnum
Указывает тип объекта базы данных, для которого задаются разрешения или владение.  
  
|Константа|Значение|Описание|  
|--------------|-----------|-----------------|  
|**adPermObjColumn**|2|Объект является столбцом.|  
|**адпермобждатабасе**|3|Объект является базой данных.|  
|**адпермобжпроцедуре**|4|Объект является процедурой.|  
|**adPermObjProviderSpecific**|-1|Объект является типом, определенным поставщиком. Если параметр *ObjectType* имеет значение **адпермобжпровидерспеЦифик** и не указан *обжекттипеид* , возникнет ошибка.|  
|**адпермобжтабле**|1|Объект является таблицей.|  
|**adPermObjView**|5|Объект является представлением.|  
  
## <a name="applies-to"></a>Применение  

:::row:::
    :::column:::
        [Метод GetObjectOwner (ADOX)](./getobjectowner-method-adox.md)  
        [Метод GetPermissions (ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [Метод SetObjectOwner](./setobjectowner-method.md)  
        [Метод SetPermissions (ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::
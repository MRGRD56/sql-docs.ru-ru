---
description: ADCPROP_UPDATECRITERIA_ENUM
title: ADCPROP_UPDATECRITERIA_ENUM | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_UPDATECRITERIA_ENUM
helpviewer_keywords:
- ADCPROP_UPDATECRITERIA_ENUM [ADO]
ms.assetid: 33fd7b65-2ec8-4f62-91a7-630b5dab1aa2
author: rothja
ms.author: jroth
ms.openlocfilehash: 2cf53dbf2f1e49bf61420fa99d015932535def3d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031466"
---
# <a name="adcprop_updatecriteria_enum"></a>ADCPROP_UPDATECRITERIA_ENUM
Указывает, какие поля могут использоваться для обнаружения конфликтов во время оптимистического обновления строки источника данных с объектом [Recordset](./recordset-object-ado.md) .  
  
 Используйте эти константы с динамическим свойством "**условие обновления**" **набора записей** , на которое есть ссылка в [индексе динамического свойства ADO](./ado-dynamic-property-index.md) и описанном в документации [службы курсоров Майкрософт для OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) .  
  
|Константа|Значение|Описание|  
|--------------|-----------|-----------------|  
|**адкритериааллколс**|1|Обнаруживает конфликты, если какой бы то ни было столбец в строке источника данных был изменен.|  
|**адкритериакэй**|0|Обнаруживает конфликты, если ключевой столбец строки источника данных был изменен, то есть строка была удалена.|  
|**адкритериатиместамп**|3|Обнаруживает конфликты, если метка времени строки источника данных была изменена, что означает, что доступ к строке осуществлялся после получения **набора записей** .|  
|**адкритериаупдколс**|2|Обнаруживает конфликты, если какие либо столбцы в строке источника данных, соответствующие обновленным полям **набора записей** , были изменены.|  
  
## <a name="adowfc-equivalent"></a>Эквивалент ADO/WFC  
 Пакет: **com. MS. WFC. Data**  
  
|Константа|  
|--------------|  
|AdoEnums.AdcPropUpdateCriteria.ALLCOLS|  
|AdoEnums.AdcPropUpdateCriteria.KEY|  
|Адоенумс. Адкпропупдатекритериа. TIMESTAMP|  
|Адоенумс. Адкпропупдатекритериа. УПДКОЛС|
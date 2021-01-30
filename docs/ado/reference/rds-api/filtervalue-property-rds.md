---
description: Свойство FilterValue (служба удаленных рабочих столов)
title: Свойство FilterValue (RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterValue property [ADO]
ms.assetid: 28f17186-b842-4cf9-b320-a9bb941c481b
author: rothja
ms.author: jroth
ms.openlocfilehash: af9276f641a3a9c419cc0dc49595bcddc049336e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163764"
---
# <a name="filtervalue-property-rds"></a>Свойство FilterValue (служба удаленных рабочих столов)
Указывает значение, с которым фильтруются записи.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
DataControl.FilterValue = String  
```  
  
#### <a name="parameters"></a>Параметры  
 *DataControl*  
 Объектная переменная, представляющая [RDS. Объект элемента управления](./datacontrol-object-rds.md) .  
  
 *String*  
 **Строковое** значение, представляющее значение данных, с помощью которого отфильтруются записи (например, `'Programmer'` или `125` ).  
  
## <a name="remarks"></a>Замечания  
 Свойства [sortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), **FilterValue**, [филтеркритерион](./filtercriterion-property-rds.md)и [филтерколумн](./filtercolumn-property-rds.md) предоставляют функции сортировки и фильтрации кэша на стороне клиента. Функция сортировки упорядочивает записи по значениям из одного столбца. Функция фильтрации отображает подмножество записей на основе критериев поиска, а полный [набор записей](../ado-api/recordset-object-ado.md) сохраняется в кэше. Метод [Reset](./reset-method-rds.md) выполнит условия и заменит текущий **набор** записей на обновляемый **набор записей**.  
  
 Значения NULL приводят к ошибке несоответствия типов.  
  
## <a name="applies-to"></a>Применение  
 [Объект DataControl (служба удаленных рабочих столов)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>См. также:  
 [Примеры свойств Филтерколумн, Филтеркритерион, FilterValue, SortColumn и SortDirection и метода Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Свойство Филтерколумн (RDS)](./filtercolumn-property-rds.md)   
 [Свойство Филтеркритерион (RDS)](./filtercriterion-property-rds.md)   
 [Свойство SortColumn (RDS)](./sortcolumn-property-rds.md)   
 [Свойство SortDirection (служба удаленных рабочих столов)](./sortdirection-property-rds.md)
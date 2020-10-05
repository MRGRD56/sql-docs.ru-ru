---
description: Свойство FilterColumn (служба удаленных рабочих столов)
title: Свойство Филтерколумн (RDS) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- FilterColumn property [ADO]
ms.assetid: 0a5473e8-8ce6-4518-83fb-4920b827e285
author: rothja
ms.author: jroth
ms.openlocfilehash: f4fa38c64a353f6250bc400bf5ed7fc6bf46b95c
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91722231"
---
# <a name="filtercolumn-property-rds"></a>Свойство FilterColumn (служба удаленных рабочих столов)
Указывает столбец, по которому вычисляется критерий фильтра.  
  
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
DataControl.FilterColumn = String  
```  
  
#### <a name="parameters"></a>Параметры  
 *DataControl*  
 Объектная переменная, представляющая [RDS. Объект элемента управления](./datacontrol-object-rds.md) .  
  
 *String*  
 **Строковое** значение, указывающее столбец, по которому вычисляется критерий фильтра. Условия фильтра задаются в свойстве [филтеркритерион](./filtercriterion-property-rds.md) .  
  
## <a name="remarks"></a>Комментарии  
 Свойства [sortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), [филтеркритерион](./filtercriterion-property-rds.md)и **филтерколумн** предоставляют функции сортировки и фильтрации кэша на стороне клиента. Функция сортировки упорядочивает записи по значениям из одного столбца. Функция фильтрации отображает подмножество записей на основе критериев поиска, а полный [набор записей](../ado-api/recordset-object-ado.md) сохраняется в кэше. Метод [Reset](./reset-method-rds.md) выполнит условия и заменит текущий **набор** записей на обновляемый **набор записей**.  
  
## <a name="applies-to"></a>Применение  
 [Объект DataControl (служба удаленных рабочих столов)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>См. также  
 [Примеры свойств Филтерколумн, Филтеркритерион, FilterValue, SortColumn и SortDirection и метода Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Свойство Филтеркритерион (RDS)](./filtercriterion-property-rds.md)   
 [Свойство FilterValue (RDS)](./filtervalue-property-rds.md)   
 [Свойство SortColumn (RDS)](./sortcolumn-property-rds.md)   
 [Свойство SortDirection (служба удаленных рабочих столов)](./sortdirection-property-rds.md)
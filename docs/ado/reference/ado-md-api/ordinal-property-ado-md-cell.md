---
description: Свойство Ordinal (многомерный объект ADO Cell)
title: Свойство Ordinal (объекты данных ActiveX (MD) ячейка) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: a6001168-b954-47f0-ba0d-c05c4cc40c58
author: rothja
ms.author: jroth
ms.openlocfilehash: 4d76ed341fe71f7a5927f6a7e84a63322adcf13f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051045"
---
# <a name="ordinal-property-ado-md-cell"></a>Свойство Ordinal (многомерный объект ADO Cell)
Однозначно определяет [ячейку](./cell-object-ado-md.md) по ее положению в наборе ячеек.  
  
## <a name="return-values"></a>Возвращаемые значения  
 Возвращает **длинное** целое число и доступно только для чтения.  
  
## <a name="remarks"></a>Remarks  
 Порядковое значение ячейки однозначно определяет ячейку в наборе ячеек. По сути, ячейки нумеруются в наборе ячеек, как если *бы набор ячеек* был многомерным массивом, где *p* — это число [осей](./axes-collection-ado-md.md). Нумерация ячеек начинается с нуля в построчном порядке. Ниже приведена формула для вычисления порядкового номера ячейки.  
  
 Порядковое значение ячейки можно использовать со свойством [Item](./item-property-ado-md-cellset.md) объекта набора [ячеек](./cellset-object-ado-md.md) для быстрого извлечения [ячейки](./cell-object-ado-md.md).  
  
## <a name="applies-to"></a>Применение  
 [Объект Cell (многомерные объекты ADO)](./cell-object-ado-md.md)  
  
## <a name="see-also"></a>См. также:  
 [Пример оси (VBScript)](./axis-example-vbscript.md)   
 [Объект набора ячеек (объекты данных ActiveX (MD))](./cellset-object-ado-md.md)   
 [Свойство Item (объекты данных ActiveX (MD) набор ячеек)](./item-property-ado-md-cellset.md)   
 [Свойство Ordinal (многомерный объект ADO Position)](./ordinal-property-ado-md-position.md)
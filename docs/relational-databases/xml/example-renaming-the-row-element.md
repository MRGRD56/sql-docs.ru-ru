---
title: Пример Переименование элемента &lt;row&gt; | Документация Майкрософт
description: Обзор примера переименования элемента строки XML путем указания необязательного аргумента в режиме RAW в предложении FOR XML.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- RAW mode, renaming <row> example
ms.assetid: b042292a-0b6e-40a3-b254-71c06e626706
author: rothja
ms.author: jroth
ms.openlocfilehash: eaaf500022ffc89460a76cacb7e77e12ed10c396
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488631"
---
# <a name="example-renaming-the-ltrowgt-element"></a>Пример Переименование элемента &lt;row&gt;
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Для каждой строки результирующего набора режим RAW создает элемент `<row>`. При необходимости можно задать другое имя для этого элемента путем определения дополнительного аргумента в режиме RAW, как показано в данном запросе. Запрос возвращает элемент <`ProductModel`> для каждой строки из набора строк.  
  
## <a name="example"></a>Пример  
  
```  
SELECT ProductModelID, Name   
FROM Production.ProductModel  
WHERE ProductModelID=122  
FOR XML RAW ('ProductModel'), ELEMENTS  
GO  
```  
  
 Результат. Из-за того, что в запрос добавлена директива `ELEMENTS` , результат состоит из элементов.  
  
```  
<ProductModel>  
  <ProductModelID>122</ProductModelID>  
  <Name>All-Purpose Bike Stand</Name>  
</ProductModel>   
```  
  
## <a name="see-also"></a>См. также:  
 [Использование RAW Mode с FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)  
  
  

---
description: Пример набора записей для проверки данных
title: Пример набора записей для проверки данных | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- record location [ADO]
- current record [ADO]
ms.assetid: e770e626-68b1-4ddf-a217-d7b30311e2ee
author: rothja
ms.author: jroth
ms.openlocfilehash: baea4d79057a46ec26c6ccd82f834f5fa3481091
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037074"
---
# <a name="sample-recordset-for-examining-data"></a>Пример набора записей для проверки данных
Сначала рассмотрим объект **Recordset** , возвращенный с помощью следующего запроса SQL, выполненного для базы данных "Northwind Sample Data" в Microsoft SQL Server.  
  
```  
SELECT ProductID,ProductName,UnitPrice   
FROM Products   
WHERE CategoryID = 7    
```  
  
 Результирующий объект **набора записей** содержит все объекты, которые создает в базе данных, показанной в следующей таблице.  
  
|ProductID|ProductName|UnitPrice|  
|---------------|-----------------|---------------|  
|7|Дядюшканые груши Высохнутьы Боба|30,0000|  
|14|тофу|23,2500|  
|28|Рссле квашеной|45,6000|  
|51|Манжимуп высохнуть яблоки|53,0000|  
|74|Лонглифе тофу|10,0000|  
  
 Если вы хотите получить эти результаты самостоятельно, попробуйте следующий пример в JScript:  
  
-   [Пример JScript для возврата набора записей](../../../ado/guide/data/jscript-code-example-to-return-a-recordset.md)

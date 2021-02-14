---
title: Функция Max (XQuery) | Документация Майкрософт
description: Сведения о функции Max () XQuery, возвращающей один элемент в последовательности, значение которой больше значения всех остальных.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- max function [XQuery]
- fn:max function
ms.assetid: 5ee625c0-044a-4cda-b210-02b64e619d65
author: rothja
ms.author: jroth
ms.openlocfilehash: b52ae499cf09e8fcd024f592ae639d0f9a7c1c5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341961"
---
# <a name="aggregate-functions---max"></a>Агрегатные функции — max
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Возвращает из последовательности атомарных значений, *$arg* один элемент, значение которого больше всех остальных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
fn:max($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Аргументы  
 *$arg*  
 Последовательность атомарных значений, из которой требуется вернуть максимальное значение.  
  
## <a name="remarks"></a>Remarks  
 Все типы атомарных значений, передаваемых в **Max ()** , должны быть подтипами одного и того же базового типа. Допустимые базовые типы — это типы, поддерживающие операцию **gt** . Эти типы включают в себя три встроенных базовых численных типа, базовые типы даты-времени, а также xs:string, xs:boolean, xdt:untypedAtomic. Значения типа xdt:untypedAtomic приводятся к типу xs:double. Если существует смесь этих типов или передаются другие значения других типов, возникает статическая ошибка.  
  
 Результат **Max ()** получает базовый тип переданных типов, например xs: Double в случае xdt: untypedAtomic. Если вход статически пуст, подразумевается пустое значение, и формируется статическая ошибка.  
  
 Функция **Max ()** возвращает одно значение в последовательности, которое больше, чем любое другое во входной последовательности. Для значений xs:string используются параметры сортировки кодовых точек Юникода по умолчанию. Если значение xdt: untypedAtomic не может быть приведено к типу xs: Double, оно игнорируется во входной последовательности *$arg*. Если вход — это динамически вычисляемая пустая последовательность, возвращается пустая последовательность.  
  
## <a name="examples"></a>Примеры  
 В этом разделе приведены примеры запросов XQuery к экземплярам XML, хранящимся в различных столбцах типа **XML** в [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] базе данных.  
  
### <a name="a-using-the-max-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-that-have-the-most-labor-hours"></a>A. Использование XQuery-функции max() для поиска в производственном процессе цехов с наибольшим количеством рабочих часов  
 Запрос, предоставленный в [функции min (XQuery)](../xquery/aggregate-functions-min.md) , может быть переписан для использования функции **Max ()** .  
  
## <a name="implementation-limitations"></a>Ограничения реализации  
 Существуют следующие ограничения:  
  
-   Функция **Max (**) сопоставляет все целые числа с xs: Decimal.  
  
-   Функция **Max ()** для значений типа xs: Duration не поддерживается.  
  
-   не поддерживаются последовательности, в которых смешиваются типы на основе разных базовых типов;  
  
-   Синтаксический параметр для указания параметров сортировки не поддерживается.  
  
## <a name="see-also"></a>См. также:  
 [Функции XQuery для типа данных xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  

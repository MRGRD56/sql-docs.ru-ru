---
title: Функция AVG (XQuery) | Документация Майкрософт
description: Подробнее о функции XQuery avg (), возвращающей среднее значение указанной последовательности чисел.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:avg function
- avg function [XQuery]
ms.assetid: 0cc60267-3c56-4a88-8ad7-bb07f0255d56
author: rothja
ms.author: jroth
ms.openlocfilehash: ee09ab89e1acb0fc8ddad23d002e2cd0e136bc59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340494"
---
# <a name="aggregate-functions---avg"></a>Агрегатные функции — avg
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Возвращает среднее значение для последовательности чисел.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
fn:avg($arg as xdt:anyAtomicType*) as xdt:anyAtomicType?  
```  
  
## <a name="arguments"></a>Аргументы  
 *$arg*  
 Последовательность атомарных значений, для которых вычисляется среднее значение.  
  
## <a name="remarks"></a>Remarks  
 Все типы разделенных значений, которые передаются в **AVG ()** , должны быть подтипами только одного из трех встроенных числовых базовых типов или xdt: untypedAtomic. Они не могут быть смешанными. Значения типа xdt:untypedAtomic приводятся к типу xs:double. Результат функции **AVG ()** получает базовый тип переданных типов, например xs: Double в случае xdt: untypedAtomic.  
  
 Если вход статически пуст, подразумевается пустое значение, и формируется статическая ошибка.  
  
 Функция **AVG ()** Возвращает среднее арифметическое вычисленных чисел. Пример:  
  
 **Sum (** *$arg* **) число Div (** *$arg* **)**  
  
 Если *$arg* является пустой последовательностью, возвращается пустая последовательность.  
  
 Если значение xdt: untypedAtomic не может быть приведено к типу xs: Double, оно не учитывается во входной последовательности *$arg*.  
  
 Во всех прочих случаях функция возвращает статическую ошибку.  
  
## <a name="examples"></a>Примеры  
 В этом разделе приведены примеры запросов XQuery к экземплярам XML, хранящимся в различных столбцах типа **XML** в базе данных AdventureWorks.  
  
### <a name="a-using-the-avg-xquery-function-to-find-work-center-locations-in-the-manufacturing-process-in-which-labor-hours-are-greater-than-the-average-for-all-work-center-locations"></a>A. Использование функции XQuery avg() для поиска на производстве расположения цехов, время работы в которых превышает среднее значение для всех цехов.  
 Можно переписать запрос, предоставленный [функцией min (XQuery)](../xquery/aggregate-functions-min.md) , для использования функции **AVG ()** .  
  
## <a name="implementation-limitations"></a>Ограничения реализации  
 Существуют следующие ограничения:  
  
-   Функция **AVG ()** сопоставляет все целые числа с xs: Decimal.  
  
-   Функция **AVG ()** для значений типа xs: Duration не поддерживается.  
  
-   не поддерживаются последовательности, в которых смешиваются типы на основе разных базовых типов;  
  
## <a name="see-also"></a>См. также:  
 [Функции XQuery для типа данных xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  

---
title: Функция RunningValue (построитель отчетов) | Документация Майкрософт
description: Узнайте о функции RunningValue, которая возвращает текущий агрегат всех числовых значений, отличных от NULL, заданных выражением в построителе отчетов.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 6bee2f15-0e69-49c8-9689-b04544063b1d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 75b25a7cb08a7473e6a725a841e53011c6967756
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596022"
---
# <a name="report-builder-functions---runningvalue-function"></a>Функции построителя отчетов — функция RunningValue
  Возвращает текущий агрегат всех числовых значений, отличных от NULL, заданных выражением, вычисляемым для данной области.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
RunningValue(expression, function, scope)  
```  
  
#### <a name="parameters"></a>Параметры  
 *expression*  
 Выражение, к которому применяется статистическая обработка, например `[Quantity]`.  
  
 *function*  
 (**Enum**) Имя агрегата функции для применения к выражению, например **Sum**. Этой функцией не может быть **RunningValue**, **RowNumber** или **Aggregate**.  
  
 *область*  
 (**Enum**) Строковая константа, являющаяся именем набора данных, региона данных или группы или значением null (**Nothing** в [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]), которая определяет контекст, в котором рассчитывается агрегат. Значение **Nothing** указывает самый внешний контекст, обычно набор данных отчета.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 Определяется агрегатной функцией, указанной параметром *function* .  
  
## <a name="remarks"></a>Remarks  
 Это значение для функции **RunningValue** сбрасывается в 0 для каждого нового экземпляра этой области. Если указано группирование, то текущее значение сбрасывается при изменении выражения группы. Если указана область данных, то текущее значение сбрасывается для каждого нового экземпляра области данных. Если указан набор данных, то текущее значение не сбрасывается по всему набору данных.  
  
 Функция **RunningValue** не может быть использована в выражении фильтра или сортировки.  
  
 Набор данных, для которого вычислено текущее значение, должен иметь такой же тип данных. Чтобы преобразовать данные, имеющие разные числовые типы, к одному и тому же типу, используйте функции преобразования, такие как **CInt**, **CDbl** или **CDec**. Дополнительные сведения см. в разделе [Функции преобразования типов](/dotnet/visual-basic/language-reference/functions/type-conversion-functions).  
  
 Значением *Scope* не может быть выражение.  
  
 *Expression* может содержать вызовы вложенных агрегатных функций со следующими условиями и исключениями.  
  
-   Область для вложенных агрегатов должна совпадать с областью внешнего агрегата или входить в нее. Одна область из всех уникальных областей в выражении должна быть дочерней относительно всех других областей.  
  
-   Область для вложенных агрегатов не может быть именем набора данных.  
  
-   *Expression* не может содержать функции **First**, **Last**, **Previous** и **RunningValue** .  
  
-   *Expression* не может содержать вложенные агрегаты, в которых указан параметр *recursive*.  
  
 Для вычисления текущего значения числа строк используйте функцию **RowNumber**. Дополнительные сведения см. в разделе [Функция RowNumber (построитель отчетов и службы SSRS)](../../reporting-services/report-design/report-builder-functions-rownumber-function.md).  
  
 Дополнительные сведения см. в разделах [Справочник по агрегатным функциям (построитель отчетов и службы SSRS)](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md) и [Область выражения для суммирования, агрегатных функций и встроенных коллекций (построитель отчетов и службы SSRS)](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
 Дополнительные сведения о рекурсивных агрегатах см. в разделе [Создание групп рекурсивной иерархии (построитель отчетов и службы SSRS)](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md).  
  
## <a name="examples"></a>Примеры  
 Следующий пример кода возвращает текущую сумму поля стоимости `Cost` в самой внешней области данных, которой является набор данных.  
  
```  
=RunningValue(Fields!Cost.Value, Sum, Nothing)  
```  
  
 Следующий пример кода выдает сумму с накоплением поля с названием `Score` в наборе данных с названием `DataSet1`.  
  
```  
=RunningValue(Fields!Score.Value,sum,"DataSet1")  
```  
  
 Следующий пример кода выдает сумму с накоплением поля с названием `Traffic Charges` во внешней области видимости.  
  
```  
=RunningValue(Fields!Traffic Charges.Value, Sum, Nothing)  
```  
  
## <a name="see-also"></a>См. также:  
 [Использование выражений в отчетах (построитель отчетов и службы SSRS)](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Примеры выражений (построитель отчетов и службы SSRS)](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Типы данных в выражениях (построитель отчетов и службы SSRS)](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Область выражения для суммирования, агрегатных функций и встроенных коллекций (построитель отчетов и службы SSRS)](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)  
  

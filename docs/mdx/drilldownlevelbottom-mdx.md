---
description: DrilldownLevelBottom (многомерные выражения)
title: DrilldownLevelBottom (многомерные выражения) | Документация Майкрософт
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 6f761c0af97cf6a9d2a5dda4e26ac2e4503a8b4b
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92193996"
---
# <a name="drilldownlevelbottom-mdx"></a>DrilldownLevelBottom (многомерные выражения)


  Детализирует углублением самые нижние элементы набора на указанном уровне и одним уровнем ниже.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
DrilldownLevelBottom(Set_Expression, Count [,[<Level_Expression>] [,[<Numeric_Expression>][,INCLUDE_CALC_MEMBERS]]])  
```  
  
## <a name="arguments"></a>Аргументы  
 *Set_Expression*  
 Допустимое многомерное выражение, возвращающее набор.  
  
 *Count*  
 Допустимое числовое выражение, указывающее количество возвращаемых кортежей.  
  
 *Level_Expression*  
 Допустимое многомерное выражение, возвращающее уровень.  
  
 *Numeric_Expression*  
 Необязательный элемент. Допустимое числовое выражение (обычно многомерное выражение координат ячейки), возвращающее число.  
  
 *Include_Calc_Members*  
 Необязательный элемент. Ключевое слово, которое добавляет вычисляемые элементы в результаты углубленной детализации.  
  
## <a name="remarks"></a>Remarks  
 Если числовое выражение указано, функция **DrilldownLevelBottom** сортирует в возрастающем порядке дочерние элементы каждого элемента в указанном наборе согласно указанному значению, вычисленному по набору дочерних элементов. Если числовое выражение не задано, эта функция сортирует в порядке возрастания дочерние элементы каждого элемента в указанном наборе согласно значениям ячеек, представленным набором дочерних элементов, в соответствии с определением в контексте запроса. Это поведение аналогично поведению функций многомерных выражений BottomCount и Tail, которые возвращают набор элементов в естественном порядке без какой-либо сортировки.  
  
 После сортировки функция **DrilldownLevelBottom** возвращает набор, содержащий родительские элементы и количество дочерних элементов, указанное в параметре *Count*, с наименьшим значением.  
  
 Функция **DrilldownLevelBottom** похожа на функцию [DrilldownLevel](../mdx/drilldownlevel-mdx.md) , но вместо включения всех дочерних элементов для каждого элемента на указанном уровне функция **DrilldownLevelBottom** Возвращает самое нижнее число дочерних элементов.  
  
 Запрос свойства XMLA Мдпропмдксдриллфунктионс позволяет проверить уровень поддержки, предоставляемый сервером для функций сверления; Дополнительные сведения см. в разделе [Поддерживаемые свойства xmla &#40;&#41;XMLA ](/analysis-services/xmla/xml-elements-properties/propertylist-element-supported-xmla-properties) .  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются три нижних потомка уровня категории продуктов согласно мере по умолчанию. В примере куба Adventure Works три нижних потомка для Accessories являются Tires и Tubes, Pumps и Panniers. В Management Studio в окне запроса MDX можно перейти в раздел Продукты | Категории продуктов | Элементы | Все продукты | Аксессуары, чтобы просмотреть весь список. Вы можете увеличить аргумент счетчика, чтобы вернуть больше элементов.  
  
```  
SELECT DrilldownLevelBottom   
   ([Product].[Product Categories].children,  
   3,  
   [Product].[Product Categories].[Category])  
   ON 0  
   FROM [Adventure Works]  
```  
  
 В следующем примере показано использование флага **include_calc_members** , который используется для включения вычисляемых элементов в уровень детализации. Мера [число заказов торгового посредника] добавляется в инструкцию **DrilldownLevelBottom** , чтобы обеспечить сортировку результатов по мере. Чтобы увидеть вычисляемый элемент, необходимо увеличить счетчик по крайней мере до 9.  
  
```  
WITH MEMBER   
[Product].[Product Categories].[Category].&[3].[Premium Clothes] AS  
[Product].[Product Categories].[Subcategory].&[18] +  
[Product].[Product Categories].[Subcategory].&[21]  
SELECT [Measures].[Reseller Order Count] ON 0,  
DRILLDOWNLEVELBOTTOM(  
  [Product].[Product Categories].children ,  
  9,  
  [Product].[Product Categories].[Category] ,  
  [Measures].[Reseller Order Count],  
  INCLUDE_CALC_MEMBERS ) ON 1  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>См. также:  
 [DrilldownLevel &#40;&#41;многомерных выражений ](../mdx/drilldownlevel-mdx.md)   
 [Справочник по функциям многомерных выражений (многомерные выражения)](../mdx/mdx-function-reference-mdx.md)  
  

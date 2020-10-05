---
description: PredictCaseLikelihood (расширения интеллектуального анализа данных)
title: PredictCaseLikelihood (расширения интеллектуального анализа данных) | Документация Майкрософт
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: a08ec4809b281205386abad429afe339e0dbb1d1
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91727715"
---
# <a name="predictcaselikelihood-dmx"></a>PredictCaseLikelihood (расширения интеллектуального анализа данных)
[!INCLUDE[ssas](../includes/applies-to-version/ssas.md)]

  Эта функция возвращает правдоподобие того, что входной вариант попадет в существующую модель. Используется только с моделями кластеризации.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
PredictCaseLikelihood([NORMALIZED|NONNORMALIZED])  
```  
  
## <a name="arguments"></a>Аргументы  
 NORMALIZED  
 Возвращаемое значение содержит отношение вероятности варианта в рамках модели к вероятности нахождения варианта вне модели.  
  
 NONNORMALIZED  
 Возвращаемое значение содержит необработанное значение вероятности варианта, представляющее собой произведение вероятностей атрибутов варианта.  
  
## <a name="applies-to"></a>Применение  
 Модели, построенные с использованием [!INCLUDE[msCoName](../includes/msconame-md.md)] алгоритмов кластеризации и [!INCLUDE[msCoName](../includes/msconame-md.md)] кластеризации последовательностей.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 Число с плавающей запятой двойной точности в диапазоне от 0 до 1. Число, более близкое к 1, обозначает большую вероятность вхождения варианта в модель; число, близкое к 0, обозначает меньшую вероятность вхождения варианта в модель.  
  
## <a name="remarks"></a>Комментарии  
 По умолчанию результат функции **PredictCaseLikelihood** нормализован. Нормализованные значения, как правило, более эффективны, поскольку в варианте увеличивается число атрибутов и разница между необработанными вероятностями двух вариантов существенно уменьшается.  
  
 Следующее уравнение используется для вычисления нормализованных значений при заданных x и y:  
  
-   x = вероятность варианта на основе модели кластеризации;  
  
-   y = вероятность граничного варианта, вычисленная как логарифм правдоподобия варианта на основе подсчета количества обучающих вариантов.  
  
-   Z = exp (log (x) — log (Y))  
  
 Нормализованные = (z/(1 + z))  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращается правдоподобие вхождения указанного варианта в модель кластеризации, основанную на базе данных [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] DW.  
  
```  
SELECT  
  PredictCaseLikelihood() AS Default_Likelihood,  
  PredictCaseLikelihood(NORMALIZED) AS Normalized_Likelihood,  
  PredictCaseLikelihood(NONNORMALIZED) AS Raw_Likelihood,  
FROM  
  [TM Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
  '2-5 Miles' AS [Commute Distance],  
  'Graduate Degree' AS [Education],  
  0 AS [Number Cars Owned],  
  0 AS [Number Children At Home]) AS t  
```  
  
 Ожидаемый результат:  
  
|Default_Likelihood|Normalized_Likelihood|Raw_Likelihood|  
|-------------------------|----------------------------|---------------------|  
|6.30672792729321E-08|6.30672792729321E-08|9.5824454056846E-48|  
  
 Разница между результатами демонстрирует влияние нормализации. Необработанное значение для **каселикелихуд** предполагает, что вероятность варианта составляет примерно 20 процентов; Однако при нормализации результатов становится очевидно, что вероятность варианта очень мала.  
  
## <a name="see-also"></a>См. также  
 [Алгоритмы интеллектуального анализа данных &#40;Analysis Services — интеллектуальный анализ данных&#41;](/analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining)   
 [Расширения интеллектуального анализа данных &#40;Справочник по функциям DMX&#41;](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Функции &#40;&#41;расширений интеллектуального анализа данных ](../dmx/functions-dmx.md)   
 [Общие прогнозирующие функции &#40;&#41;расширений интеллектуального анализа данных ](../dmx/general-prediction-functions-dmx.md)  
  

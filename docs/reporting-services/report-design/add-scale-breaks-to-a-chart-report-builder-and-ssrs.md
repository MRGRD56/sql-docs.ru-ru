---
title: Добавление в диаграмму разрывов шкалы (построитель отчетов) | Документация Майкрософт
description: Сведения о том, как использовать разрыв шкалы, чтобы отобразить два разных диапазона в одной и той же области диаграммы в построителе отчетов.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 84d66436-ed62-4967-bbbd-b457593ee787
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c83ef4812dbd28da971877cf90b1b4e8406af272
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935256"
---
# <a name="add-scale-breaks-to-a-chart-report-builder-and-ssrs"></a>Добавление в диаграмму разрывов шкалы (построитель отчетов и службы SSRS)

  Разрыв шкалы представляет собой полосу, нарисованную в области построения диаграммы, обозначающую нарушение непрерывной последовательности изменения значений от малых к большим на оси значений (обычно это вертикальная ось, или ось Y). Разрыв шкалы используется, чтобы отобразить два разных диапазона в одной и той же области диаграммы.  
  
 ![Диаграмма с разрывом шкалы](../../reporting-services/report-design/media/rs-multipledatarangeschart-scalebreak.gif "Диаграмма с разрывом шкалы")  
  
> [!NOTE]  
>  Положение разрыва шкалы указать на диаграмме нельзя. Диаграмма на основе значений набора данных самостоятельно вычисляет, где имеется значительный разрыв между диапазонами данных, чтобы поместить разрыв шкалы на оси значений (оси Y) во время выполнения.  
  
 Пример диаграммы с разрывами шкалы доступен в виде образца отчета. Дополнительные сведения о скачивании этого и других примеров отчетов см. в статье [Report Builder and Report Designer sample reports](https://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-enable-scale-breaks-on-the-chart"></a>Включение разрывов шкалы на диаграмме  
  
1.  Щелкните правой кнопкой мыши вертикальную ось и выберите пункт **Свойства оси**. Откроется диалоговое окно **Свойства вертикальной оси** .  
  
2.  Установите флажок **Включить разрывы шкалы** .  
  
### <a name="to-change-the-style-of-the-scale-break"></a>Изменение стиля разрыва шкалы  
  
1.  Откройте панель «Свойства».  
  
2.  В области конструктора щелкните правой кнопкой мыши ось Y диаграммы. На панели «Свойства» будут отображены свойства объекта оси Y (по умолчанию называемой «Ось диаграммы»).  
  
3.  В разделе **Шкала** раскройте свойство ScaleBreakStyle.  
  
4.  Измените значения свойства ScaleBreakStyle, такие как BreakLineType и Spacing. Дополнительные сведения о свойствах разрыва шкалы см. в разделе [Отображение на диаграмме ряда с несколькими диапазонами данных (построитель отчетов и службы SSRS)](../../reporting-services/report-design/displaying-a-series-with-multiple-data-ranges-on-a-chart.md).  

## <a name="next-steps"></a>Дальнейшие действия

[Диаграммы](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Форматирование диаграммы](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[Диалоговое окно "Свойства оси" — "Параметры оси"](/previous-versions/sql/)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
---
description: Синтаксис литерала интервала
title: Синтаксис литерала Interval | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- literals [ODBC], interval
- interval literals [ODBC]
- ODBC literals [ODBC], interval
ms.assetid: 2f2d22c1-51d6-4055-9f5a-53bc31e9fea0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5a58ddbde1be09f16fec79fcff13147a805febfd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208524"
---
# <a name="interval-literal-syntax"></a>Синтаксис литерала интервала
Для литералов интервала в ODBC используется следующий синтаксис.  
  
 Interval *-литерал:: = Interval* [+*&#124;*-] интервал — *интервал строкового квалификатора*  
  
 *интервал-строка* :: = *кавычка* { *year-month-литерал* &#124; *день-время-литерал* } *кавычка*  
  
 *год-месяц-литерал* :: = *years-value* &#124; [*years-value* -] *месяцы-value*  
  
 *день-время-литерал* :: = *день-время-интервал* &#124; интервал *времени*  
  
 *день-время-интервал* :: = *дни — значение* [*hours-value* [:*минуты-значение*[:*Seconds-value*]]]  
  
 *интервал времени* :: = *hours-value* [:*минуты-значение* [:*секунды-значение* ]]  
  
 &#124; *минут-значение* [:*секунды-значение* ]  
  
 &#124; *секунды — значение*  
  
 *years — значение* :: = *DateTime-value*  
  
 *месяцы-значение* :: = *DateTime-value*  
  
 *Days — значение* :: = *DateTime-value* .  
  
 *hours — значение* :: = *DateTime-value* .  
  
 *минуты-значение* :: = *DateTime-value*  
  
 *секунды-value* :: = *Seconds-Integer-value* [. [ *секунды-дробь*] ]  
  
 *секунды-целое число — значение* :: = *без знака — целое число*  
  
 *секунды-дробь* :: = *без знака — целое число*  
  
 *DateTime-value* :: = *без знака — целое число*  
  
 *квалификатор интервала* :: = *Начало-поле* в *конец поля* &#124; *единичное значение DateTime-Field*  
  
 *Start-Field* :: = *не второй-DateTime-Field* [(*интервал — начальное поле-точность* )]  
  
 *конец поля* :: = *не второе — datetime-Field* &#124; секунды [(*Interval-долей-Seconds-Precision*)]  
  
 *Single-DateTime-Field* :: = *не второй-DateTime-Field* [(*интервал — начальное поле*)] &#124; Second [(*интервал с начальным полем и точностью* [, (*Interval-долей-секунд-точность*)]  
  
 *DateTime-Field* :: = *не второе — datetime-Field* &#124; секунды  
  
 *не второй-DateTime — поле* :: = год &#124; месяц &#124; день &#124; час &#124; минуте  
  
 *Interval-долей-секунды-точность* :: = *без знака — целое число*  
  
 *Interval-в начале поля-точность* :: = *без знака — целое число*  
  
 *quote* :: = '  
  
 *без знака — целое число* :: = *цифра...*

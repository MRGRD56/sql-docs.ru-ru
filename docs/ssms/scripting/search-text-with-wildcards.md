---
title: Поиск текста с символами-шаблонами
description: Узнайте, как использовать подстановочные знаки в поле "Найти" диалогового окна "Найти и заменить", чтобы указать шаблон для сопоставления.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- vswildcardsbuilder
helpviewer_keywords:
- searches [SQL Server Management Studio], wildcards
- Query Editor [SQL Server Management Studio], wildcard searches
- wildcard options [SQL Server Management Studio]
ms.assetid: 449600f8-cc87-4b3f-878a-59c158a88a40
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 670b276a54796862b68b7aa49aef5145c508b317
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344163"
---
# <a name="search-text-with-wildcards"></a>Поиск текста с символами-шаблонами
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  С помощью приведенных ниже выражений можно заменять символы либо цифры в поле **Найти** в диалоговом окне **Найти и заменить** в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-search-using-wildcards"></a>Осуществление поиска с шаблонами  
  
1.  Чтобы включить использование шаблонов в поле **Найти** во время операций «Быстрый поиск», **Найти в файлах**, **Быстрая замена** или **Заменить в файлах** , выберите **Использовать** в области **Параметры поиска** , а затем установите флажок **Подстановочные знаки**.  
  
2.  В этом случае становится доступной треугольная кнопка **Список ссылок** , расположенная рядом с полем **Найти** . Нажмите эту кнопку, чтобы отобразить список доступных шаблонов. При выборе любого элемента в **Списке подстановки** он будет вставлен в строку **Найти** .  
  
 Следующая таблица содержит описание шаблонов, доступных через **Список подстановки**.  
  
|Выражение|Синтаксис|Описание|  
|----------------|------------|-----------------|  
|Любой отдельный символ|?|Соответствует любому отдельному символу.|  
|Любая отдельная цифра|#|Соответствует любой отдельной цифре. Например, выражение 7# соответствует числам, начинающимся с 7 и содержащим еще одну цифру, например 71, но не 17.|  
|Символы вне набора|[! ]|Соответствует любому символу, кроме заданных в наборе.|  
|Один или несколько символов|*|Соответствует одному или нескольким символам. Например, выражение new* соответствует любому тексту, включающему подстроку «new», например newfile.txt.|  
|Набор символов|[ ]|Соответствует любому из символов, заданных в наборе.|  
  
## <a name="see-also"></a>См. также:  
 [Поиск и замена](./search-and-replace.md)   
 [Поиск текста с помощью регулярных выражений](./search-text-with-regular-expressions.md)
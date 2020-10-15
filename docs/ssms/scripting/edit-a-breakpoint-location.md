---
title: Изменение положения точки останова
description: Сведения о том, как переместить точку останова в файле скрипта Transact-SQL в другое место того же скрипта или в другой скрипт.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint location
ms.assetid: 5c28e411-0377-4210-a7ce-2a5c13dcdf74
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22e94d5bd53287600458504834321106fa22a1ce
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92036472"
---
# <a name="edit-a-breakpoint-location"></a>Изменение положения точки останова

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Положение точки останова задает строку и символ, где находится точка останова в файле скрипта [!INCLUDE[tsql](../../includes/tsql-md.md)] . Положение точки останова можно изменить и перенести точку останова в другое место в скрипте или в другой скрипт.

## <a name="editing-a-location"></a>Изменение положения

При изменении положения точки останова она перемещается в новое место вместе со всеми существующими свойствами, такими как число попаданий или условие.  

#### <a name="to-edit-a-breakpoint-location"></a>Изменение положения точки останова

1. В окне редактора щелкните метку точки останова правой кнопкой мыши и выберите в контекстном меню пункт **Положение** .  
  
     -или-  
  
     В окне **Точки останова** щелкните метку точки останова правой кнопкой мыши и выберите в контекстном меню пункт **Положение** .  
  
2. В диалоговом окне **Точка останова в файле** в поле **Файл** укажите новый файл, в поле **Строка** укажите новую строку, а в поле **Символ** укажите новое расположение в строке. Если новый указанный файл уже открыт в окне редактора запросов, точка останова перемещается в это окно редактора. Если файл не открыт, открывается новое окно редактора, в него загружается указанный файл и точка останова перемещается в новое расположение.  
  
     При отладке **параметр** Разрешить наличие отличий в исходном коде от первоначальной версии [!INCLUDE[tsql](../../includes/tsql-md.md)]не учитывается.  
  
## <a name="see-also"></a>См. также:

- [Настройка счетчика числа попаданий](./specify-a-hit-count.md)
- [Задание действия в точке останова](./specify-a-breakpoint-action.md)
- [Задание условия точки останова](./specify-a-breakpoint-condition.md)
- [Задание фильтра точек останова](./specify-a-breakpoint-filter.md)
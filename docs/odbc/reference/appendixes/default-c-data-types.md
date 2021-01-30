---
description: Типы данных C по умолчанию
title: Типы данных C по умолчанию | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], pseudo-type identifiers
- pseudo-type identifiers [ODBC], about pseudo-type identifiers
- pseudo-type identifiers [ODBC]
ms.assetid: 229140ae-af8f-4ec8-9ccf-1e92360e0bac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fd7f50e58fd84cec5938aacc598d12e6d733fcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194896"
---
# <a name="default-c-data-types"></a>Типы данных C по умолчанию
Если приложение указывает SQL_C_DEFAULT в **SQLBindCol**, **SQLGetData** или **SQLBindParameter**, драйвер предполагает, что тип данных C выходного или входного буфера соответствует типу данных SQL столбца или параметра, к которому привязан буфер.  
  
> [!IMPORTANT]  
>  В приложениях, поддерживающих взаимодействие, не следует использовать SQL_C_DEFAULT. Вместо этого они всегда должны указывать тип C буфера, который они используют. Это обусловлено тем, что драйверы не всегда могут правильно определить тип C по умолчанию по следующим причинам.  
  
-   Если СУБД додвигает тип данных SQL для столбца или параметра, драйвер не может определить исходный тип данных SQL для столбца или параметра. Поэтому он не может определить соответствующий тип данных C по умолчанию.  
  
-   Если драйвер не может определить, подписан ли определенный столбец или параметр, как правило, если он обрабатывается СУБД, драйвер не может определить, должен ли соответствующий тип данных C по умолчанию быть знаком или без знака.  
  
     Поскольку SQL_C_DEFAULT предоставляется только в качестве удобства программирования, приложение не теряет никаких функциональных возможностей, если оно указывает фактический тип данных C.  
  
 Таблица, показывающая тип данных C по умолчанию для каждого типа данных SQL, включается в [Преобразование данных из SQL в типы данных C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)в дальнейшем в этом приложении.

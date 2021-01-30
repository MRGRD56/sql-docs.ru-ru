---
description: Ограничения инструкции CREATE INDEX
title: Ограничения инструкции CREATE INDEX | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE INDEX statement limitations [ODBC]
- ODBC SQL grammar, CREATE INDEX statement limitations
ms.assetid: 832dcda1-e452-48e6-8adb-7fb33c4fb4ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60952e7b9236c392e10bc155a19c5b147e011dbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206166"
---
# <a name="create-index-statement-limitations"></a>Ограничения инструкции CREATE INDEX
Инструкция CREATE INDEX не поддерживается для Microsoft Excel или текстовых драйверов.  
  
 Индекс может быть определен не более чем в 10 столбцах. Если в инструкцию CREATE INDEX включены более 10 столбцов, индекс не будет распознан, а таблица будет обрабатываться так, как будто индекс не был создан.  
  
 Драйверу dBASE не удается создать индекс для логического столбца.  
  
 При использовании драйвера dBASE время отклика на большие файлы можно улучшить, создав индекс многомерных выражений (или ндкс) в столбце (поле), указанном в предложениях WHERE инструкции SELECT. Существующие индексы многомерных выражений будут автоматически применены к операторам =, >, \<, > =, =< и between в предложении WHERE, а также в предикатах JOIN.  
  
 При использовании драйвера dBASE индекс, созданный инструкцией CREATE UNIQUE INDEX, фактически не уникален, а повторяющиеся значения могут быть вставлены в индексированный столбец. В индекс можно добавить только одну запись из набора с идентичными значениями ключа.  
  
 При использовании драйвера Paradox необходимо определить уникальный индекс для непрерывного подмножества столбцов в таблице, включая первый столбец. Таблица не может быть обновлена драйвером Paradox, если в таблице не определен уникальный индекс или если драйвер Paradox используется без реализации ядро СУБД Borland.

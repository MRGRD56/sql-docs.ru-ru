---
description: Строковые функции (Transact-SQL)
title: Строковые функции (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/15/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], strings
- strings [SQL Server], functions
- string functions
- strings [SQL Server]
ms.assetid: 6940a83d-5374-4af3-bb27-5d89c8af83ac
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0224186826fcc188e8dfea8bd7df30db7218d640
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753484"
---
# <a name="string-functions-transact-sql"></a>Строковые функции (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Следующие скалярные функции выполняют операции над входным строковым значением и возвращают строковое или числовое значение:  

:::row:::
    :::column:::
        [ASCII](../../t-sql/functions/ascii-transact-sql.md)
    :::column-end:::
    :::column:::
        [CHAR](../../t-sql/functions/char-transact-sql.md)
    :::column-end:::
    :::column:::
        [CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [CONCAT](../../t-sql/functions/concat-transact-sql.md)
    :::column-end:::
    :::column:::
        [CONCAT_WS](../../t-sql/functions/concat-ws-transact-sql.md)
    :::column-end:::
    :::column:::
        [DIFFERENCE](../../t-sql/functions/difference-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [FORMAT](../../t-sql/functions/format-transact-sql.md)
    :::column-end:::
    :::column:::
        [LEFT](../../t-sql/functions/left-transact-sql.md)
    :::column-end:::
    :::column:::
        [LEN](../../t-sql/functions/len-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [LOWER](../../t-sql/functions/lower-transact-sql.md)
    :::column-end:::
    :::column:::
        [LTRIM](../../t-sql/functions/ltrim-transact-sql.md)
    :::column-end:::
    :::column:::
        [NCHAR](../../t-sql/functions/nchar-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)
    :::column-end:::
    :::column:::
        [QUOTENAME](../../t-sql/functions/quotename-transact-sql.md)
    :::column-end:::
    :::column:::
        [REPLACE](../../t-sql/functions/replace-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [REPLICATE](../../t-sql/functions/replicate-transact-sql.md)
    :::column-end:::
    :::column:::
        [REVERSE](../../t-sql/functions/reverse-transact-sql.md) 
    :::column-end:::
    :::column:::
        [RIGHT](../../t-sql/functions/right-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [RTRIM](../../t-sql/functions/rtrim-transact-sql.md)
    :::column-end:::
    :::column:::
        [SOUNDEX](../../t-sql/functions/soundex-transact-sql.md) 
    :::column-end:::
    :::column:::
        [SPACE](../../t-sql/functions/space-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [STR](../../t-sql/functions/str-transact-sql.md)
    :::column-end:::
    :::column:::
        [STRING_AGG](../../t-sql/functions/string-agg-transact-sql.md)
    :::column-end:::
    :::column:::
        [STRING_ESCAPE](../../t-sql/functions/string-escape-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [STRING_SPLIT](../../t-sql/functions/string-split-transact-sql.md)
    :::column-end:::
    :::column:::
        [STUFF](../../t-sql/functions/stuff-transact-sql.md)
    :::column-end:::
    :::column:::
        [SUBSTRING](../../t-sql/functions/substring-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [TRANSLATE](../../t-sql/functions/translate-transact-sql.md)
    :::column-end:::
    :::column:::
        [TRIM](../../t-sql/functions/trim-transact-sql.md)
    :::column-end:::
    :::column:::
        [UNICODE](../../t-sql/functions/unicode-transact-sql.md) 
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [UPPER](../../t-sql/functions/upper-transact-sql.md) 
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

  
 Все встроенные строковые функции, кроме `FORMAT`, являются детерминированными. Это значит, что они каждый раз возвращают одинаковое значение для одинакового набора входных параметров. Дополнительные сведения о детерминированности функций см. в статье [Детерминированные и недетерминированные функции](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).  
  
 При передаче в строковые функции аргументов, которые не являются строковыми значениями, входной тип неявно преобразуется в текстовый тип данных. Дополнительные сведения см. в статье [Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
## <a name="see-also"></a>См. также:  
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)  
  
  


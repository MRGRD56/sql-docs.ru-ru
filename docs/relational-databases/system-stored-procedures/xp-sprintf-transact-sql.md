---
description: xp_sprintf (Transact-SQL)
title: xp_sprintf (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_sprintf_TSQL
- xp_sprintf
dev_langs:
- TSQL
helpviewer_keywords:
- xp_sprintf
ms.assetid: 1eedd65c-03cc-4eab-b76e-04684fdfec52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c4cccbc09cfeb79284dd17a0b107a754c548f5c4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755174"
---
# <a name="xp_sprintf-transact-sql"></a>xp_sprintf (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Форматирует и сохраняет серию символов и значений в строковом выходном параметре. Каждый аргумент форматирования заменяется соответствующим аргументом.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
xp_sprintf { string OUTPUT , format }  
     [ , argument [ ,...n ] ]  
```  
  
## <a name="arguments"></a>Аргументы  
 *строка*  
 Переменная типа **varchar** , которая получает выходные данные.  
  
 OUTPUT  
 При указании помещает значение переменной в выходной параметр.  
  
 *format*  
 — Это символьная строка форматирования с заполнителями для значений *аргументов* , аналогичная функции, которая поддерживается в C-Language **sprintf** . В настоящее время поддерживается только аргумент форматирования %s.  
  
 *argument*  
 Символьная строка, представляющая значение соответствующего аргумента форматирования.  
  
 *n*  
 Заполнитель, показывающий, что можно указать максимум 50 аргументов.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 **xp_sprintf** возвращает следующее сообщение:  
  
 `The command(s) completed successfully.`  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Общие расширенные хранимые процедуры &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_sscanf &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-sscanf-transact-sql.md)  
  
  

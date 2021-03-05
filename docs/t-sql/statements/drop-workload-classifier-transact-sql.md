---
description: Классификатор DROP WORKLOAD (Transact-SQL)
title: Классификатор DROP WORKLOAD (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WORKLOAD CLASSIFIER
- WORKLOAD_CLASSIFIER_TSQL
- DROP_WORKLOAD_CLASSIFIER_TSQL
- DROP WORKLOAD GROUP
dev_langs:
- TSQL
helpviewer_keywords:
- DROP WORKLOAD CLASSIFIER statement
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: 2509ee07decbfcc9ca97b247b78191fead413c6b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186465"
---
# <a name="drop-workload-classifier-transact-sql"></a>Классификатор DROP WORKLOAD (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Удаляет существующий определяемый пользователем классификатор рабочей нагрузки управления.  Если запросы выполняются или находятся в очереди запросов в приостановленном состоянии, они сохраняют свою классификацию и классификатор может быть удален немедленно. Удаление и повторное создание классификатора с другим уровнем важности не затрагивает уже классифицированные запросы.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис

```syntaxsql
DROP WORKLOAD CLASSIFIER classifier_name;
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Аргументы

*classifier_name*  
Указывает имя, по которому идентифицируется классификатор рабочей нагрузки.
  
## <a name="permissions"></a>Разрешения

Необходимо разрешение CONTROL DATABASE.  
  
## <a name="examples"></a>Примеры

В следующем примере удаляется классификатор рабочей нагрузки с именем `wgcELTROLE`.  

```sql
DROP WORKLOAD CLASSIFIER wgcELTRole;
```

> [!NOTE]
> Запрос, отправленный без соответствующего классификатора, классифицируется в группу рабочей нагрузки по умолчанию.  Группа рабочей нагрузки по умолчанию — класс ресурсов smallrc.
  
## <a name="see-also"></a>См. также

[CREATE WORKLOAD CLASSIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/create-workload-classifier-transact-sql.md)</br>
[Классификация рабочих нагрузок [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] ](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)

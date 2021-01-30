---
description: sysarticles (системное представление) (Transact-SQL)
title: sysarticles (Системное представление) (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticles
- sysarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticles view
ms.assetid: 18f8c9b3-cab7-4e8f-8754-11ac38c3f789
author: stevestein
ms.author: sstein
ms.openlocfilehash: 21e1ca1548eee3fd9b17674cdf8ee7d32845b43f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160336"
---
# <a name="sysarticles-system-view-transact-sql"></a>sysarticles (системное представление) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Представление **sysarticles** предоставляет свойства статьи. Это представление хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Столбец идентификаторов, в котором хранится уникальный идентификатор статьи.|  
|**creation_script**|**nvarchar(255)**|Скрипт схемы для статьи.|  
|**del_cmd**|**nvarchar(255)**|Команда для выполнения после инструкции DELETE; в ином случае строится на основе журнала.|  
|**description**|**nvarchar(255)**|Описание статьи.|  
|**dest_table**|**sysname**|Имя целевой таблицы.|  
|**filter**|**int**|Идентификатор хранимой процедуры, используемый для горизонтального секционирования.|  
|**filter_clause**|**ntext**|Предложение WHERE для статьи, используемое при горизонтальной фильтрации.|  
|**ins_cmd**|**nvarchar(255)**|Команда для выполнения после инструкции INSERT; иначе строится на основе журнала.|  
|**name**|**sysname**|Имя, ассоциированное со статьей, уникальное внутри публикации.|  
|**objid**|**int**|Идентификатор объекта опубликованной таблицы.|  
|**pubid**|**int**|Идентификатор публикации, к которой принадлежит статья.|  
|**pre_creation_cmd**|**tinyint**|Команда, выполняемая перед инструкциями DROP TABLE, DELETE TABLE или TRUNCATE:<br /><br /> **0** = нет.<br /><br /> **1** = удалить.<br /><br /> **2** = удалить.<br /><br /> **3** = усечение.|  
|**status**|**tinyint**|Битовая маска параметров и состояния статьи, которая может быть результатом операции побитового логического ИЛИ над одним или несколькими из следующих значений:<br /><br /> **1** = статья активна.<br /><br /> **8** = включить имя столбца в инструкции INSERT.<br /><br /> **16** = использовать параметризованные инструкции.<br /><br /> **24** = оба значения включают имя столбца в инструкции INSERT и используют параметризованные инструкции.<br /><br /> **64** = горизонтальная секция для статьи определяется трансформируемой подпиской.<br /><br /> Например, активная статья, использующая параметризованные инструкции, будет иметь значение **17** в этом столбце. Значение **0** означает, что статья неактивна и дополнительные свойства не определены.|  
|**sync_objid**|**int**|Идентификатор таблицы или представления, которое является определением статьи.|  
|**type**|**tinyint**|Тип статьи:<br /><br /> **1** = статья на основе журнала.<br /><br /> **3** = статья на основе журнала с ручным фильтром.<br /><br /> **5** = статья на основе журнала с ручным просмотром.<br /><br /> **7** = статья на основе журнала с ручным фильтром и ручным просмотром.<br /><br /> **8** = выполнение хранимой процедуры.<br /><br /> **24** = выполнение сериализуемых хранимых процедур.<br /><br /> **32** = хранимая процедура (только схема).<br /><br /> **64** = представление (только схема).<br /><br /> **128** = функция (только схема).|  
|**upd_cmd**|**nvarchar(255)**|Команда, которая должна быть выполнена после инструкции UPDATE; в ином случае она строится на основе журнала.|  
|**schema_option**|**Binary (8)**|Битовая маска параметров формирования схемы для статьи, указывающая, какие части схемы статьи назначены для доставки подписчику. Дополнительные сведения о параметрах схемы см. в статье [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Владелец таблицы в целевой базе данных.|  
|**ins_scripting_proc**|**int**|Зарегистрированная пользовательская хранимая процедура или скрипт, выполняющиеся при репликации инструкции INSERT.|  
|**del_scripting_proc**|**int**|Зарегистрированная пользовательская хранимая процедура или скрипт, выполняющиеся при репликации инструкции DELETE.|  
|**upd_scripting_proc**|**int**|Зарегистрированная пользовательская хранимая процедура или скрипт, выполняющиеся при репликации инструкции UPDATE.|  
|**custom_script**|**nvarchar (2048)**|Зарегистрированная пользовательская хранимая процедура или скрипт, выполняющиеся в конце триггера DDL.|  
|**fire_triggers_on_snapshot**|**bit**|Показывает, выполняются ли реплицированные триггеры в момент, когда делается моментальный снимок. Может принимать следующие значения:<br /><br /> **0** = триггеры не выполняются.<br /><br /> **1** = триггеры выполняются.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)  
  
  

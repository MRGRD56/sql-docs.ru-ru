---
description: Database Suspect Data Page, класс событий
title: Класс событий Database Suspect Data Page | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- event notifications [SQL Server], database mirroring
- suspect_pages system table
- database mirroring [SQL Server], event notifications
- Database Suspect Data Page event class
ms.assetid: 098e1443-a8a0-425c-9311-0a479b1370ed
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 42da9776abe587581855e94dc0a11bbe036da7a7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469795"
---
# <a name="database-suspect-data-page-event-class"></a>Database Suspect Data Page, класс событий
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Класс событий **Database Suspect Data Page** регистрирует добавление страниц в таблицу [suspect_pages](../../relational-databases/system-tables/suspect-pages-transact-sql.md) базы данных [msdb](../../relational-databases/databases/msdb-database.md). Он включается в трассировки для отслеживания появления подозрительных страниц.  
  
> [!NOTE]  
>  Данное событие возникает асинхронно по отношению к вставке соответствующей строки в таблицу **suspect_pages** . Таким образом, прослушивающее это событие задание даже может не сразу обнаружить соответствующую запись в таблице **suspect_pages** .  
  
 Если класс событий **Database Suspect Data Page** включен в трассировку, связанные с этим издержки невелики. Но они могут увеличиться при увеличении количества подозрительных страниц, например при проблемах с жестким диском.  
  
## <a name="database-suspect-data-page-event-class-data-columns"></a>Столбцы данных класса событий Database Suspect Data Page  
  
|Имя столбца данных|Тип данных|Описание|Идентификатор столбца|Фильтруемый|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|Идентификатор базы данных, для которой возникло событие подозрительной страницы. Это то же самое, что столбец **database_id** таблицы **suspect_pages** .|3|Да|  
|**EventClass**|**int**|Событие имеет тип 213.|27|Нет|  
|**EventSequence**|**int**|Порядковый номер класса событий в пакете.|51|нет|  
|**SPID**|**int**|Идентификатор задачи [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , при выполнении которой возникла подозрительная страница.|12|Да|  
|**StartTime**|**datetime**|Время возникновения события.|14|Да|  
|**ObjectID**|**int**|Идентификатор файла базы данных, содержащего подозрительную страницу. Это то же самое, что столбец **file_id** таблицы **suspect_pages** .|22|Да|  
|**ObjectID2**|**int**|Идентификатор подозрительной страницы в файле. Это то же самое, что столбец **page_id** таблицы **suspect_pages** .|56|Да|  
|**Error**|**int**|Тип происшедшей ошибки. Это значение такое же, как и значение **event_type** для страницы таблицы **suspect_pages** .|31|Да|  
  
## <a name="see-also"></a>См. также:  
 [Хранимая процедура sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Управление таблицей suspect_pages (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  

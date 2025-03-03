---
title: Руководство по процессу очистки фантомных записей | Документы Майкрософт
description: Сведения о процессе очистки фантомных записей, который представляет собой фоновый процесс, удаляющий записи со страниц, которые были помечены для удаления, в SQL Server.
ms.custom: ''
ms.date: 05/02/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- ghost cleanup
- ghost records
- ghost clean up process
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 05113205c693de928321880f1586996f81b76a66
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750364"
---
# <a name="ghost-cleanup-process-guide"></a>Руководство по процессу очистки фантомных записей

Процесс очистки фантомных записей представляет собой однопоточный фоновый процесс, удаляющий записи со страниц, которые были помечены для удаления. В следующей статье приводится обзор этого процесса.

## <a name="ghost-records"></a>Фантомные записи

Записи, которые удаляются с конечного уровня страницы индексов, физически не удаляются со страницы — запись помечается как подлежащая удалению или *фантомная*. Это означает, что строка остается на странице, но меняется в строке заголовка, чтобы указать, что она действительно является фантомной. Это позволяет оптимизировать производительность во время операции удаления. Фантомные записи необходимы для блокировки на уровне строк. Они также нужны для изоляции моментального снимка, когда требуется сохранить более старые версии строк.

## <a name="ghost-record-cleanup-task"></a>Задача очистки фантомных записей

Записи, помеченные для удаления (или *фантомные*), очищаются в ходе фонового процесса очистки фантомных записей. Этот фоновый процесс иногда выполняется после фиксации операции удаления и физически удаляет фантомные записи со страниц. Процесс очистки фантомных записей запускается автоматически через определенный интервал (каждые 5 секунд для SQL Server 2012 и более поздних версий, каждые 10 секунд для SQL Server 2008/2008 R2) и ищет все страницы, помеченные как имеющие фантомные записи. При обнаружении таких страниц процесс удаляет записи, помеченные для удаления (или *фантомные*), захватывая максимум 10 страниц при каждом выполнении.

Если запись является фантомной, база данных помечается как имеющая фантомные записи и процесс очистки фантомных записей будет проверять только эти базы данных. После удаления всех фантомных записей процесс очистки также пометит базу данных как не имеющую фантомных записей и пропустит ее во время следующего выполнения. Процесс также будет пропускать все базы данных, к которым он не может применить общую блокировку, и повторит попытку во время следующего запуска.

Приведенный ниже запрос позволяет определить количество фантомных записей, существующих в одной базе данных. 

 ```sql
 SELECT sum(ghost_record_count) total_ghost_records, db_name(database_id) 
 FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, 'SAMPLED')
 group by database_id
 order by total_ghost_records desc
```

## <a name="disable-the-ghost-cleanup"></a>Отключение очистки фантомных записей

В системах с высокой нагрузкой и большим количеством операций удаления процесс очистки фантомных записей может вызвать проблемы с производительностью, связанные с сохранением страниц в буферном пуле и созданием операций ввода-вывода. Поэтому этот процесс можно отключить с помощью [флага трассировки 661](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Однако отключение процесса оказывает влияние на производительность.

Отключение процесса очистки фантомных записей может привести к ненужному увеличению базы данных и возникновению проблем с производительностью. Так как процесс очистки фантомных записей удаляет записи, помеченные как фантомные, после его отключения эти записи будут оставаться на странице и SQL Server не сможет использовать это пространство повторно. В результате SQL Server принудительно добавляет данные на новые страницы, что приводит к чрезмерному увеличению файлов базы данных и может также стать причиной [разбиений страниц](indexes/specify-fill-factor-for-an-index.md). Разбиения страниц снижают производительность при создании планов выполнения и проведении операций сканирования. 

После отключения процесса очистки фантомных записей необходимо выполнить определенное действие по удалению фантомных записей. Одним вариантом является перестроение индекса для перемещения данных на страницах. Другой вариант заключается в запуске хранимой процедуры [sp_clean_db_free_space](system-stored-procedures/sp-clean-db-free-space-transact-sql.md) (для очистки всех файлов базы данных) или [sp_clean_db_file_free_space](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) (для очистки одного файла данных базы данных) вручную для удаления фантомных записей.

 >[!warning]
 > Обычно не рекомендуется отключать процесс очистки фантомных записей. Перед окончательным отключением процесса в рабочей среде необходимо провести соответствующую тщательную проверку в управляемой среде.


## <a name="next-steps"></a>Дальнейшие действия  
[Отключение процесса очистки фантомных записей](https://support.microsoft.com/help/920093/tuning-options-for-sql-server-when-running-in-high-performance-workloa)
<br>[Удаление фантомных записей из одного файла базы данных](system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
<br>[Удаление фантомных записей из всех файлов базы данных](system-stored-procedures/sp-clean-db-free-space-transact-sql.md)



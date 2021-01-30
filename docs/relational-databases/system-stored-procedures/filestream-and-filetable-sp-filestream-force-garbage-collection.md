---
description: sp_filestream_force_garbage_collection (Transact-SQL)
title: sp_filestream_force_garbage_collection (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_filestream_force_garbage_collection
- sp_filestream_force_garbage_collection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILESTREAM [SQL Server]
- sp_filestream_force_garbage_collection
ms.assetid: 9d1efde6-8fa4-42ac-80e5-37456ffebd0b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c234c5c03b00b25babbec0a4fbc197f235c966d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165245"
---
# <a name="sp_filestream_force_garbage_collection-transact-sql"></a>sp_filestream_force_garbage_collection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Принудительно запускает сборщик мусора FILESTREAM, который удаляет все ненужные файлы FILESTREAM.  
  
 Контейнер FILESTREAM не может быть удален до тех пор, пока все удаленные файлы в нем не будут очищены сборщиком мусора. Сборщик мусора FILESTREAM запускается автоматически. Однако, если необходимо удалить контейнер перед запуском сборщика мусора, можно использовать sp_filestream_force_garbage_collection для запуска сборщика мусора вручную.  
  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sp_filestream_force_garbage_collection
    [ @dbname = ]  'database_name'
    [ , [ @filename = ] 'logical_file_name' ]
```  
  
## <a name="arguments"></a>Аргументы  
 `[ @dbname = ]  'database_name'`  
 Обозначает имя базы данных, в которой будет запущен сборщик мусора.  
  
> [!NOTE]  
> `@dbname` имеет тип **sysname**. Если он не указан, предполагается текущая база данных.  
  
 `[ @filename = ] 'logical_file_name'`  
 Обозначает логическое имя контейнера FILESTREAM, в котором будет запущен сборщик мусора. Аргумент `@filename` является необязательным. Если логическое имя файла не указано, сборщик мусора очищает все контейнеры FILESTREAM в указанной базе данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
  
| Значение | Описание |
| ----- | ----------- |   
|0|Операция выполнена успешно|  
|1|Ошибка при выполнении операции|  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Значение|Описание|  
|-----------|-----------------|  
|*file_name*|Указывает имя контейнера FILESTREAM|  
|*num_collected_items*|Указывает число элементов FILESTREAM (файлов и каталогов), которые были собраны как мусор (удалены) в данном контейнере.|  
|*num_marked_for_collection_items*|Указывает число элементов FILESTREAM (файлов и каталогов), которые были отмечены для сборки мусора в данном контейнере. Эти элементы еще не удалены, но могут быть доступны для удаления после этапа сборки мусора.|  
|*num_unprocessed_items*|Указывает число подлежащих сборке мусора элементов FILESTREAM (файлов и каталогов), для которых сборка в данном контейнере не была выполнена. Это может произойти по разным причинам, включая следующие.<br /><br /> Файлы закреплены, так как еще не создана резервная копия журналов или контрольной точки.<br /><br /> Файлы относятся к модели восстановления FULL или BULK_LOGGED.<br /><br /> Существует длительно выполняющаяся активная транзакция.<br /><br /> Задание чтения журнала репликации не запущено. Дополнительные сведения см. в техническом документе [хранилище FILESTREAM в SQL Server 2008](/previous-versions/sql/sql-server-2008/hh461480(v=msdn.10)) .|  
|*last_collected_xact_seqno*|Возвращает последний соответствующий номер последовательности LSN, до которого для файлов была выполнена сборка мусора в указанном контейнере FILESTREAM.|  
  
## <a name="remarks"></a>Замечания  
 Явным образом запускает задачу сборщика мусора FILESTREAM для указанной базы данных (и контейнера FILESTREAM). Файлы, которые больше не требуются, удаляются процессом сборки мусора. Время, необходимое на выполнение этой операции, зависит от объема данных FILESTREAM в базе данных или контейнере, а также от объема операций DML, произведенных в последнее время над данными FILESTREAM. Хотя эта операция может выполняться и при режиме работы базы данных в сети, но в таком случае производительность базы данных может понизиться, так как во время сборки мусора выполняются различные операции ввода-вывода.  
  
> [!NOTE]  
>  Рекомендуется запускать эту операцию только при необходимости и в нерабочее время.  
  
Несколько вызовов этой хранимой процедуры можно осуществить одновременно только в различных контейнерах или базах данных.  

Из-за 2-фазных операций хранимая процедура должна запускаться дважды для фактического удаления базовых файлов FILESTREAM.  

Сборка мусора (GC) основывается на усечении журнала. Таким образом, если файлы были недавно удалены из базы данных с использованием модели полного восстановления, они являются GC-ED только после создания резервной копии журнала этих частей журнала транзакций, а часть журнала помечается как неактивная. В базе данных, использующей простую модель восстановления, усечение журнала происходит после выполнения в `CHECKPOINT` базе данных.  


## <a name="permissions"></a>Разрешения  
 Требуется членство в роли базы данных db_owner.  
  
## <a name="examples"></a>Примеры  
 В следующих примерах сборщик мусора запускается для контейнеров FILESTREAM в базе данных `FSDB`.  
  
### <a name="a-specifying-no-container"></a>A. Без указания контейнера  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB';  
```  
  
### <a name="b-specifying-a-container"></a>Б. С указанием контейнера  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB',
    @filename = N'FSContainer';  
```  
  
## <a name="see-also"></a>См. также:  
[Потока](../../relational-databases/blob/filestream-sql-server.md)
<br>[Таблицы FileTable](../../relational-databases/blob/filetables-sql-server.md)
<br>[Динамические административные представления Filestream и FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Представления каталога Filestream и FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)
  

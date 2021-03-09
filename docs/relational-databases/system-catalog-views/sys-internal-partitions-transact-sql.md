---
description: sys.internal_partitions (Transact-SQL)
title: sys.internal_partitions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0262df2b-5ba7-4715-b17b-3d9ce470a38e
author: ronortloff
ms.author: rortloff
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5d43d8923a7e12a8389a9b3bd8a0801b989a560
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464850"
---
# <a name="sysinternal_partitions-transact-sql"></a>sys.internal_partitions (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Возвращает по одной строке для каждого набора строк, который отслеживает внутренние данные для индексов columnstore на дисковых таблицах. Эти наборы строк являются внутренними индексами columnstore и отслеживанием удаленных строк, группы строк сопоставлений и разностного хранилища групп строк. Они отписывают данные для каждой секции таблицы. Каждая таблица имеет по крайней мере одну секцию. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] повторно создает наборы строк каждый раз при пересборке индекса columnstore.   
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|partition_id|**bigint**|Идентификатор секции для этой секции. Является уникальным в пределах базы данных.|  
|object_id|**int**|Идентификатор объекта для таблицы, содержащей секцию.|  
|index_id|**int**|Идентификатор индекса для индекса columnstore, определенного для таблицы.<br /><br /> 1 = кластеризованный индекс columnstore<br /><br /> 2 = некластеризованный индекс columnstore|  
|partition_number|**int**|Номер секции.<br /><br /> 1 = Первая секция секционированной таблицы или единственная секция несекционированной таблицы.<br /><br /> 2 = Вторая секция и т. д.|  
|internal_object_type|**tinyint**|Объекты наборов строк, которые отписывают внутренние данные для индекса columnstore.<br /><br /> 2 = COLUMN_STORE_DELETE_BITMAP<br /><br /> 3 = COLUMN_STORE_DELTA_STORE<br /><br /> 4 = COLUMN_STORE_DELETE_BUFFER<br /><br /> 5 = COLUMN_STORE_MAPPING_INDEX|  
|internal_object_type_desc|**nvarchar(60)**|COLUMN_STORE_DELETE_BITMAP — этот индекс точечного рисунка отслеживает строки, помеченные как удаленные из columnstore. Точечный рисунок предназначен для каждого группы строк, так как секции могут содержать строки в нескольких групп строк. Строки являются физически присутствующими и занимают место в columnstore.<br /><br /> COLUMN_STORE_DELTA_STORE — хранит группы строк с именем групп строк, которые не были сжаты в хранилище по столбцам. Каждая секция таблицы может иметь ноль или более deltastore групп строк.<br /><br /> COLUMN_STORE_DELETE_BUFFER — для обслуживания удалений с обновляемыми некластеризованными индексами columnstore. Когда запрос удаляет строку из базовой таблицы rowstore, буфер удаления отслеживает удаление из columnstore. Если число удаленных строк превышает 1048576, они объединяются в цепочку удаления растрового объекта в фоновом режиме или с помощью явной команды реорганизации.  В любой момент времени объединение битовой карты удаления и буфера удаления представляет все удаленные строки.<br /><br /> COLUMN_STORE_MAPPING_INDEX — используется только в том случае, если кластеризованный индекс columnstore имеет вторичный некластеризованный индекс. Он сопоставляет ключи некластеризованного индекса с правильным группы строк и ИДЕНТИФИКАТОРом строки в columnstore. Он хранит только ключи для строк, которые перемещаются в другую группы строк. Это происходит, когда разностный группы строк сжимается в columnstore, и когда операция слияния объединяет строки из двух разных групп строк.|  
|Row_group_id|**int**|Идентификатор deltastore группы строк. Каждая секция таблицы может иметь ноль или более deltastore групп строк.|  
|hobt_id|**bigint**|Идентификатор внутреннего объекта набора строк (HoBT). Это хороший ключ для объединения с другими динамическими представлениями DMV, чтобы получить дополнительные сведения о физических характеристиках внутреннего набора строк.|  
|rows|**bigint**|Приблизительное количество строк в данной секции.|  
|data_compression|**tinyint**|Состояние сжатия для набора строк:<br /><br /> 0 = нет<br /><br /> 1 = ROW<br /><br /> 2 = PAGE|  
|data_compression_desc|**nvarchar(60)**|Состояние сжатия для каждой секции. Возможные значения для таблиц rowstore: NONE, ROW и PAGE. Возможные значения для таблиц columnstore: COLUMNSTORE и COLUMNSTORE_ARCHIVE.|  
|optimize_for_sequential_key|**bit**|1 = для секции включена оптимизация вставки последней страницы.<br><br>0 = значение по умолчанию. Для секции включена оптимизация вставки последней страницы.|
  
## <a name="permissions"></a>Разрешения  
 Необходимо членство в роли `public`. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="general-remarks"></a>Общие замечания  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] повторно создает новые внутренние индексы columnstore каждый раз при создании или пересборке индекса columnstore.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-view-all-of-the-internal-rowsets-for-a-table"></a>A. Просмотреть все внутренние наборы строк для таблицы  
 В этом примере возвращаются все внутренние наборы строк columnstore для таблицы. Можно также использовать hobt_id для поиска дополнительных сведений о конкретном наборе строк.  
  
```sql  
SELECT i.object_id, i.index_id, i.name, p.hobt_id, p.internal_object_type_id, p.internal_object_type_desc  
FROM sys.internal_partitions AS p  
JOIN sys.indexes AS i  
on i.object_id = p.object_id  
WHERE p.object_id = OBJECT_ID ( '<table name' ) ;  
```  
  
## <a name="see-also"></a>См. также  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  

---
description: sys.dm_os_buffer_descriptors (Transact-SQL)
title: sys.dm_os_buffer_descriptors (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_buffer_descriptors_TSQL
- dm_os_buffer_descriptors_TSQL
- sys.dm_os_buffer_descriptors
- dm_os_buffer_descriptors
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_buffer_descriptors dynamic management view
ms.assetid: 012aab95-8888-4f35-9ea3-b5dff6e3f60f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 449c4d2ca5a101d17c84b414ffc119dbdb0fa476
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054737"
---
# <a name="sysdm_os_buffer_descriptors-transact-sql"></a>sys.dm_os_buffer_descriptors (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает сведения обо всех страницах данных, расположенных в данный момент в буферном пуле [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это представление может использоваться, чтобы определить распределение страниц баз данных в буферном пуле в соответствии с базой данных, объектом или типом. В [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] это динамическое административное представление также возвращает сведения о страницах данных в файле расширения буферного пула. Дополнительные сведения см. в разделе [расширение буферного пула](../../database-engine/configure-windows/buffer-pool-extension.md).  
  
 При считывании страницы данных с диска она копируется в буферный пул [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и кэшируется для повторного использования. Каждая страница данных в кэше имеет один дескриптор буфера. Дескрипторы буфера уникально идентифицируют каждую страницу данных, кэшируемую в данный момент в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_os_buffer_descriptors возвращает кэшированные страницы для всех пользовательских и системных баз данных. В их число входят страницы, связанные с базой данных Resource.  
  
> **Примечание.** Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_buffer_descriptors**.  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|database_id|**int**|Идентификатор базы данных, связанный со страницей в буферном пуле. Допускает значение NULL.|  
|file_id|**int**|Идентификатор файла, хранящего постоянный образ страницы. Допускает значение NULL.|  
|page_id|**int**|Идентификатор страницы в файле. Допускает значение NULL.|  
|page_level|**int**|Индексный уровень страницы. Допускает значение NULL.|  
|allocation_unit_id|**bigint**|Идентификатор единицы распределения страницы. Это значение может быть использовано для соединения sys.allocation_units. Допускает значение NULL.|  
|page_type|**nvarchar(60)**|Тип страницы, например: страница данных или страница индекса. Допускает значение NULL.|  
|row_count|**int**|Количество строк на странице. Допускает значение NULL.|  
|free_space_in_bytes|**int**|Объем доступного свободного места, в байтах, на странице. Допускает значение NULL.|  
|is_modified|**bit**|1 = страница была изменена после того, как она была считана с диска. Допускает значение NULL.|  
|numa_node|**int**|Узел с неоднородным доступом к памяти для буфера. Допускает значение NULL.|  
|read_microsec|**bigint**|Фактическое время (в миллисекундах), необходимое для считывания страницы в буфер. Счетчик сбрасывается, если буфер используется повторно. Допускает значение NULL.| 
|is_in_bpool_extension|**bit**|1 = страница находится в расширении буферного пула. Допускает значение NULL.| 
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.| 
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
   
## <a name="remarks"></a>Комментарии  
 sys.dm_os_buffer_descriptors возвращает страницы, используемые базой данных ресурсов. sys.dm_os_buffer_descriptors не возвращает сведения о свободных или украденных страницах или о страницах, на которых произошли ошибки при их чтении.  
  
|Исходный тип|Кому|Включено|Связь|  
|----------|--------|--------|------------------|  
|sys.dm_os_buffer_descriptors|sys.databases|database_id|«многие к одному»|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.allocation_units|allocation_unit_id|«многие к одному»|  
|sys.dm_os_buffer_descriptors|\<userdb>.sys.database_files|file_id|«многие к одному»|  
|sys.dm_os_buffer_descriptors|sys.dm_os_buffer_pool_extension_configuration|file_id|«многие к одному»|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-cached-page-count-for-each-database"></a>A. Получение количества страниц в кэше для каждой базы данных  
 Следующий пример возвращает количество страниц в кэше, загруженных для каждой базы данных.  
  
```  
SELECT COUNT(*)AS cached_pages_count  
    ,CASE database_id   
        WHEN 32767 THEN 'ResourceDb'   
        ELSE db_name(database_id)   
        END AS database_name  
FROM sys.dm_os_buffer_descriptors  
GROUP BY DB_NAME(database_id) ,database_id  
ORDER BY cached_pages_count DESC;  
```  
  
### <a name="b-returning-cached-page-count-for-each-object-in-the-current-database"></a>Б. Получение количества страниц в кэше для каждого объекта в текущей базе данных  
 Следующий пример возвращает количество страниц в кэше, загруженных для каждого объекта в текущей базе данных.  
  
```  
SELECT COUNT(*)AS cached_pages_count   
    ,name ,index_id   
FROM sys.dm_os_buffer_descriptors AS bd   
    INNER JOIN   
    (  
        SELECT object_name(object_id) AS name   
            ,index_id ,allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.hobt_id   
                    AND (au.type = 1 OR au.type = 3)  
        UNION ALL  
        SELECT object_name(object_id) AS name     
            ,index_id, allocation_unit_id  
        FROM sys.allocation_units AS au  
            INNER JOIN sys.partitions AS p   
                ON au.container_id = p.partition_id   
                    AND au.type = 2  
    ) AS obj   
        ON bd.allocation_unit_id = obj.allocation_unit_id  
WHERE database_id = DB_ID()  
GROUP BY name, index_id   
ORDER BY cached_pages_count DESC;  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.allocation_units (Transact-SQL)](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 
 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [База данных Resource](../../relational-databases/databases/resource-database.md)   
 [sys.dm_os_buffer_pool_extension_configuration (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-pool-extension-configuration-transact-sql.md)  
  

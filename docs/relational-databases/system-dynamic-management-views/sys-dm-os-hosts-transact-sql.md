---
description: sys.dm_os_hosts (Transact-SQL)
title: sys.dm_os_hosts (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_hosts_TSQL
- dm_os_hosts
- dm_os_hosts_TSQL
- sys.dm_os_hosts
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_hosts dynamic management view
ms.assetid: a313ff3b-1fe9-421e-b94b-cea19c43b0e5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ce103d01327aac7250df993bfb9123439a5f5ca7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100338886"
---
# <a name="sysdm_os_hosts-transact-sql"></a>sys.dm_os_hosts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает список всех узлов, зарегистрированных на данный момент в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это представление также возвращает ресурсы, используемые перечисляемыми узлами.  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_os_hosts**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**host_address**|**varbinary(8)**|Внутренний адрес в памяти объекта узла.|  
|**type**|**nvarchar(60)**|Тип размещенного компонента. Например,<br /><br /> SOSHOST_CLIENTID_SERVERSNI = собственный интерфейс SQL Server;<br /><br /> SOSHOST_CLIENTID_SQLOLEDB = поставщик OLE DB для собственного клиента SQL Server;<br /><br /> SOSHOST_CLIENTID_MSDART = компоненты доступа к данным MDA.|  
|**name**|**nvarchar(32)**|Имя узла.|  
|**enqueued_tasks_count**|**int**|Общее количество задач, которые данный узел поместил в очереди на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**active_tasks_count**|**int**|Количество выполняющихся в данный момент задач, помещенных этим узлом в очереди.|  
|**completed_ios_count**|**int**|Количество операций ввода-вывода, инициированных и выполненных посредством этого узла.|  
|**completed_ios_in_bytes**|**bigint**|Суммарное количество байтов, обработанных в операциях ввода-вывода посредством этого узла.|  
|**active_ios_count**|**int**|Общее количество запросов ввода-вывода, относящихся к этому узлу, ожидающих завершения в настоящее время.|  
|**default_memory_clerk_address**|**varbinary(8)**|Адрес в памяти объекта клерка памяти, связанного с этим узлом. Дополнительные сведения см. в разделе [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md).|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="remarks"></a>Remarks  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] разрешены компоненты, такие как поставщик OLE DB, которые не являются частью исполняемого файла [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], для выделения памяти и участия в планировании в режиме без вытеснения. Эти компоненты размещаются в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а все ресурсы, выделенные им, отслеживаются. Размещение позволяет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] лучше учитывать ресурсы, которые используются компонентами, внешними по отношению к исполняемому объекту [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|Исходный тип|Кому|Relationship|  
|----------|--------|------------------|  
|sys.dm_os_hosts. default_memory_clerk_address|sys.dm_os_memory_clerks. memory_clerk_address|один к одному|  
|sys.dm_os_hosts. host_address|sys.dm_os_memory_clerks. host_address|один к одному|  
  
## <a name="examples"></a>Примеры  
 В следующем примере определяется общий объем памяти, задействованной размещенным компонентом.  
  
||  
|-|  
|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.|  
  
```  
SELECT h.type, SUM(mc.pages_kb) AS commited_memory  
FROM sys.dm_os_memory_clerks AS mc   
INNER JOIN sys.dm_os_hosts AS h   
    ON mc.memory_clerk_address = h.default_memory_clerk_address  
GROUP BY h.type;  
```  
  
## <a name="see-also"></a>См. также:  

 [sys.dm_os_memory_clerks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)   
 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  



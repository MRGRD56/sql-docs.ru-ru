---
description: catalog.object_versions (база данных SSISDB)
title: catalog.object_versions (база данных SSISDB) | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2fd8c020-1c77-4702-8e6b-efa6a348daab
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 83ccc09b8a38340939340f214c3ca62fddca6d47
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835177"
---
# <a name="catalogobject_versions-ssisdb-database"></a>catalog.object_versions (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Отображает версии объектов в каталоге служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. В этом выпуске в данном представлении поддерживаются только версии проектов.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_version_lsn|**bigint**|Уникальный идентификатор версии объекта. Не гарантируется, что это число будет последовательным.|  
|object_id|**bigint**|Уникальный идентификатор объекта.|  
|object_type|**smallint**|Тип объекта. Для проектов отображается значение `20`.|  
|object_name|**sysname(nvarchar(128))**|Имя объекта.|  
|description|**nvarchar(1024)**|Описание проекта.|  
|created_by|**nvarchar(128)**|Имя пользователя, который добавил объект в каталог.|  
|created_time|**datetimeoffset**|Дата и время добавления объекта в каталог.|  
|restored_by|**nvarchar(128)**|Имя пользователя, который восстановил объект.|  
|last_restored_time|**datetimeoffset**|Дата и время последнего восстановления объекта.|  
  
## <a name="remarks"></a>Комментарии  
 В этом представлении отображается по одной строке для каждой из версий объекта в каталоге.  
  
## <a name="permissions"></a>Разрешения  
 Для просмотра строк в этом представлении необходимо иметь одно из следующих разрешений:  
  
-   разрешение READ на объект  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**.  
  
> [!NOTE]  
>  Действует защита на уровне строки. Отображаются только строки, на которые у вас имеется разрешение.  
  
  

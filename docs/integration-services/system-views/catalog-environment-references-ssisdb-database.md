---
description: catalog.environment_references (база данных SSISDB)
title: catalog.environment_references (база данных SSISDB) | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: efec53ef-3e5a-4b76-b71d-a0cf9e11ac00
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 617f7b5132f6df2cd8acd02579512b880ece4ff2
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243651"
---
# <a name="catalogenvironment_references-ssisdb-database"></a>catalog.environment_references (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Отображает ссылки на среду для всех проектов в каталоге **SSISDB**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|reference_id|**bigint**|Уникальный идентификатор (ID) ссылки.|  
|project_id|**bigint**|Уникальный идентификатор проекта.|  
|reference_type|**char(1)**|Указывает, может среда находиться в той же папке, что и проект (относительная ссылка), или в другой папке (абсолютная ссылка). Если значение равно `R`, для определения расположения сред используется относительная ссылка. Если значение равно `A`, то для определения расположения сред используется абсолютная ссылка.|  
|environment_folder_name|**sysname**|Имя папки в случае, если для определения местоположения среды используется абсолютная ссылка.|  
|environment_name|**sysname**|Имя среды, на которую ссылается проект.|  
|validation_status|**char(1)**|Состояние проверки.|  
|last_validation_time|**datatimeoffset(7)**|Время последней проверки.|  
  
## <a name="remarks"></a>Комментарии  
- В этом представлении отображается строка для каждой ссылки на среду в каталоге.  
  
- Проект может иметь относительные или абсолютные ссылки на среду. Относительные ссылки указывают среду по имени и требуют, чтобы она находилась в той же папке, что и проект. Абсолютные ссылки указывают среду с применением имени и папки и могут указывать среды, находящиеся в иной папке, чем проект. Проект может ссылаться на несколько сред.  

## <a name="permissions"></a>Разрешения  
 Это представление требует применения одного из следующих разрешений:  
  
-   Разрешение READ на соответствующий проект  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**.  
  
> [!NOTE]  
>  Наличие разрешения READ на проект влечет за собой также наличие разрешения READ на все пакеты и ссылки на среду, связанные с этим проектом. Действует защита на уровне строки. Отображаются только строки, на которые у вас имеется разрешение.  
  

---
description: catalog.delete_project (база данных SSISDB)
title: catalog.delete_project (база данных SSISDB) | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: f3431445-8dd2-443b-813e-b99db893977e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 40e9a9a404c17b1a86b48fef45cdc32bf399ace3
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129826"
---
# <a name="catalogdelete_project-ssisdb-database"></a>catalog.delete_project (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет существующий проект из папки в каталоге служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
catalog.delete_project [ @folder_name = ] folder_name , [ @project_name = ] project_name  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @folder_name = ] *folder_name*  
 Имя папки, которая содержит проект. Параметр *folder_name* имеет тип **nvarchar(128)**.  
  
 [ @project_name = ] *project_name*  
 Имя удаляемого проекта. Параметр *project_name* имеет тип **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Значения кодов возврата  
 0 (успешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует применения одного из следующих разрешений:  
  
-   Разрешения READ и MODIFY на проект  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
## <a name="errors-and-warnings"></a>Ошибки и предупреждения  
 В следующем списке перечислены некоторые условия, при которых хранимая процедура delete_project может вызвать ошибку:  
  
-   Проект не существует  
  
-   Папка не существует  
  
-   Пользователь не имеет соответствующих разрешений  
  
## <a name="remarks"></a>Комментарии  
 Все объекты и ссылки на среду удаляются вместе с соответствующим проектом. Однако, версии проекта и значимые записи об операциях сохраняются до следующего запуска задания очистки операций.  
  
  

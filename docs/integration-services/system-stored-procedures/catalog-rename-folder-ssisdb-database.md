---
description: catalog.rename_folder (база данных SSISDB)
title: catalog.rename_folder (база данных SSISDB) | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 336ab467-c32f-4d2e-a79c-174dc6fab75e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 65b913576b68e5c84037eac57205b23b81d53f95
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129764"
---
# <a name="catalogrename_folder-ssisdb-database"></a>catalog.rename_folder (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Переименовывает папку в каталоге служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
catalog.rename_folder [ @old_name = ] old_name , [ @new_name = ] new_name  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @old_name = ] *old_name*  
 Исходное имя папки. Параметр *old_name* имеет тип **nvarchar(128)**.  
  
 [ @new_name = ] *new_name*  
 Новое имя папки. Параметр *new_name* имеет тип **nvarchar(128)**.  
  
## <a name="return-code-value"></a>Значения кодов возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует применения одного из следующих разрешений:  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
## <a name="errors-and-warnings"></a>Ошибки и предупреждения  
 Следующий список содержит описания некоторых условий, которые могут вызвать ошибку или предупреждение.  
  
-   Исходное имя папки недопустимо  
  
-   Новое имя уже используется существующей папкой  
  
  

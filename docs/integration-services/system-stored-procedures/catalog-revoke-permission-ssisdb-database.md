---
description: catalog.revoke_permission (база данных SSISDB)
title: catalog.revoke_permission (база данных SSISDB) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- revoke_permission stored procedure [Integration Services]
- catalog.revoke_permission stored procedure [Integration Services]
ms.assetid: 850b9c26-5c7c-47b9-a61c-5cf9bb5948cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0f2a25972d62f519f7a2c465df78b4101640944c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355336"
---
# <a name="catalogrevoke_permission-ssisdb-database"></a>catalog.revoke_permission (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Отзывает разрешение на защищаемый объект в каталоге служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```sql
catalog.revoke_permission [ @object_type = ] object_type  
    , [ @object_id = ] object_id  
    , [ @principal_id = ] principal_id  
    , [ @permission_type = ] permission_type  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @object_type = ] *object_type*  
 Тип защищаемого объекта. Типы защищаемых объектов включают папку (`1`), проект (`2`), среду (`3`) и операцию (`4`). Параметр *object_type* имеет тип **smallint** _._  
  
 [ @object_id = ] *object_id*  
 Уникальный идентификатор защищаемого объекта. Параметр *object_id* имеет тип **bigint**.  
  
 [ @principal_id = ] *principal_id*  
 Идентификатор участника, у которого отзывается разрешение. Параметр *principal_id* имеет тип **int**.  
  
 [ @permission_type = ] *permission_type*  
 Тип разрешения. Параметр *permission_type* имеет тип **smallint**.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение)  
  
 1 (object_class недопустим)  
  
 2 (object_id не существует)  
  
 3 (субъект не существует)  
  
 4 (разрешение не существует)  
  
 5 (другая ошибка)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует применения одного из следующих разрешений:  
  
-   Разрешения ASSIGN_PERMISSIONS на объект  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
## <a name="remarks"></a>Комментарии  
 Если задан тип разрешения permission_type, хранимая процедура удаляет разрешение, явно предоставленное участнику на объект. Даже если таких экземпляров нет, процедура возвращает значение успешного выполнения (`0`). Если permission_type не задан, хранимая процедура удаляет все разрешения, явно предоставленные участнику на объект.  
  
> [!NOTE]  
>  У участника может оставаться указанное разрешение на объект, если он является членом роли, имеющей это разрешение.  
  
 Хранимая процедура позволяет отзывать типа разрешений, указанные в следующей таблице:  
  
|Значение permission_type|Имя разрешения|Описание разрешения|Применимые типы объектов|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|Разрешает участнику читать сведения, рассматриваемые как часть объекта, например свойства. Тем самым участнику не разрешается перечислять или читать содержимое других объектов, содержащихся в этом объекте.|Папка, проект, среда, операция|  
|`2`|MODIFY|Разрешает участнику изменять сведения, рассматриваемые как часть объекта, например свойства. Тем самым участнику не разрешается изменять другие объекты, содержащиеся в этом объекте.|Папка, проект, среда, операция|  
|`3`|EXECUTE|Разрешает участнику выполнять все пакеты в проекте.|Project|  
|`4`|MANAGE_PERMISSIONS|Разрешает участнику назначать разрешения на объекты.|Папка, проект, среда, операция|  
|`100`|CREATE_OBJECTS|Разрешает участнику создавать объекты в папке.|Папка|  
|`101`|READ_OBJECTS|Разрешает участнику читать все объекты в папке.|Папка|  
|`102`|MODIFY_OBJECTS|Разрешает участнику изменять все объекты в папке.|Папка|  
|`103`|EXECUTE_OBJECTS|Разрешает участнику выполнять все пакеты из всех проектов в папке.|Папка|  
|`104`|MANAGE_OBJECT_PERMISSIONS|Разрешает участнику управлять разрешениями на все объекты в папке.|Папка|  
  
  

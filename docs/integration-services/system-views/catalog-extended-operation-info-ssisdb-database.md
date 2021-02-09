---
description: catalog.extended_operation_info (база данных SSISDB)
title: catalog.extended_operation_info (база данных SSISDB) | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: db299b45-557d-4c62-8e14-355cdb051f63
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d08a1df29dbea83704165122a16d8daaff110e76
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835223"
---
# <a name="catalogextended_operation_info-ssisdb-database"></a>catalog.extended_operation_info (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Отображает расширенные сведения для всех операций в каталоге служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|info_id|**bigint**|Уникальный идентификатор (ID) расширенных сведений.|  
|operation_id|**bigint**|Уникальный идентификатор (ID) операции, соответствующей расширенным сведениям.|  
|object_name|**nvarchar(260)**|Имя объекта.|  
|object_type|**smallint**|Тип объекта, затронутого операцией. Объект может представлять собой папку (`10`), проект (`20`), пакет (`30`), среду (`40`) или экземпляр выполнения (`50`).|  
|reference_id|**bigint**|Уникальный идентификатор (ID) ссылки, используемой в операции.|  
|status|**int**|Состояние операции. Возможными значениями являются: создана (`1`), запущена (`2`), отменена (`3`), завершена неуспешно (`4`), ожидает (`5`), завершена непредвиденно (`6`), выполнена успешно (`7`), остановлена (`8`) и завершена (`9`).|  
|start_time|**datetimeoffset(7)**|Дата и время начала операции.|  
|end_time|**datetimeoffset(7)**|Дата и время окончания операции.|  
  
## <a name="remarks"></a>Remarks  
 В одном объекте может быть несколько строк расширенных сведений.  
  
## <a name="permissions"></a>Разрешения  
 Это представление требует применения одного из следующих разрешений:  
  
-   Разрешение READ по отношению к операции  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
> [!NOTE]  
>  Наличие разрешения на выполнение операции на сервере подразумевает наличие разрешения на просмотр сведений об этой операции. Действует защита на уровне строки. Отображаются только строки, на которые у вас имеется разрешение.  
  
  

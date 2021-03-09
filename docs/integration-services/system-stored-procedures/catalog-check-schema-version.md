---
description: catalog.check_schema_version
title: catalog.check_schema_version | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e0d5e9f5-59c6-4118-87b5-4aa5c37a7df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa47e5107551ec2d4ded0832ff52922ffb4bfd55
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247521"
---
# <a name="catalogcheck_schema_version"></a>catalog.check_schema_version 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Определяет, совместимы ли схема каталога SSISDB и двоичные файлы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (сборка ISServerExec и SQLCLR).  
  
 ISServerExec.exc заносит в журнал сообщение об ошибке, если схема и двоичные файлы несовместимы.  
  
 Версия схемы SSISDB увеличивается, если происходит изменение схемы во время применения исправлений и во время обновлений. Поэтому рекомендуется запускать эту хранимую процедуру после восстановления резервной копии SSISDB, чтобы убедиться, что схема и двоичные файлы совместимы.  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
catalog.check_schema_version [ @use32bitruntime = ] use32bitruntime  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @use32bitruntime= ] *use32bitruntime*  
 Если параметр имеет значение **1**, то вызывается 32-разрядная версия программы dtexec. Параметр *use32bitruntime* имеет тип **int**.  
  
 
## <a name="return-code-value"></a>Значения кодов возврата 
Возвращаемое значение 0 означает успешное выполнение. 

## <a name="result-set"></a>Результирующий набор  

Возвращает таблицу следующего формата:

| Имя столбца | Тип данных | Описание |
|---|---|---|
| SERVER_BUILD | **decimal** | Версия SQL Server. Например, сервер, на котором работает SQL Server 2014, имеет версию `14.0.3335.7`. |
| SCHEMA_VERSION | **tinyint** | Номер версии SQL Server. Например, SQL Server 2017 и 2019 соответствуют `6` и `7`.|
| SCHEMA_BUILD | **string** | Сборка для схемы. |
| ASSEMBLY_BUILD | **string** | Версия сборки. |
| SHARED_COMPONENT_VERSION | **string** | Версия общего компонента. | 

## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует наличия одного из следующих разрешений:  
  
-   Членство в роли базы данных **ssis_admin**.  
  
  

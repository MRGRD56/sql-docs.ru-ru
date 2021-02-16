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
ms.openlocfilehash: fe36fbd5d5a49889fb7a0a02c82f7ade489f2570
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339215"
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
  
## <a name="result-set"></a>Результирующий набор  
 None  
  
## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует наличия одного из следующих разрешений:  
  
-   Членство в роли базы данных **ssis_admin**.  
  
  

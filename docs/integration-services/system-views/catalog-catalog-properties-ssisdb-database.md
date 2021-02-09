---
description: catalog.catalog_properties (база данных SSISDB)
title: catalog.catalog_properties (база данных SSISDB) | Документы Майкрософт
ms.custom: ''
ms.date: 12/11/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e604a382-95c8-4764-b268-742eb5c6d4cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4028247051220e1e45d054e400da050601254fb2
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835882"
---
# <a name="catalogcatalog_properties-ssisdb-database"></a>catalog.catalog_properties (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Отображает свойства каталога служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|property_name|**nvarchar(256)**|Имя свойства каталога.|  
|property_value|**nvarchar(256)**|Значение свойства каталога.|  
  
## <a name="remarks"></a>Комментарии  
 В этом представлении отображается строка для каждого свойства каталога.
  
|Имя свойства|Описание|  
|-------------------|-----------------|  
|**DEFAULT_EXECUTION_MODE**|Режим выполнения пакетов, установленный по умолчанию на уровне сервера: `Server` (0) или `Scale Out` (1). |
|**ENCRYPTION_ALGORITHM**|Тип алгоритма шифрования конфиденциальных данных. Допустимыми значениями являются `DES`, `TRIPLE_DES`, `TRIPLE_DES_3KEY`, `DESX`, `AES_128`, `AES_192` и `AES_256`. Примечание. Для изменения этого свойства база данных каталога должна работать в однопользовательском режиме.|
|**IS_SCALEOUT_ENABLED**|Значение `True` свидетельствует о том, что функция SSIS Scale Out включена. Если функция Scale Out не включена, это свойство может не отображаться в представлении.|
|**MAX_PROJECT_VERSIONS**|Количество новых версий проекта, хранимых в одном проекте. При включении очистки версий старые версии сверх этого количества удаляются.|  
|**OPERATION_CLEANUP_ENABLED**|Если значение равно `TRUE`, подробности и сообщения, связанные с операциями старше **RETENTION_WINDOW** (дней), удаляются из каталога. Если значение равно `FALSE`, все подробности и сообщения операции сохраняются в каталоге. Примечание. Задание [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет очистку операций.|  
|**RETENTION_WINDOW**|Срок (в днях) сохранения подробностей и сообщений операции в каталоге. Значение, равное `-1`, соответствует неограниченному сроку хранения. Примечание. Если очистка не требуется, присвойте свойству **OPERATION_CLEANUP_ENABLED** значение **FALSE**.|
|**SCHEMA_BUILD**|Номер сборки для схемы базы данных каталога SSISDB. Этот номер изменяется каждый раз при создании или обновлении каталога SSISDB.|
|**SCHEMA_VERSION**|Основной номер версии для схемы базы данных каталога SSISDB. Этот номер изменяется каждый раз при создании или обновлении основной версии каталога SSISDB.|
|**VALIDATION_TIMEOUT**|Срок (в секундах), по истечении которого прекращаются невыполненные проверки.|  
|**SERVER_CUSTOMIZED_LOGGING_LEVEL**|Настраиваемый уровень ведения журнала по умолчанию для сервера служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Если настраиваемые уровни ведения журнала не созданы, это свойство может не отображаться в представлении.|
|**SERVER_LOGGING_LEVEL**|Уровень ведения журнала по умолчанию для сервера служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].|
|**SERVER_OPERATION_ENCRYPTION_LEVEL**|Если значение равно 1 (`PER_EXECUTION`), сертификат и симметричный ключ, используемые для защиты важных параметров выполнения и журналов выполнения, создаются для каждого *выполнения*. Если значение равно 2 (`PER_PROJECT`), сертификат и симметричный ключ создаются один раз для каждого *проекта*. Дополнительные сведения об этом свойстве см. в разделе "Примечания" для хранимой процедуры SSIS [catalog.cleanup_server_log](../system-stored-procedures/catalog-cleanup-server-log.md#remarks).|
|**VERSION_CLEANUP_ENABLED**|Если значение равно `TRUE`, в каталоге будет сохраняться определенное количество версий проекта, равное **MAX_PROJECT_VERSIONS**, а остальные будут удаляться. Если это значение равно **FALSE**, в каталоге будут сохраняться все версии проекта. Примечание. Задание [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет очистку операций.|
|||
  
## <a name="permissions"></a>Разрешения  
 Это представление требует применения одного из следующих разрешений:  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
  

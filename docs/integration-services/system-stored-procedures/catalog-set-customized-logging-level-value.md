---
description: catalog.set_customized_logging_level_value
title: catalog.set_customized_logging_level_value | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: d83fb763-c7c6-4e20-bd10-0f995598b198
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4b37d7d4d9bdb7eb3e16d9d8295dcc38a7523430
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273245"
---
# <a name="catalogset_customized_logging_level_value"></a>catalog.set_customized_logging_level_value 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Изменяет статистику или события, которые регистрируются на существующем настроенном уровне ведения журнала. Дополнительные сведения о настроенных уровнях ведения журналов см. в разделе [Ведение журналов в службах Integration Services (SSIS)](../../integration-services/performance/integration-services-ssis-logging.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
catalog.set_customized_logging_level_value [ @level_name = ] level_name  
    , [ @property_name = ] property_name  
    , [ @property_value = ] property_value  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @level_name = ] *level_name*  
 Название существующего настроенного уровня ведения журнала.  
  
 Параметр *level_name* имеет тип **nvarchar(128)**.  
  
 [ @property_name = ] *property_name*  
 Имя свойства, которое необходимо изменить. Допустимые значения: **PROFILE** и **EVENTS**.  
  
 Параметр *property_name* имеет тип **nvarchar(128)**.  
  
 [ @property_value = ] *property_value*  
 Новое значение указанного свойства для заданного настроенного уровня ведения журнала.  
  
 Список допустимых значений для профиля и событий см. в разделе [catalog.create_customized_logging_level](../../integration-services/system-stored-procedures/catalog-create-customized-logging-level.md).  
  
 Параметр *property_value* имеет тип **bigint**.  
  
## <a name="remarks"></a>Remarks  
  
## <a name="return-codes"></a>Коды возврата  
 0 (успешное завершение)  
  
 В случае отказа хранимой процедуры выдается ошибка.  
  
## <a name="result-set"></a>Результирующий набор  
 None  
  
## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует применения одного из следующих разрешений:  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
## <a name="errors-and-warnings"></a>Ошибки и предупреждения  
 В следующем списке описываются условия, приводящие к сбою хранимой процедуры.  
  
-   У пользователя отсутствуют необходимые разрешения.  
  
  

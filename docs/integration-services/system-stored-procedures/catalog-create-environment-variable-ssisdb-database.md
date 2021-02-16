---
description: catalog.create_environment_variable (база данных SSISDB)
title: catalog.create_environment_variable (база данных SSISDB Database) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91ed017b-6567-4bf2-b9f1-e2b5c70a5343
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7a3b2b55c5a0efbc2acf8152b89a292adf4cc298
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346810"
---
# <a name="catalogcreate_environment_variable-ssisdb-database"></a>catalog.create_environment_variable (база данных SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создайте переменную среды в каталоге служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
catalog.create_environment_variable [ @folder_name = ] folder_name  
    , [ @environment_name = ] environment_name  
    , [ @variable_name = ] variable_name  
    , [ @data_type = ] data_type  
    , [ @sensitive = ] sensitive  
    , [ @value = ] value  
    , [ @description = ] description  
```  
  
## <a name="arguments"></a>Аргументы  
 [@folder_name =] *folder_name*  
 Имя папки, которая содержит среду. Параметр *folder_name* имеет тип **nvarchar(128)** .  
  
 [@environment_name =] *environment_name*  
 Имя среды. Параметр *environment_name* имеет тип **nvarchar(128)** .  
  
 [@variable_name =] *variable_name*  
 Имя переменной среды. Параметр *variable_name* имеет тип **nvarchar(128)**.  
  
 [@data_type =] *data_type*  
 Тип данных переменной. Поддерживаемые типы данных переменной среды: **Boolean**, **Byte**, **DateTime**, **Double**, **Int16**, **Int32**, **Int64**, **Single**, **String**, **UInt32** и **UInt64**. Следующие типы данных в переменной среды не поддерживаются: **Char**, **DBNull**, **Object** и **Sbyte**. Параметр *data_type* имеет тип данных **nvarchar(128)**.  
  
 [@sensitive =] *sensitive*  
 Указывает, содержит переменная конфиденциальное значение или нет. Значение `1` указывает, что значение переменной среды является конфиденциальным, а значение `0` — что оно таковым не является. Конфиденциальное значение шифруется при его сохранении. Неконфиденциальное значение хранится в виде обычного текста. Параметр *Sensitive* имеет тип **bit**.  
  
 [@value =] *value*  
 Значение переменной среды. Параметр *value* имеет тип **sql_variant**.  
  
 [@description =] *description*  
 Описание переменной среды. Параметр *value* имеет тип **nvarchar(1024)**.  
  
## <a name="return-code-value"></a>Значения кодов возврата  
 0 (успешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует применения одного из следующих разрешений:  
  
-   Разрешения READ и MODIFY для среды  
  
-   Членство в роли базы данных **ssis_admin**  
  
-   Членство в роли сервера **sysadmin**  
  
## <a name="errors-and-warnings"></a>Ошибки и предупреждения  
 Следующий список содержит описания некоторых условий, которые могут вызвать ошибку или предупреждение.  
  
-   Недопустимое имя папки, имя среды или имя переменной среды  
  
-   Имя переменной уже существует в среде  
  
-   Пользователь не имеет соответствующих разрешений  
  
## <a name="remarks"></a>Комментарии  
 Переменная среды — это удобный способ присвоения значения параметру проекта или параметру пакета для использования во время выполнения пакета. Переменные среды позволяют организовать значения параметров. Имена переменных должны быть уникальными в пределах среды.  
  
 Хранимая процедура проверяет тип данных переменной, чтобы убедиться в том, что каталог служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] его поддерживает.  
  
> [!TIP]  
>  Можно использовать тип данных **Int16** в [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] вместо неподдерживаемого типа данных **Sbyte**.  
  
 Значение, передаваемое в хранимую процедуру с параметром *value*, будет преобразовано из типа данных служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] в тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] согласно следующей таблице:  
  
|Тип данных служб Integration Services|Тип данных SQL Server|  
|------------------------------------|--------------------------|  
|**Boolean**|**bit**|  
|**Byte**|**binary**, **varbinary**|  
|**DateTime**|**datetime**, **datetime2**, **datetimeoffset**, **smalldatetime**|  
|**Double**|Точное числовое значение: **decimal**, **numeric**; приблизительное числовое значение: **float**, **real**|  
|**Int16**|**smallint**|  
|**Int32**|**int**|  
|**Int64**|**bigint**|  
|**Single**|Точное числовое значение: **decimal**, **numeric**; приблизительное числовое значение: **float**, **real**|  
|**String**|**varchar**, **nvarchar**, **char**|  
|**UInt32**|**int** (**int** — это наиболее близкое доступное сопоставление с **Uint32**.)|  
|**UInt64**|**bigint** (**int** — это наиболее близкое доступное сопоставление с **Uint64**.)|  
  
  

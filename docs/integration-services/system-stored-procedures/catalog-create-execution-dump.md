---
description: catalog.create_execution_dump
title: catalog.create_execution_dump | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91319b0b-5536-4ab4-a403-9559ed9dd177
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e1364857eb48fd5b31e6808eddaae481d3e441d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346800"
---
# <a name="catalogcreate_execution_dump"></a>catalog.create_execution_dump 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Приводит к приостановке выполняемого пакета и создает файл дампа. Файл сохраняется в папке *\<drive>* :\Program Files\Microsoft SQL Server\130\Shared\ErrorDumps.  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
catalog.create_execution_dump [ @execution_id = ] execution_id  
  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @execution_id = ] *execution_id*  
 Идентификатор выполнения для выполняющегося пакета. Параметр *execution_id* имеет тип **bigint**.  
  
## <a name="example"></a>Пример  
 В следующем примере выполняющийся пакет с идентификатором выполнения равным 88 получает запрос для создания файла дампа.  
  
```sql
EXEC create_execution_dump @execution_id = 88  
```  
  
## <a name="return-codes"></a>Коды возврата  
 0 (успешное завершение)  
  
 В случае отказа хранимой процедуры выдается ошибка.  
  
## <a name="result-set"></a>Результирующий набор  
 None  
  
## <a name="permissions"></a>Разрешения  
 Для этой хранимой процедуры требуется, чтобы пользователи были членами роли базы данных **ssis_admin**.  
  
## <a name="errors-and-warnings"></a>Ошибки и предупреждения  
 В следующем списке описываются условия, приводящие к сбою хранимой процедуры.  
  
-   Задан недопустимый идентификатор выполнения.  
  
-   Пакет уже завершен.  
  
-   В настоящее время пакет создает файл дампа.  
  
## <a name="see-also"></a>См. также:  
 [Создание файлов дампа для выполнения пакетов](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
  

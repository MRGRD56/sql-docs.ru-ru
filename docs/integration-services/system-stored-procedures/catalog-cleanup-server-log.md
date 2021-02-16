---
description: catalog.cleanup_server_log
title: catalog.cleanup_server_log | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 0dedb685-d3a6-4bd6-8afd-58d98853deee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3784d6262bc8f3fd2653b55970d3915587a68276
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346889"
---
# <a name="catalogcleanup_server_log"></a>catalog.cleanup_server_log 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Очищает журналы операций, чтобы привести базу данных SSISDB в состояние, позволяющее менять значение свойства SERVER_OPERATION_ENCRYPTION_LEVEL.  
  
## <a name="syntax"></a>Синтаксис  
  
```sql
catalog.cleanup_server_log  
```  
  
## <a name="arguments"></a>Аргументы  
 Нет.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 — успех; 1 — ошибка.  
  
## <a name="result-sets"></a>Результирующие наборы  
 Нет.  
  
## <a name="permissions"></a>Разрешения  
 Эта хранимая процедура требует применения одного из следующих разрешений:  
  
-   разрешения READ и EXECUTE для проекта, а также, если применимо, разрешение READ для среды, указанной в ссылке.  
  
-   Членство в роли базы данных **ssis_admin**.  
  
-   Членство в роли сервера **sysadmin**.  
  
## <a name="errors-and-warnings"></a>Ошибки и предупреждения  
 Эта хранимая процедура вызывает ошибки в указанных ниже случаях.  
  
-   Существует одна или несколько активных операций в базе данных SSISDB.  
  
-   База данных SSISDB не находится в однопользовательском режиме.  
  
## <a name="remarks"></a>Комментарии  
 В SQL Server 2012 с пакетом обновления 2 (SP2) в таблицу **internal.catalog_properties** добавлено свойство SERVER_OPERATION_ENCRYPTION_LEVEL. Оно имеет два возможных значения.  
  
-   **PER_EXECUTION (1)** — сертификат и симметричный ключ, используемые для защиты важных параметров выполнения и журналов выполнения, создаются для каждого выполнения. Из-за создания сертификата и ключей для каждого выполнения в рабочей среде могут возникнуть проблемы с производительностью (взаимоблокировки, сбои заданий обслуживания и т. д.). Однако это значение обеспечивает более высокий уровень безопасности, чем другое (2).  
  
-   **PER_PROJECT (2)** — сертификат и симметричный ключ, используемые для защиты важных параметров, создаются для каждого проекта. PER_PROJECT (2) используется по умолчанию. При этом значении производительность выше, чем при PER_EXECUTION, так как ключ и сертификат создаются для проекта лишь один раз, а не для каждого выполнения.  
  
 Прежде чем изменять значение SERVER_OPERATION_ENCRYPTION_LEVEL с 2 на 1 или с 1 на 2, нужно выполнить хранимую процедуру [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md). Перед ее запуском выполните указанные ниже действия.  
  
1.  Убедитесь в том, что свойству OPERATION_CLEANUP_ENABLED в таблице [catalog.catalog_properties &#40;база данных SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) присвоено значение TRUE.  
  
2.  Переведите базу данных служб Integration Services (SSISDB) в однопользовательский режим. В SQL Server Management Studio откройте диалоговое окно "Свойства базы данных" для базы данных SSISDB, перейдите на вкладку "Параметры" и в качестве значения свойства "Ограничение доступа" выберите однопользовательский режим (SINGLE_USER). После выполнения хранимой процедуры cleanup_server_log восстановите исходное значение свойства.  
  
3.  Выполните хранимую процедуру [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md).  
  
4.  Теперь можно изменить значение свойства SERVER_OPERATION_ENCRYPTION_LEVEL в таблице [catalog.catalog_properties &#40;база данных SSISDB&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md).  
  
5.  Выполните хранимую процедуру [catalog.cleanup_server_execution_keys](../../integration-services/system-stored-procedures/catalog-cleanup-server-execution-keys.md), чтобы удалить ключи и сертификаты из базы данных SSISDB. Удаление сертификатов и ключей из базы данных SSISDB может занять много времени, поэтому эту операцию следует проводить в периоды низкой нагрузки.  
  
     Вы можете указать область действия или уровень (выполнение или проект) и число удаляемых ключей. Размер удаляемого пакета по умолчанию — 1000. Если задан уровень 2, ключи и сертификаты удаляются только при условии, что удалены связанные проекты.  
  
 Дополнительные сведения см. в следующей статье базы знаний: [Устранение проблем с производительностью при использовании базы данных SSISDB в качестве хранилища развертывания в SQL Server 2012](https://support.microsoft.com/kb/2972285).  
  
## <a name="example"></a>Пример  
 В этом примере вызывается хранимая процедура cleanup_server_log.  
  
```sql  
USE [SSISDB]  
GO  
  
DECLARE@return_value int  
EXEC@return_value = [internal].[cleanup_server_log]  
SELECT'Return Value' = @return_value  
GO   
```  
  
  

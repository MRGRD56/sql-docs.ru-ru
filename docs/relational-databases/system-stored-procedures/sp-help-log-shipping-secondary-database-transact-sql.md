---
description: sp_help_log_shipping_secondary_database (Transact-SQL)
title: sp_help_log_shipping_secondary_database (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_secondary_database
- sp_help_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_secondary_database
ms.assetid: 11ce42ca-d3f1-44c8-9cac-214ca8896b9a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a5fc81b903438d874a80399563527747ada9ef43
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199630"
---
# <a name="sp_help_log_shipping_secondary_database-transact-sql"></a>sp_help_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Эта хранимая процедура получает настройки для одной или нескольких баз данных-получателей.  
  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_help_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database' OR  
[ @secondary_id = ] 'secondary_id'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @secondary_database = ] 'secondary_database'` Имя базы данных-получателя. Аргумент *secondary_database* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @secondary_id = ] 'secondary_id'` Идентификатор сервера-получателя в конфигурации доставки журналов. *secondary_id* имеет тип **uniqueidentifier** и не может иметь значение null.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|**secondary_id**|Идентификатор сервера-получателя в конфигурации доставки журналов.|  
|**primary_server**|Имя первичного экземпляра компонента [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] в конфигурации доставки журнала.|  
|**primary_database**|Имя базы данных-источника в конфигурации доставки журналов.|  
|**backup_source_directory**|Каталог, в котором хранятся файлы резервной копии журнала транзакций с сервера-источника.|  
|**backup_destination_directory**|Каталог сервера-получателя, в который копируются файлы резервных копий.|  
|**file_retention_period**|Время в минутах, в течение которого файл резервной копии хранится на сервере-получателе.|  
|**copy_job_id**|Идентификатор, назначенный заданию копирования на сервере-получателе.|  
|**restore_job_id**|Идентификатор, назначенный заданию восстановления на сервере-получателе.|  
|**monitor_server**|Имя экземпляра [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], используемого в качестве сервера мониторинга в конфигурации доставки журналов.|  
|**monitor_server_security_mode**|Режим безопасности, используемый для подключения к серверу мониторинга:<br /><br /> 1 = проверка подлинности [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности.|  
|**secondary_database**|Имя базы данных-получателя в конфигурации доставки журналов.|  
|**restore_delay**|Время ожидания (в минутах), по истечении которого сервер-получатель восстанавливает данный файл резервной копии. Значение по умолчанию — 0 минут.|  
|**restore_all**|Если значение этого аргумента равно 1, сервер-получатель восстанавливает все доступные резервные копии журналов транзакций при выполнении задания восстановления. В противном случае восстанавливается один файл.|  
|**restore_mode**|Режим восстановления базы данных-получателя.<br /><br /> 0 = восстановление журнала с аргументом NORECOVERY.<br /><br /> 1 = восстановить журнал в режиме STANDBY.|  
|**disconnect_users**|Если значение этого аргумента равно 1, пользователи отключаются от базы данных-получателя при выполнении операции восстановления. По умолчанию равно 0.|  
|**block_size**|Размер блока (в байтах) устройства резервного копирования.|  
|**buffer_count**|Общее число буферов, используемых операцией создания резервной копии или восстановления.|  
|**max_transfer_size**|Размер (в байтах) максимального входного или выходного запроса, который [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выдает на устройство резервного копирования.|  
|**restore_threshold**|Время (в минутах), которое может пройти между операциями восстановления, прежде чем сформируется предупреждение.|  
|**threshold_alert**|Предупреждение, создаваемое при истечении порогового срока восстановления.|  
|**threshold_alert_enabled**|Этот аргумент определяет, активированы ли предупреждения об истечении порогового срока восстановления.<br /><br /> 1 = включено.<br /><br /> 0 = отключено.|  
|**last_copied_file**|Имя последнего файла резервной копии, скопированного на сервер-получатель.|  
|**last_copied_date**|Дата и время последней операции копирования на сервер-получатель.|  
|**last_copied_date_utc**|Дата и время последней операции копирования на сервер-получатель по времени в формате UTC.|  
|**last_restored_file**|Имя файла последней резервной копии, восстановленной в базу данных-получатель.|  
|**last_restored_date**|Дата и время последней операции восстановления в базе данных-получателе.|  
|**last_restored_date_utc**|Дата и время последней операции восстановления в базу данных-получатель по времени в формате UTC.|  
|**history_retention_period**|Время (в минутах) хранения истории доставки журналов для конкретной базы данных-получателя; по истечении этого времени записи удаляются.|  
|**last_restored_latency**|Время (в минутах), прошедшее от создания резервной копии журналов в базе данных-источнике до ее восстановления в базу данных-получатель.<br /><br /> Исходное значение равно NULL.|  
  
## <a name="remarks"></a>Замечания  
 Если включить параметр *secondary_database* , результирующий набор будет содержать сведения об этой базе данных-получателе. Если включить параметр *secondary_id* , результирующий набор будет содержать сведения обо всех базах данных-получателях, связанных с этим ВТОРИЧным идентификатором.  
  
 **sp_help_log_shipping_secondary_database** должны запускаться из базы данных **master** на сервере-получателе.  
  
## <a name="permissions"></a>Разрешения  
 Эту процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** .  
  
## <a name="see-also"></a>См. также:  
 [sp_help_log_shipping_secondary_primary &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)   
 [Сведения о доставке журналов (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

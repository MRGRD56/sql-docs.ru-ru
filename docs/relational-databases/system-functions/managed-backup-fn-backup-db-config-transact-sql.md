---
description: managed_backup.fn_backup_db_config managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_backup_db_config (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.fn_backup_db_config
- smart_admin.fn_backup_db_config_TSQL
- fn_backup_db_config
- fn_backup_db_config_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_backup_db_config
- fn_backup_db_config
ms.assetid: 7c755d8a-64dd-44b2-be5e-735d30758900
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 01d2ecc1fffc66d92508b71951ca05d33a224cff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207427"
---
# <a name="managed_backupfn_backup_db_config-transact-sql"></a>managed_backup.fn_backup_db_config managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Возвращает 0, 1 или более строк с параметрами конфигурации «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]». Возвращает 1 строку для указанной базы данных или возвращает все базы данных из экземпляра, в которых настроено «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]».  
  
 Эта хранимая процедура используется для просмотра или определения текущих параметров конфигурации «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]» для базы данных или всех баз данных на экземпляре SQL Server.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
managed_backup.fn_backup_db_config ('database_name' | '' | NULL)  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 @db_name  
 Имя базы данных. @db_nameПараметр имеет тип **sysname**. Если в этом параметре передается пустая строка или значение NULL, возвращаются сведения обо всех базах данных на экземпляре SQL Server.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|db_name|SYSNAME|имя базы данных.|  
|db_guid|UNIQUEIDENTIFIER|Уникальный идентификатор базы данных.|  
|is_availability_database|BIT|Указывает, участвует ли база данных в группе доступности. Значение 1 указывает, что база данных является базой данных доступности, 0 — что не является.|  
|is_dropped|BIT|Значение 1 указывает на то, что данная база данных удалена.|  
|credential_name|SYSNAME|Имя объекта учетных данных SQL, который используется для проверки подлинности учетной записи хранения. Значение NULL указывает, что учетные данные SQL не заданы.|  
|retention_days|INT|Текущий срок хранения в днях. Значение NULL указывает, что настройка «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]» для этой базы данных не производилась.|  
|is_managed_backup_enabled|INT|Показывает, включено ли для базы данных «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]». Значение 1 указывает, что «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]» для этой базы данных в настоящий момент включено, а значение 0 — что «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]» выключено.|  
|storage_url|NVARCHAR (1024)|URL-адрес учетной записи хранения.|  
|Encryption_algorithm|NCHAR (20)|Возвращает текущий алгоритм шифрования, используемый для шифрования резервной копии.|  
|Encryptor_type|NCHAR(15)|Возвращает параметр шифратора: Certificate или асимметричный ключ.|  
|Encryptor_name|NCHAR(max_length_of_cert/asymm_key_name)|Имя сертификата или асимметричного ключа.|  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требуется членство в роли базы данных **db_backupoperator** с разрешениями **ALTER ANY CREDENTIAL** . Пользователю не следует запрещать разрешение **View ANY DEFINITION** .  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращается [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] Конфигурация для "TestDB".  
  
 Для каждого фрагмента кода в поле атрибута языка выберите «tsql».  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config('TestDB')  
```  
  
 В следующем примере возвращается конфигурация «[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]» для всех баз данных в экземпляре SQL Server, где выполняется пример.  
  
```  
Use msdb  
GO  
SELECT * FROM managed_backup.fn_backup_db_config (NULL)  
```  
  
  

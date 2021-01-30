---
description: managed_backup.sp_backup_on_demand managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_on_demand (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- smart_admin.sp_backup_on_demand
- smart_admin.sp_backup_on_demand_TSQL
- sp_backup_on_demand_TSQL
- sp_backup_on_demand
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.sp_backup_on_demand
- sp_backup_on_demand
ms.assetid: 638f809f-27fa-4c44-a549-9cf37ecc920c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 480338a6194b07f15d8c139a6675649e5c4f8e7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205300"
---
# <a name="managed_backupsp_backup_on_demand-transact-sql"></a>managed_backup.sp_backup_on_demand managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Запрашивает у компонента [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] выполнение резервного копирования указанной базы данных.  
  
 Эта хранимая процедура используется для выполнения нерегламентированного резервного копирования для базы данных, настроенной с помощью компонента [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Это предотвращает все перерывы в цепочке резервного копирования и [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] процессах, и резервная копия хранится в том же контейнере хранилища BLOB-объектов Azure.  
  
 При успешном завершении резервного копирования возвращается полный путь к файлу резервной копии. В него включено имя и местоположение нового файла резервной копии, созданного в операции резервного копирования.  
  
 Если [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] находится в процессе выполнения резервного копирования данного типа для указанной базы данных, возвращается ошибка. В этом случае возвращаемое сообщение об ошибке включает полный путь к файлу резервной копии, по которому будет загружена текущая резервная копия.  
   
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
EXEC managed_backup.sp_backup_on_demand   
[@database_name  =]  'database name',[@type = ] { 'Database' | 'Log' }  
  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 @database_name  
 Имя базы данных, для которой будет выполняться резервное копирование. @database_nameАргумент имеет тип **sysname**.  
  
 @type  
 Тип выполняемой резервной копии: база данных или журнал. @typeПараметр имеет тип **nvarchar (32)**.  
  
## <a name="return-code-value"></a>Значения кодов возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требуется членство в роли базы данных **db_backupoperator** , с разрешениями **ALTER ANY CREDENTIAL** и **EXECUTE** для хранимой процедуры **sp_delete_backuphistory**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается запрос на резервное копирование базы данных "TestDB". В базе данных включен компонент [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
```  
Use MSDB  
Go  
EXEC managed_backup.sp_backup_on_demand  
 @database_name = 'TestDB'  
,@type = 'Database'  
  
```  
  
 Для каждого фрагмента кода в поле атрибута языка выберите «tsql».  
  
  

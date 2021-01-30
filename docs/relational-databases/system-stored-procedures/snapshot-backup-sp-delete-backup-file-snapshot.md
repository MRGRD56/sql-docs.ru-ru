---
description: sp_delete_backup_file_snapshot (Transact-SQL)
title: sp_delete_backup_file_snapshot (Transact-SQL) | Документация Майкрософт
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.custom: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5afe5530-a404-4fa5-af3c-bc7c3ca43ce6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0edcaca6a812ed5162aa94a091a5f2d27a83baab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186028"
---
# <a name="sp_delete_backup_file_snapshot-transact-sql"></a>sp_delete_backup_file_snapshot (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Удаляет указанный моментальный снимок резервной копии из указанной базы данных. Используйте эту системную хранимую процедуру совместно с системной функцией **sys.fn_db_backup_file_snapshots** для обнаружения и удаления потерянных моментальных снимков резервных копий. Дополнительные сведения см. в разделе [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.sp_delete_backup_file_snapshot  
    [ @db_name = ] N'<database_name>  
    , [ @snapshot_url = ] N'<snapshot_url>  
```  
  
## <a name="arguments"></a>Аргументы  
 *[ @db_name =] database_name*  
 Имя базы данных, содержащей удаляемый моментальный снимок, предоставленный в виде строки Юникода.  
  
 *[ @snapshot_url =] snapshot_url*  
 URL-адрес удаляемого моментального снимка, указанный в виде строки в Юникоде.  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение ALTER ANY DATABASE.  
  
## <a name="see-also"></a>См. также:  
 [sys.fn_db_backup_file_snapshots &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-db-backup-file-snapshots-transact-sql.md)   
 [sp_delete_backup (Transact-SQL)](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
  
  

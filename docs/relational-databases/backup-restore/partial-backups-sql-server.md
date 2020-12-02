---
title: Частичные резервные копии (SQL Server) | Документация Майкрософт
description: Частичное резервное копирование в SQL Server содержит данные первичной файловой группы, всех файловых групп с режимом доступа "чтение и запись" и (дополнительно) одного или нескольких файлов только для чтения.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full backups [SQL Server]
- partial backups [SQL Server]
- READ_WRITE_FILEGROUPS option
- database backups [SQL Server], about backing up databases
ms.assetid: fe6b6bb1-38d0-46c4-bab8-31df14e8999c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 715eb703a5ae2d09e363c1b572f3645d42e7090f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129239"
---
# <a name="partial-backups-sql-server"></a>Частичные резервные копии (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Все модели восстановления в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживают частичные резервные копии, поэтому данный раздел относится ко всем базам данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Однако частичные резервные копии предназначены для использования в простой модели восстановления с целью повышения гибкости при резервном копировании очень больших баз данных, которые содержат одну или несколько файловых групп только для чтения.  
  
 Частичные резервные копии могут оказаться полезны в тех случаях, когда необходимо исключить файловые группы только для чтения. *Частичная резервная копия* отличается от полной тем, что содержит в себе не все файловые группы. Вместо этого (для баз данных, доступных для записи и чтения) в нее включаются все данные первичной файловой группы, всех файловых групп с режимом доступа «чтение и запись» и (дополнительно) одного или нескольких файлов только для чтения. Частичная резервная копия базы данных, доступной только для чтения, содержит только первичную файловую группу.  
  
> [!NOTE]  
>  Если база данных только для чтения переведена в режим чтения и записи после создания частичной резервной копии, то могут остаться некоторые файловые группы для чтения и записи, не вошедшие в частичную резервную копию. Если это так, то попытка частичного разностного резервного копирования завершится неуспешно. Перед созданием частичной разностной резервной копии базы данных необходимо создать другую частичную резервную копию. Новая частичная резервная копия будет содержать все вторичные файловые группы для чтения и записи и может служить основой для создания частичных разностных резервных копий.  
  
 Резервные копии файловых групп, доступных только для чтения, могут совмещаться с частичными резервными копиями. Сведения о резервных копиях файлов см. в статье [Полные резервные копии файлов (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md).  
  
 Частичная резервная копия может служить *базовой копией для разностного копирования* для частичных разностных резервных копий. Дополнительные сведения см. в разделе [Разностные резервные копии (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
> [!NOTE]  
>  Частичные резервные копии не поддерживаются в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] и мастером планов обслуживания.  
  
 **Создание частичной резервной копии**  
  
-   [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md) (при необходимости, параметр READ_WRITE_FILEGROUPS; FILEGROUP)  
  
 **Использование частичной резервной копии в последовательности восстановления**  
  
-   [Пример. Поэтапное восстановление базы данных &#40;простая модель восстановления&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Пример. Поэтапное восстановление отдельных файловых групп &#40;простая модель восстановления&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
## <a name="see-also"></a>См. также:  
 [Общие сведения о резервном копировании (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Восстановление файлов (простая модель восстановления)](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Поэтапное восстановление (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  

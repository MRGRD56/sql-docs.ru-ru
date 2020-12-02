---
title: 'Оперативное восстановление: доступный для чтения и записи файл (модель полного восстановления)'
description: В этом примере показано оперативное восстановление в SQL Server доступного для чтения и записи файла базы данных с помощью модели полного восстановления с несколькими файловыми группами.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full recovery model [SQL Server], RESTORE example
- online restores [SQL Server], full recovery model
- restore sequences [SQL Server], online
ms.assetid: 0dbeda81-1464-44ba-9011-914900096368
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce68e817070765a6f84a12c518e71de221734b34
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126994"
---
# <a name="example-online-restore-of-a-read-write-file-full-recovery-model"></a>Пример Оперативное восстановление доступного для чтения и записи файла (модель полного восстановления)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Сведения в этом разделе относятся только к базам данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , использующим полную модель восстановления, которые содержат несколько файлов или файловых групп.  
  
 В этом примере база данных `adb`, которая использует модель полного восстановления, содержит три файловые группы. Файловая группа `A` доступна для записи и для чтения, файловые группы `B` и `C` доступны только для чтения. Изначально все файловые группы находятся в режиме в сети.  
  
 Файл `a1` в файловой группе `A` , похоже, поврежден, и администратор базы данных решает восстановить его при сохранении базы данных в режиме «в сети».  
  
> [!NOTE]  
>  При простой модели восстановления восстановление доступных для чтения и записи данных «в сети» не разрешено.  
  
## <a name="restore-sequences"></a>Последовательности восстановления  
  
> [!NOTE]  
>  Синтаксис последовательности восстановления в сети тот же самый, что и в случае последовательности восстановления вне сети.  
  
1.  Восстановление файла `a1`в режиме «в сети».  
  
    ```  
    RESTORE DATABASE adb FILE='a1' FROM backup   
    WITH NORECOVERY;  
    ```  
  
     На данном этапе файл a1 находится в состоянии восстановления (RESTORING), а файловая группа A — в режиме «вне сети».  
  
2.  После восстановления файла администратор базы данных выполняет новое резервное копирование журнала, чтобы удостовериться, что момент перехода файла в режим «вне сети» перехвачен.  
  
    ```  
    BACKUP LOG adb TO log_backup3;   
    ```  
  
3.  Восстановление резервных копий журналов «в сети».  
  
     Администратор производит восстановление всех резервных копий журналов, созданных со времени создания резервной копии восстановленного файла до последней резервной копии журнала (*log_backup3*, сделанной в шаге 2). База данных будет восстановлена после восстановления последней резервной копии.  
  
    ```  
    RESTORE LOG adb FROM log_backup1 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup2 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup3 WITH NORECOVERY;  
    RESTORE DATABASE adb WITH RECOVERY;  
    ```  
  
     Файл `a1` теперь находится в режиме «в сети».  
  
## <a name="additional-examples"></a>Дополнительные примеры  
  
-   [Пример. Поэтапное восстановление базы данных &#40;простая модель восстановления&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Пример. Поэтапное восстановление отдельных файловых групп &#40;простая модель восстановления&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [Пример. Оперативное восстановление доступного только для чтения файла &#40;простая модель восстановления&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Пример. Поэтапное восстановление базы данных &#40;модель полного восстановления&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [Пример. Поэтапное восстановление только некоторых файловых групп &#40;модель полного восстановления&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [Пример. Оперативное восстановление файла, доступного только для чтения &#40;модель полного восстановления&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>См. также:  
 [Восстановление в сети (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Поэтапное восстановление (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [Обзор процессов восстановления (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Применение резервных копий журналов транзакций (SQL Server)](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  

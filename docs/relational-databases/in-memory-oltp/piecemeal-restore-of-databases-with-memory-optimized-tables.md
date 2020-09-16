---
title: Поэтапное восстановление баз данных — оптимизированные для памяти таблицы
description: Базы данных с оптимизированными для памяти таблицами поддерживают поэтапное восстановление в SQL Server. Узнайте основные сценарии для поэтапного резервного копирования и восстановления.
ms.custom: seo-dt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 732c9721-8dd4-481d-8ff9-1feaaa63f84f
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e78c9912b4fa1482d44f7dc9367f70b3c25f04bc
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2020
ms.locfileid: "89551609"
---
# <a name="piecemeal-restore-of-databases-with-memory-optimized-tables"></a>Поэтапное восстановление баз данных с таблицами, оптимизированными для памяти

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Поэтапное восстановление поддерживается в базах данных с таблицами, оптимизированными для памяти, за исключением одного случая, описанного ниже. Дополнительные сведения о поэтапном резервном копировании и восстановлении см. в разделах [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md) и [Поэтапное восстановление (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md).  
  
 Файловая группа, оптимизированная для памяти, должна подвергаться резервному копированию и восстанавливаться вместе с первичной файловой группой.  
  
-   При резервном копировании (или восстановлении) первичной файловой группы необходимо указать оптимизированную для памяти файловую группу.  
  
-   При резервном копировании (или восстановлении) оптимизированной для памяти файловой группы необходимо указать первичную файловую группу.  
  
 Основные сценарии для поэтапного резервного копирования и восстановления.  
  
-   Поэтапное резервное копирование позволяет уменьшить размер копии. Некоторые примеры.  
  
    -   Настройка резервного копирования базы данных на выполнение в разные моменты времени, чтобы свести к минимуму влияние этого процесса на рабочую нагрузку. Одним из примеров будет очень крупная база данных (больше 1 ТБ), где полное резервное копирование базы данных не может быть выполнено за время, отведенное для обслуживания базы данных. В этой ситуации можно использовать поэтапное резервное копирование всей базы за несколько этапов.  
  
    -   Если файловая группа помечена как доступная только для чтения, то не требуется резервное копирование журнала транзакций после того, как группа было помечена как только для чтения. Можно выбрать вариант разового резервного копирования файловой группы после того, как она будет помечена как группа только для чтения.  
  
-   Поэтапное восстановление.  
  
    -   Цель поэтапного восстановления — перевести критические части базы данных в рабочий режим, не ожидая поступления всех данных. Одним из примеров будет таков: база данных имеет секционированные данные, причем старые секции используются только изредка. Их можно восстановить только по мере необходимости. То же верно и для файловых групп, содержащих, например, исторические данные.  
  
    -   С помощью восстановления страниц можно исправить именно поврежденные страницы. Дополнительные сведения см. в разделе [Восстановление страниц (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md).  
  
## <a name="samples"></a>Примеры  
 В примерах используется следующая схема:  
  
```sql
CREATE DATABASE imoltp
    ON PRIMARY (
        name = imoltp_primary1,
        filename = 'c:\data\imoltp_data1.mdf')
    LOG ON (
        name = imoltp_log,
        filename = 'c:\data\imoltp_log.ldf');
    GO  
  
ALTER DATABASE imoltp
    ADD FILE (
        name = imoltp_primary2,
        filename = 'c:\data\imoltp_data2.ndf');
GO  
  
ALTER DATABASE imoltp
    ADD FILEGROUP imoltp_secondary;

ALTER DATABASE imoltp
    ADD FILE (
        name = imoltp_secondary,
        filename = 'c:\data\imoltp_secondary.ndf')
            TO FILEGROUP imoltp_secondary;
GO  
  
ALTER DATABASE imoltp
    ADD FILEGROUP imoltp_mod
    CONTAINS MEMORY_OPTIMIZED_DATA;

ALTER DATABASE imoltp
    ADD FILE (
        name = 'imoltp_mod1',
        filename = 'c:\data\imoltp_mod1')
            TO FILEGROUP imoltp_mod;

ALTER DATABASE imoltp
    ADD FILE (
        name = 'imoltp_mod2',
        filename = 'c:\data\imoltp_mod2')
            TO FILEGROUP imoltp_mod;
GO  
```  
  
### <a name="backup"></a>Резервное копирование  
 Этот пример показывает, как создать резервную копию первичной файловой группы и группы, оптимизированной для памяти. Следует совместно указать и оптимизированную для памяти группу, и первичную файловую группу.  
  
```sql
BACKUP database imoltp
    filegroup = 'primary',
    filegroup = 'imoltp_mod'
    to disk = 'c:\data\imoltp.dmp'
    with init;
```
  
 Следующий пример показывает, что резервное копирование файловой группы (или групп), отличной от первичной и оптимизированной для памяти файловой группы, выполняется так же, как и для баз данных без таблиц, оптимизированных для памяти. Следующая команда создает резервную копию вторичной файловой группы.  
  
```sql
BACKUP database imoltp
    filegroup = 'imoltp_secondary'
    to disk = 'c:\data\imoltp_secondary.dmp'
    with init;
```
  
### <a name="restore"></a>Восстановить  
 В следующем примере показано, как совместно восстановить первичную файловую группу и группу, оптимизированную для памяти.  

```sql
RESTORE database imoltp
    filegroup = 'primary',
    filegroup = 'imoltp_mod'
    from disk = 'c:\data\imoltp.dmp'
    with
        partial,
        norecovery;

-- Restore the transaction log.

RESTORE LOG [imoltp]
    FROM DISK = N'c:\data\imoltp_log.dmp'
    WITH
        FILE = 1,
        NOUNLOAD,
        STATS = 10;
GO
```
  
 Следующий пример показывает, что восстановление файловой группы (или групп), отличной от первичной и оптимизированной для памяти файловой группы, выполняется так же, как и для баз данных без таблиц, оптимизированных для памяти.  
  
```sql
RESTORE DATABASE [imoltp]
    FILE = N'imoltp_secondary'
    FROM DISK = N'c:\data\imoltp_secondary.dmp'
    WITH
        FILE = 1,
        RECOVERY,
        NOUNLOAD,
        STATS = 10;
GO
```

## <a name="see-also"></a>См. также:  
 [Резервное копирование и восстановление оптимизированных для памяти таблиц](https://msdn.microsoft.com/library/3f083347-0fbb-4b19-a6fb-1818d545e281)  


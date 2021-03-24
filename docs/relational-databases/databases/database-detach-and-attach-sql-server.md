---
title: Присоединение и отсоединение базы данных (SQL Server)
description: Можно отсоединить и повторно присоединить файлы данных и журналов транзакций базы данных SQL Server, чтобы переместить базу данных в другой экземпляр или место.
ms.custom: ''
ms.date: 06/30/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- upgrading databases
- databases [SQL Server], detaching
- detach database [SQL Server]
- databases [SQL Server], attaching
- removing databases
- transaction logs [SQL Server], detaching
- databases [SQL Server], removing
- restoring [SQL Server], attached databases
- transaction logs [SQL Server], attaching
- differential backups, after detaching
- moving databases
- attach database [SQL Server]
- detaching databases [SQL Server]
- differential base [SQL Server]
- attaching databases [SQL Server]
- databases [SQL Server], moving
ms.assetid: d0de0639-bc54-464e-98b1-6af22a27eb86
author: stevestein
ms.author: sstein
ms.openlocfilehash: ffa25f98d738b2cac2ec4d796bb946e9bac9e03b
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890750"
---
# <a name="database-detach-and-attach-sql-server"></a>Присоединение и отсоединение базы данных (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Файлы данных и журналов транзакций базы данных можно отсоединить, а затем снова присоединить к тому же или другому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Отсоединение и присоединение базы данных полезно, если необходимо переместить базу данных на другой экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на том же компьютере либо перенести базу данных.  
  
  
##  <a name="security"></a><a name="Security"></a> безопасность  
Разрешения на доступ к файлам устанавливаются во время выполнения определенных операций с базами данных, включая отсоединение и присоединение баз данных.  
  
> [!IMPORTANT]  
> Не рекомендуется подключать или восстанавливать базы данных, полученные из неизвестных или ненадежных источников. В этих базах данных может содержаться вредоносный код, вызывающий выполнение непредусмотренных инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] или появление ошибок из-за изменения схемы или физической структуры базы данных.   
> Перед тем как использовать базу данных, полученную из неизвестного или ненадежного источника, выполните на тестовом сервере инструкцию [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) для этой базы данных, а также изучите исходный код в базе данных, например хранимые процедуры и другой пользовательский код.  
  
##  <a name="detaching-a-database"></a><a name="DetachDb"></a> Отсоединение базы данных  
Отсоединение базы данных означает удаление ее с экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , но сама база данных остается неповрежденной со всеми своими файлами данных и журналов транзакций. Эти файлы затем можно использовать для присоединения базы данных к любому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], включая сервер, от которого она была отсоединена.  
  
Базу данных невозможно отсоединить в следующих случаях.  
  
-   База данных реплицируется и публикуется. При репликации база данных должна быть снята с публикации. Перед тем как отсоединить базу данных, необходимо отключить публикацию, выполнив процедуру [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
    > [!NOTE]  
    > Если невозможно использовать процедуру **sp_replicationdboption**, можно удалить репликацию, выполнив процедуру [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
-   Имеется моментальный снимок базы данных.  
  
    Перед отсоединением базы данных необходимо удалить все моментальные снимки. Дополнительные сведения см. в разделе [Удаление моментального снимка базы данных (Transact-SQL)](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
    > [!NOTE]  
    > Невозможно отсоединить или присоединить моментальный снимок базы данных.  

-   Эта база данных является частью группы доступности AlwaysOn.  
  
    База данных не может быть отсоединена, пока она не будет удалена из группы доступности. Дополнительные сведения см. в разделе [Удаление базы данных — источника из группы доступности Always On](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).
  


-   База данных находится в сеансе зеркального копирования.  
  
    Отключить базу данных невозможно, пока этот сеанс не завершится. Дополнительные сведения см. в разделе [Удаление зеркального отображения базы данных (SQL Server)](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   База данных помечена как подозрительная. Подозрительную базу данных невозможно отсоединить. Для отсоединения ее необходимо перевести в аварийный режим. Дополнительные сведения о переводе базы данных в аварийный режим см. в разделе [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md).  
  
-  База данных является системной базой данных.  
  
### <a name="backup-and-restore-and-detach"></a>Резервное копирование, восстановление и отсоединение  
Для разностных резервных копий отсоединение базы данных, доступной только для чтения, приводит к потере сведений о базовой копии для разностного копирования. Дополнительные сведения см. в разделе [Разностные резервные копии (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
### <a name="responding-to-detach-errors"></a>Реакция на ошибки отсоединения  
Ошибки, возникшие во время отсоединения базы данных, могут воспрепятствовать чистому закрытию базы данных и перестроению журнала транзакций. При получении сообщения об ошибке выполните следующие действие по исправлению.  
  
1.  Заново присоедините все файлы, связанные с базой данных, а не только первичный файл.  
  
2.  Исправьте неполадку, ставшую причиной сообщения об ошибке.  
  
3.  Отсоедините базу данных повторно.  
  
##  <a name="attaching-a-database"></a><a name="AttachDb"></a> Присоединение базы данных  
Можно присоединить скопированную или отсоединенную базу данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Когда базу данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] с файлами полнотекстовых каталогов присоединяют к экземпляру сервера [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , то присоединение файлов каталогов выполняется из их предыдущего расположения вместе с другими файлами баз данных, как и в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Дополнительные сведения см. в разделе [Обновление полнотекстового поиска](../../relational-databases/search/upgrade-full-text-search.md).  
  
При присоединении базы данных должны быть доступны все файлы данных (файлы MDF и NDF). Если у какого-либо файла данных путь отличается от того, каким он был при первом создании или последнем присоединении, необходимо указать текущий путь к файлу.  
  
> [!NOTE]  
> Если присоединяемый первичный файл данных доступен только для чтения, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] предполагает, что и база данных доступна только для чтения.  
  
Когда зашифрованная база данных впервые присоединяется к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], владелец базы данных должен открыть главный ключ базы данных, выполнив следующую инструкцию: `OPEN MASTER KEY DECRYPTION BY PASSWORD = 'password'`. Рекомендуется включить автоматическую расшифровку главного ключа, выполнив следующую инструкцию: `ALTER MASTER KEY ADD ENCRYPTION BY SERVICE MASTER KEY`. Дополнительные сведения см. в разделах [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md) и [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md).  
  
 Требования для присоединения файлов журналов частично зависят от того, доступна база данных для записи и чтения или только для чтения.  
  
-   Для базы данных, доступной для записи и чтения, обычно можно присоединить файл журнала в новом расположении. Однако в некоторых случаях для повторного соединения базы данных требуются файлы ее существующих журналов. Поэтому всегда храните все отсоединенные файлы журналов, пока база данных не будет успешно присоединена без них.  
  
    Если у базы данных, доступной для записи и чтения, только один файл журнала и для него не указано новое расположение, операция присоединения использует старое расположение файла. Если он найден, применяется старый файл журнала независимо от того, была ли база данных выключена чисто. Однако если старый файл журнала не найден и база данных была выключена чисто и не имеет активной цепочки журналов, то операция присоединения пытается построить новый файл журнала для базы данных.  
  
-   Если присоединяемый первичный файл данных доступен только для чтения, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] предполагает, что и база данных доступна только для чтения. Для базы данных, доступной только для чтения, файл или файлы журнала должны быть доступны в расположении, указанном в первичном файле базы данных. Новый файл журнала построить невозможно, так как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не может обновить расположение журнала, указанное в первичном файле.  
 
###  <a name="metadata-changes-on-attaching-a-database"></a><a name="Metadata"></a> Изменение метаданных при присоединении базы данных  
Если база данных, доступная только для чтения, отсоединяется, а затем снова присоединяется, то данные о текущей базовой копии для разностного копирования будут утеряны. *Базовая копия для разностного копирования* — это последняя из полных резервных копий всех данных из базы данных или из подмножества файлов и файловых групп, содержащихся в базе данных. Без сведений о базовой резервной копии база данных **master** утрачивает синхронизацию с базой данных, доступной только для чтения, и дальнейшее создание разностных резервных копий может привести к непредвиденным результатам. Таким образом, если с базой данных, доступной только для чтения, используются разностные резервные копии, то после повторного присоединения базы данных необходимо установить новую базовую копию для разностного копирования, создав полную резервную копию. Сведения о разностных резервных копиях см. в разделе [Разностные резервные копии (SQL Server)](../../relational-databases/backup-restore/differential-backups-sql-server.md).  
  
После присоединения происходит запуск базы данных. Обычно присоединение базы данных переводит ее в то же состояние, в котором она находилась на момент отсоединения или копирования. Однако операции присоединения и отсоединения отключают создание межбазовых цепочек владения для этой базы данных. Сведения о том, как включить цепочки владения, см. в разделе [Параметр конфигурации сервера "cross db ownership chaining"](../../database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option.md). 

 > [!IMPORTANT]
 > По умолчанию в целях безопасности параметры *is_broker_enabled*, *is_honor_broker_priority_on* и *is_trustworthy_on* устанавливаются в значение OFF при подключении базы данных. Сведения о том, как установить эти параметры в значение ON, см. в статье [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  Дополнительные сведения о метаданных см. в статье [Управление метаданными при предоставлении доступа к базе данных на другом сервере](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).
  
### <a name="backup-and-restore-and-attach"></a>Резервное копирование, восстановление и присоединение  
Подобно любой базе данных, которая полностью или частично вне сети, база данных с восстановлением файлов не может быть присоединена. Базу данных можно присоединить после остановки последовательности восстановления. Затем можно снова запустить последовательность восстановления.  
  
###  <a name="attaching-a-database-to-another-server-instance"></a><a name="OtherServerInstance"></a> Присоединение базы данных к другому экземпляру сервера  
  
> [!IMPORTANT]  
> База данных, созданная в более поздней версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], не может быть присоединена в ранних версиях. Это физически исключает возможность использования базы данных с более старой версией [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Тем не менее это относится к состоянию метаданных и не влияет на [режим совместимости базы данных](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md). Дополнительные сведения см. в разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).   
  
При присоединении базы данных к другому экземпляру сервера для обеспечения ее согласованного функционирования для пользователей и приложений может понадобиться повторное создание некоторых или всех метаданных базы данных, например имен входа и задания, на другом экземпляре сервера. Дополнительные сведения см. в статье [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
**Отсоединение базы данных**  
  
-   [sp_detach_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
-   [Отсоединение базы данных](../../relational-databases/databases/detach-a-database.md)  
  
**Присоединение базы данных**  
  
-   [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)  
  
-   [Присоединение базы данных](../../relational-databases/databases/attach-a-database.md)  
  
-   [sp_attach_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md)  
  
-   [sp_attach_single_file_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql.md)  
  
**Обновление базы данных при помощи операций отсоединения и присоединения**  
  
-   [Обновление базы данных при помощи отсоединения и присоединения (Transact-SQL)](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md)  
  
**Перемещение базы данных при помощи операций отсоединения и присоединения**  
  
-   [Перенос базы данных путем отсоединения и присоединения (язык Transact-SQL)](../../relational-databases/databases/move-a-database-using-detach-and-attach-transact-sql.md)  
  
**Удаление моментального снимка базы данных**  
  
-   [Удаление моментального снимка базы данных (Transact-SQL)](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [Файлы и файловые группы базы данных](../../relational-databases/databases/database-files-and-filegroups.md)  
  

---
title: Состояния базы данных | Документация Майкрософт
description: Узнайте о различных состояниях базы данных, например ONLINE, OFFLINE или SUSPECT. Узнайте, как проверить текущее состояние базы данных.
ms.custom: ''
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.DATABASESTATES.F1
helpviewer_keywords:
- emergency database state [SQL Server]
- verifying database states
- viewing database states
- displaying database states
- database states [SQL Server]
- current database state
- offline database state [SQL Server]
- suspect database state
- recovery pending database state [SQL Server]
- states [SQL Server], databases
- online database state
- states [SQL Server]
- restoring database state [SQL Server]
ms.assetid: b7f1f111-ca73-4a89-b567-a98d64d6ecb3
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 85b7aee1d344ff9fe70d17d6bdaadde1497bfc4e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748156"
---
# <a name="database-states"></a>Состояния базы данных
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  База данных всегда находится в определенном состоянии. Например, к этим состояниям относятся состояния ONLINE, OFFLINE или SUSPECT. Чтобы проверить текущее состояние базы данных, выберите столбец **state_desc** в представлении каталога [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) или свойство **Status** в функции [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) .  
  
## <a name="database-state-definitions"></a>Определения состояний базы данных  
 Состояния базы данных определяются в следующей таблице.  
  
|Состояние|Определение|  
|-----------|----------------|  
|ONLINE|База данных доступна. Первичная файловая группа находится в режиме в сети, хотя возможно не завершена стадия отката восстановления.|  
|OFFLINE|База данных недоступна. База данных переходит в режим вне сети с помощью явного указания пользователя и остается в режиме вне сети до тех пор, пока пользователем не будет предпринято дополнительное действие. Например, база данных может быть переведена в режим вне сети, чтобы переместить файл на другой диск. После завершения перемещения файла база данных снова переводится в режим в сети.|  
|RESTORING|Восстанавливаются один или несколько файлов, принадлежащих к первичной файловой группе, или один или более файлов, принадлежащих ко вторичным группам, причем база данных остается в режиме в сети. База данных недоступна.|  
|RECOVERING|База данных в процессе восстановления. Процесс восстановления является переходным состоянием; после успешного завершения восстановления база данных автоматически переходит в режим вне сети. При неудачном завершении восстановления база данных будет помечена как подозрительная. База данных недоступна.|  
|RECOVERY PENDING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] во время восстановления произошла ошибка, связанная с ресурсами. База данных не повреждена, но, возможно, потеряны файлы или ограничения системных ресурсов препятствуют началу процесса восстановления. База данных недоступна. Со стороны пользователя требуется дополнительное действие, чтобы исправить ошибку и разрешить завершение процесса восстановления.|  
|SUSPECT|По меньшей мере, первичная файловая группа помечена как подозрительная и, возможно, повреждена. База данных не может быть восстановлена во время запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. База данных недоступна. Со стороны пользователя требуется дополнительное действие, чтобы устранить проблему.|  
|EMERGENCY|Пользователь изменил базу данных и установил состояние базы данных в значение EMERGENCY. База данных находится в однопользовательском режиме и, возможно, в процессе исправления или восстановления. База данных помечена как READ_ONLY, ведение журнала отключено и доступ возможен только элементам предопределенной роли сервера **sysadmin** . EMERGENCY используется в основном для диагностики. Например, база данных, помеченная как подозрительная, может быть переведена в состояние EMERGENCY. Это предоставляет системному администратору доступ к базе данных только для чтения. Только члены предопределенной роли сервера **sysadmin** могут перевести базу данных в состояние EMERGENCY.|  
  
## <a name="related-content"></a>См. также  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  
 [Состояния зеркального отображения (SQL Server)](../../database-engine/database-mirroring/mirroring-states-sql-server.md)  
  
 [Состояния файлов](../../relational-databases/databases/file-states.md)  
  
  

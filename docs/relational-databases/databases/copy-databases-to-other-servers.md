---
title: Копирование баз данных на другие серверы | Документация Майкрософт
description: Узнайте, как скопировать базу данных SQL Server с одного компьютера на другой и обеспечить удаленный доступ к ней.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- servers [SQL Server], copying databases between
- bulk exporting [SQL Server], between servers
- database copying [SQL Server]
- migrating databases [SQL Server]
- moving databases
- copying databases
- bulk importing [SQL Server], between servers
ms.assetid: 978406d6-a3c8-4902-b1f4-4ced75234be5
author: stevestein
ms.author: sstein
ms.openlocfilehash: 55560b212f692b19513d211a69f38efb80b75883
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194400"
---
# <a name="copy-databases-to-other-servers"></a>Копирование баз данных на другие серверы
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В некоторых случаях можно скопировать базу данных с одного компьютера на другой и использовать ее для тестирования, проверки согласованности данных, разработки ПО, выполнения отчетов, создания зеркальной базы данных или предоставления доступа к базе данных сотрудникам удаленного филиала.  
  
 Скопировать базу данных можно одним из следующих способов.  
  
-   Использование мастера копирования баз данных  
  
     Мастер копирования баз данных можно использовать для копирования или перемещения баз данных между серверами, а также для обновления базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до более новой версии. Дополнительные сведения см. в статье [Use the Copy Database Wizard](../../relational-databases/databases/use-the-copy-database-wizard.md).  
  
-   Восстановление базы данных из резервной копии  
  
     Чтобы копировать целую базу данных, можно использовать инструкции BACKUP и RESTORE языка [!INCLUDE[tsql](../../includes/tsql-md.md)] . Выбор методики восстановления базы данных из полной резервной копии для копирования базы данных с одного компьютера на другой может быть мотивирован разными причинами. Сведения о копировании базы данных путем восстановления из резервной копии см. в статье [Копирование баз данных путем создания и восстановления резервных копий](../../relational-databases/databases/copy-databases-with-backup-and-restore.md).  
  
    > [!NOTE]  
    >  Чтобы настроить зеркальную базу данных для зеркального отображения базы данных, необходимо восстановить базу данных на зеркальном сервере с помощью инструкции RESTORE DATABASE *<имя_базы_данных>* WITH NORECOVERY. Дополнительные сведения см. в статье [Prepare a Mirror Database for Mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
-   Использование мастера создания скриптов для публикации баз данных  
  
     Мастер создания скриптов позволяет передать базу данных с локального компьютера на веб-поставщик услуг размещения. Дополнительные сведения см. в статье [Мастер формирования и публикации скриптов](../../ssms/scripting/generate-and-publish-scripts-wizard.md).  
  

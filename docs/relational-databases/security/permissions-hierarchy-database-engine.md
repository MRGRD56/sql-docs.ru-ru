---
title: Иерархия разрешений (ядро СУБД) | Документация Майкрософт
description: Сведения об иерархии сущностей, которые могут быть защищены с помощью разрешений, известных как защищаемые объекты, в ядре СУБД SQL Server.
ms.custom: ''
ms.date: 03/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.server.permissions.f1--May use common.permissions
helpviewer_keywords:
- security [SQL Server], denying access
- hierarchies [SQL Server], permissions
- securables [SQL Server]
- security [SQL Server], permissions
- permissions [SQL Server], hierarchy
- security [SQL Server], granting access
ms.assetid: f6d20a55-ef03-4e14-85f9-009902889866
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd021a3306d7bca7d72f02430d52d787c06a23a2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748064"
---
# <a name="permissions-hierarchy-database-engine"></a>Иерархия разрешений (компонент Database Engine)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] управляет иерархической коллекцией сущностей, защита которых производится при помощи разрешений. Эти сущности называются *защищаемыми объектами*. Важнейшими защищаемыми компонентами являются серверы и базы данных, однако отдельные разрешения можно задавать на гораздо более глубоком уровне. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] регулирует действия участников на защищаемых объектах, проверяя, что им были предоставлены соответствующие разрешения.  
  
 На следующей иллюстрации показана связь между иерархиями разрешений компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Система разрешений работает одинаково во всех версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)], [!INCLUDE[ssDW](../../includes/ssdw-md.md)], [!INCLUDE[ssAPS](../../includes/ssaps-md.md)], однако некоторые возможности доступны не во всех версиях. Например нельзя настроить разрешение уровня сервера в продуктах Azure.  
  
 ![Диаграмма иерархий разрешений в ядре СУБД](../../relational-databases/security/media/wj-security-layers.gif "Диаграмма иерархий разрешений в ядре СУБД")  
  
## <a name="chart-of-sql-server-permissions"></a>Диаграмма разрешений SQL Server  
 Схему плакатного размера всех разрешений компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] в формате PDF см. по ссылке [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster).  
  
## <a name="working-with-permissions"></a>Работа с разрешениями  
 Выполнение различных действий с разрешениями осуществляется при помощи обычных запросов [!INCLUDE[tsql](../../includes/tsql-md.md)] : GRANT, DENY и REVOKE. Сведения о разрешениях доступны через представления каталога [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) и [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) . Существует также поддержка запроса сведений о разрешениях при помощи встроенных функций.  
  
 Сведения о проектировании системы разрешений см. в статье [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
## <a name="see-also"></a>См. также:  
 [Обеспечение безопасности SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.server_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  

---
title: Защищаемые объекты | Документация Майкрософт
description: Сведения об областях защищаемых объектов, которые система авторизации ядра СУБД SQL Server использует для регулирования доступа к защищаемым объектам.
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectobject.f1
helpviewer_keywords:
- securables [SQL Server]
- schemas [SQL Server], securables
- database securables [SQL Server]
- hierarchies [SQL Server], securables
- server securables [SQL Server]
ms.assetid: bfa748f0-70b0-453c-870a-04b7b205b9ff
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 87929f3e4f78d89cee514b46083d239335c2222f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739904"
---
# <a name="securables"></a>Защищаемые объекты
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  К защищаемым объектами относятся ресурсы, доступ к которым регулируется системой авторизации компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Например, защищаемым объектом является таблица. Некоторые защищаемые объекты могут храниться внутри других, создавая иерархии «областей», которые сами могут защищаться. К областям защищаемых объектов относятся **сервер**, **база данных** и **схема**.  
  
## <a name="securable-scope-server"></a>Область защищаемых объектов: Сервер  
 Область защищаемых объектов **сервера** содержит следующие защищаемые объекты:  
  
-   группа доступности  
  
-   Конечная точка  
  
-   Имя входа  
  
-   Роль сервера  
  
-   База данных  
  
## <a name="securable-scope-database"></a>Область защищаемых объектов: База данных  
 Область защищаемых объектов **базы данных** содержит следующие защищаемые объекты:  
  
-   Роль приложения  
  
-   Сборка  
  
-   Асимметричный ключ  
  
-   Сертификат  
  
-   Контракт  
  
-   Полнотекстовый каталог  
  
-   Полнотекстовый список стоп-слов  
  
-   тип сообщений;  
  
-   Привязка удаленной службы  
  
-   Роль (база данных)  
  
-   Маршрут  
  
-   схема  
  
-   Поиск в списке свойств  
  
-   Служба  
  
-   Симметричный ключ  
  
-   Пользователь  
  
## <a name="securable-scope-schema"></a>Область защищаемых объектов: схема  
 Область защищаемых объектов **схемы** содержит следующие защищаемые объекты:  
  
-   Тип  
  
-   Коллекция схем XML  
  
-   Объект — к классу объекта относятся следующие элементы:  
  
    -   Статистическое  
  
    -   Компонент  
  
    -   Процедура  
  
    -   Очередь  
  
    -   Синоним  
  
    -   Таблица  
  
    -   Представление 
    
    -   Внешняя таблица 
  
## <a name="controlling-access-to-a-securable"></a>Управление доступом к защищаемому объекту  
 Сущность, которая получает разрешение на доступ к защищаемому объекту, именуется участником. Наиболее часто в роли участников выступают имена входа и пользователи базы данных. Управление доступом к защищаемым объектам осуществляется путем предоставления или отзыва разрешений либо путем добавления имен входа и пользователей к ролям, имеющим доступ. Сведения об управлении разрешениями см. в разделах [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md), [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md), [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md), [Хранимая процедура sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) и [sp_droprolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md).  
  
> [!CAUTION]  
>  Разрешения по умолчанию, которые предоставляются системным объектам во время установки, тщательно оцениваются на предмет возможных угроз, и их не нужно будет изменять для защиты установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Любые изменения разрешений в системных объектах могут ограничить или нарушить функциональность, а также перевести установку [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в неподдерживаемое состояние.  
  
## <a name="related-content"></a>См. также  
 [Приступая к работе с разрешениями Database Engine](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
 [Обеспечение безопасности SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
 [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)  
  
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
 [sys.server_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
 [sys.sql_logins (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)  
  
  

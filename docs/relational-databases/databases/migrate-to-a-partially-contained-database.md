---
description: Migrate to a Partially Contained Database
title: Миграция на частично автономную базу данных | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- contained database, migrating to
ms.assetid: 90faac38-f79e-496d-b589-e8b2fe01c562
author: stevestein
ms.author: sstein
ms.openlocfilehash: 2a942af36a551498e81b3dbaeaa02686dec9c28b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88411000"
---
# <a name="migrate-to-a-partially-contained-database"></a>Migrate to a Partially Contained Database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как подготовить к переходу на частично автономную модель базы данных, и приведены шаги по миграции.  
  
 **В этом разделе:**  
  
-   [Подготовка к миграции базы данных](#prepare)  
  
-   [Включение автономных баз данных](#enable)  
  
-   [Преобразование базы данных в частично автономную](#convert)  
  
-   [Миграция пользователей в пользователей автономной базы данных](#users)  
  
##  <a name="preparing-to-migrate-a-database"></a><a name="prepare"></a> Подготовка к миграции базы данных  
 Перед началом перевода базы данных в частичную автономную модель просмотрите следующие указания.  
  
-   Для этого потребуется понимание модели частично автономной базы данных. Дополнительные сведения см. в разделе [Contained Databases](../../relational-databases/databases/contained-databases.md).  
  
-   Необходимо понимать риски, связанные с частично автономными базами данных. Дополнительные сведения см. в разделе [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
-   Автономные базы данных не поддерживают репликацию, отслеживание измененных данных или отслеживание изменений. Убедитесь, что в базе данных не используются эти функции.  
  
-   Просмотрите список функций базы данных, которые изменились для частично автономных баз данных. Дополнительные сведения см. в разделе [Измененные функции (автономная база данных)](../../relational-databases/databases/modified-features-contained-database.md).  
  
-   Выполнив запрос к представлению [sys.dm_db_uncontained_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) , найдите в базе данных неавтономные объекты или функции. Дополнительные сведения см. в разделе  
  
-   Мониторьте XEvent **database_uncontained_usage** , чтобы определить, когда используются неавтономные функции.  
  
##  <a name="enable-contained-databases"></a><a name="enable"></a> Включение автономных баз данных  
 Перед созданием автономных баз данных поддержка автономных баз данных должна быть включена на экземпляре [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
### <a name="enabling-contained-databases-using-transact-sql"></a>Включение автономных баз данных с помощью Transact-SQL  
 В следующем примере в экземпляре компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]создаются автономные базы данных.  
  
```sql  
sp_configure 'contained database authentication', 1;  
GO  
RECONFIGURE ;  
GO  
```  
  
#### <a name="enabling-contained-databases-using-management-studio"></a>Включение автономных баз данных с помощью среды Management Studio  
 В следующем примере в экземпляре компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]создаются автономные базы данных.  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши имя сервера и выберите пункт **Свойства**.  
  
2.  На странице **Расширенные** в разделе **Включение** установите параметр **Включить автономные базы данных** в значение **True**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

##  <a name="converting-a-database-to-partially-contained"></a><a name="convert"></a> Преобразование базы данных в частично автономную  
 База данных преобразуется в автономную путем изменения ее параметра **CONTAINMENT** .  
  
### <a name="converting-a-database-to-partially-contained-using-transact-sql"></a>Преобразование базы данных в частично автономную с помощью Transact-SQL  
 В следующем примере база данных показано преобразование `Accounting` в частично автономную базу данных.  
  
```sql  
USE [master]  
GO  
ALTER DATABASE [Accounting] SET CONTAINMENT = PARTIAL  
GO  
```  
  
### <a name="converting-a-database-to-partially-contained-using-management-studio"></a>Преобразование базы данных в частично автономную с помощью среды Management Studio  
 В следующем примере база данных преобразуется в частично автономную базу данных.  
  
1.  В обозревателе объектов разверните узел **Базы данных**, щелкните правой кнопкой мыши базу данных, которую нужно преобразовать, а затем выберите пункт **Свойства**.  
  
2.  На странице **Параметры** измените параметр **Тип вложения** на **Частичный**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="migrating-users-to-contained-database-users"></a><a name="users"></a> Миграция пользователей в пользователей автономной базы данных  
 В следующем примере выполняется миграция всех пользователей, основанных на имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , в пользователей автономной базы данных с паролями. Этот пример исключает имена входа, которые не были включены. Этот пример должен выполняться в автономной базе данных.  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a>См. также:  
 [Автономные базы данных](../../relational-databases/databases/contained-databases.md)   
 [sp_migrate_user_to_contained (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)   
 [sys.dm_db_uncontained_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)  
  
  

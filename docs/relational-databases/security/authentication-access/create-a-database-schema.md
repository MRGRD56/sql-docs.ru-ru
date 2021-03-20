---
title: Создание схемы базы данных | Документация Майкрософт
description: Сведения о том, как создать схему в SQL Server с помощью SQL Server Management Studio или Transact-SQL, включая соответствующие ограничения
ms.custom: ''
ms.date: 07/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.schemas.general.f1
helpviewer_keywords:
- creating schemas with Management Studio
- CREATE SCHEMA [Management Studio]
- database schemas
- schemas [SQL Server], creating
ms.assetid: ed2a5522-f4d2-4111-95a4-d3e1e5081739
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f0bbcabd2483d6bab39221f068c3f6b20f810432
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104733537"
---
# <a name="create-a-database-schema"></a>Создание схемы базы данных
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  В этом разделе описывается создание схемы в [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Новая схема принадлежит одному из следующих участников уровня базы данных: пользователю базы данных, роли базы данных или роли приложения. Объекты, создаваемые в схеме, принадлежат владельцу схемы и имеют значение NULL для **principal_id** в **sys.objects**. Владение объектами, содержащимися в схеме, можно передать любому участнику уровня базы данных, однако у владельца схемы всегда остается разрешение CONTROL на объекты в схеме.  
  
-   Если при создании объекта базы данных указать допустимый субъект домена (пользователя или группу) в качестве владельца объекта, то этот субъект добавляется в базу данных в качестве схемы. Новая схема принадлежит этому субъекту домена.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
  
-   Требует разрешения CREATE SCHEMA в базе данных.  
  
-   Чтобы назначить другого пользователя владельцем создаваемой схемы, у участника должно быть разрешение IMPERSONATE на этого пользователя. Если роль базы данных указана в качестве владельца, то вызывающий объект должен входить в роль или иметь на нее разрешение ALTER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
##### <a name="to-create-a-schema"></a>Создание схемы  
  
1.  В обозревателе объектов раскройте папку **Базы данных** .  
  
2.  Разверните базу данных, в которой создается новая схема базы данных.  
  
3.  Щелкните правой кнопкой мыши папку **Безопасность** , укажите на пункт **Создать** и выберите **Схема**.  
  
4.  В диалоговом окне **Схема — создать** на странице **Общие** введите имя новой схемы в поле **Имя схемы** .  
  
5.  В поле **Владелец схемы** введите имя пользователя или роли базы данных, которые будут владельцем схемы. Также можно нажать кнопку **Поиск** , чтобы открыть диалоговое окно **Поиск ролей и пользователей** .  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

> [!NOTE]
> Диалоговое окно не будет отображаться, если вы создаете схему с помощью SSMS для **Базы данных SQL Azure** или **Azure Synapse Analytics**. Потребуется создать схему шаблона T-SQL.
  
### <a name="additional-options"></a>Дополнительные параметры  
 Диалоговое окно **Схема — создать** также содержит параметры на двух дополнительных страницах: **Разрешения** и **Расширенные свойства**.  
  
-   На странице **Разрешения** перечислены все возможные защищаемые объекты и разрешения на эти объекты, которые могут быть предоставлены для имени входа.  
  
-   Страница **Расширенные свойства** позволяет добавлять пользовательские свойства пользователям базы данных.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-create-a-schema"></a>Создание схемы  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  В следующем примере создается схема `Chains`, а затем таблица `Sizes`.  
    ```sql  
    CREATE SCHEMA Chains;
    GO
    CREATE TABLE Chains.Sizes (ChainID int, width dec(10,2));
    ```

4.  Дополнительные операции могут быть выполнены в рамках одной инструкции. В следующем примере создается принадлежащая Annik схема `Sprockets`, которая содержит таблицу `NineProngs`. Инструкция предоставляет разрешение `SELECT` для Mandar и запрещает `SELECT` для Prasanna.  

    ```sql  
    CREATE SCHEMA Sprockets AUTHORIZATION Annik  
        CREATE TABLE NineProngs (source int, cost int, partnumber int)  
        GRANT SELECT ON SCHEMA::Sprockets TO Mandar  
        DENY SELECT ON SCHEMA::Sprockets TO Prasanna;  
    GO  
    ```  
5. Чтобы просмотреть схемы в этой базе данных, выполните следующую инструкцию.

   ```sql
   SELECT * FROM sys.schemas;
   ```

 Дополнительные сведения см. в разделе [CREATE SCHEMA (Transact-SQL)](../../../t-sql/statements/create-schema-transact-sql.md).  
  
  

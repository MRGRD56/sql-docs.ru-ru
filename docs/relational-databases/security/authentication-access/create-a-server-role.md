---
title: Создание роли сервера | Документация Майкрософт
description: Создайте роль сервера в SQL Server с помощью SQL Server Management Studio или Transact-SQL. Изучите ограничения и необходимые разрешения.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.SERVERROLE.GENERAL.F1
- sql13.swb.serverrole.memberships.f1
- sql13.swb.serverrole.members.f1
helpviewer_keywords:
- SERVER ROLE, creating
ms.assetid: 74f19992-8082-4ed7-92a1-04fe676ee82d
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5de0726185f6ba294c1527fbb709ba3f883b70d5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479355"
---
# <a name="create-a-server-role"></a>Создание роли сервера
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]
  В этом разделе описывается создание новой роли сервера в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Безопасность](#Security)  
  
-   **Создание новой роли сервера с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
 Ролям сервера нельзя предоставить разрешение на защищаемые объекты уровня базы данных. Сведения о создании ролей базы данных см. в статье [CREATE ROLE (Transact-SQL)](../../../t-sql/statements/create-role-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
  
-   Требует разрешения CREATE SERVER ROLE или членства в предопределенной роли сервера.  
  
-   Также требует разрешения IMPERSONATE на *server_principal* для имен входа, разрешения ALTER для ролей сервера, которые используются в качестве *server_principal*, или членства в группе Windows, которая используется в качестве server_principal.  
  
-   Если для назначения владельца роли сервера указывается параметр AUTHORIZATION, необходимы также следующие разрешения.  
  
    -   Для передачи роли сервера во владение другому имени входа необходимо связанное с этим именем входа разрешение IMPERSONATE.  
  
    -   Для передачи роли сервера во владение другой роли сервера необходимо членство в роли сервера-получателя или связанное с этой ролью сервера разрешение ALTER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-create-a-new-server-role"></a>Создание новой роли сервера  
  
1.  В обозревателе объектов разверните сервер, на котором необходимо создать новую роль сервера.  
  
2.  Разверните папку **Безопасность** .  
  
3.  Щелкните правой кнопкой мыши папку **Роли сервера** и выберите **Создать роль сервера…** .  
  
4.  В диалоговом окне **Создание роли сервера —** _имя\_роли\_сервера_ на странице **Общие** введите имя новой роли сервера в поле **Имя роли сервера**.  
  
5.  В поле **Владелец** введите имя участника на уровне сервера, которому будет принадлежать новая роль. Также можно нажать кнопку с многоточием **(...)** , чтобы открыть диалоговое окно **Выбор имени входа или роли сервера**.  
  
6.  В поле **Защищаемые объекты** выберите один или несколько защищаемых объектов уровня сервера. Когда выбран защищаемый объект, роли сервера могут быть предоставлены или запрещены разрешения для этого объекта.  
  
7.  В поле **Явные разрешения** установите флажок, чтобы предоставить, предоставить с правом передачи или отменить разрешение на выбранные защищаемые объекты для этой роли сервера. Если разрешение не может быть предоставлено или отменено для всех выделенных защищаемых объектов, разрешение представляется как частично выбранное.  
  
8.  На странице **Члены** с помощью кнопки **Добавить** добавьте имена входа, представляющие определенных пользователей или их группы, в новую роль сервера.  
  
9. Определяемая пользователем роль сервера может быть членом другой роли сервера. На странице **Членство** установите флажок, чтобы текущая, определяемая пользователем роль сервера стала членом выбранной роли сервера.  
  
10. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-create-a-new-server-role"></a>Создание новой роли сервера  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    --Creates the server role auditors that is owned the securityadmin fixed server role.  
    USE master;  
    CREATE SERVER ROLE auditors AUTHORIZATION securityadmin;  
    GO  
    ```  
  
 Дополнительные сведения см. в разделе [CREATE SERVER ROLE (Transact-SQL)](../../../t-sql/statements/create-server-role-transact-sql.md).  
  
  

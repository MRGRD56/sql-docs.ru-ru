---
description: Управление именами входа в списке доступа к публикации
title: Управление именами для входа в списке доступа к публикации | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- logins [SQL Server replication], publication access list
- publications [SQL Server replication], publication access lists
- publication access list (PAL)
- PAL (publication access list)
- logins [SQL Server replication], managing
ms.assetid: fceb216b-0b18-4e3b-8ae0-13e35920dcbc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 526d46149bbafccf8592a20b8a592012e3d6b6aa
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250653"
---
# <a name="manage-logins-in-the-publication-access-list"></a>Управление именами входа в списке доступа к публикации
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  В этом разделе описывается управление именами входа в списке доступа к публикации в [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Доступом к публикации управляет список доступа к публикации (PAL). Имена входа и группы можно добавлять в список доступа к публикации и удалять из него.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Предварительные требования](#Prerequisites)  
  
-   **Для управления именами входа в списке доступа к публикации используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Перед добавлением имени входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в список доступа к публикации необходимо связать его с пользователем базы данных публикации.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Управление именами для входа в списке доступа к публикации (PAL) осуществляется на странице **Список доступа к публикации** диалогового окна **Свойства публикации — \<Publication>** . Дополнительные сведения о доступе к этому диалоговому окну см. в статье [Просмотр и изменение свойств публикации](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-manage-logins-in-the-pal"></a>Управление именами входа в списке доступа к публикации (PAL)  
  
1.  На странице **Список доступа к публикации** диалогового окна **Свойства публикации — \<Publication>** используйте кнопки **Добавить**, **Удалить** и **Удалить все** для добавления или удаления имен для входа и групп в списке доступа к публикации (PAL). Не удаляйте учетную запись **distributor_admin** из списка доступа к публикации. Эта учетная запись используется при репликации.  
  
    > [!NOTE]  
    >  Если используется удаленный распространитель, то учетные записи в списке доступа к публикации должны быть доступны как на издателе, так и на распространителе. Это должны быть либо учетные записи домена, либо локальные учетные записи, определенные на обоих серверах. Пароли, связанные с обоими именами входа, должны совпадать.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-view-groups-and-logins-that-belong-to-the-pal"></a>Просмотр групп и имен входа из списка доступа к публикации  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_help_publication_access](../../../relational-databases/system-stored-procedures/sp-help-publication-access-transact-sql.md). Для параметра `@publication` укажите имя публикации. Будут отображены сведения о группах и именах входа из списка доступа к публикации.  
  
#### <a name="to-add-groups-and-logins-to-the-pal"></a>Добавление групп и имен входа в список доступа к публикации  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_grant_publication_access](../../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md). Для параметра `@publication` укажите имя публикации, а в параметре `@login` — имя входа или имя группы, которое нужно добавить.  
  
#### <a name="to-remove-groups-and-logins-from-the-pal"></a>Удаление групп и имен входа из списка доступа к публикации  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_revoke_publication_access](../../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md). Для параметра `@publication` укажите имя публикации, а в параметре `@login` — имя входа или имя группы, которое нужно удалить.  
  
## <a name="see-also"></a>См. также:  
 [Управление именами входа в списке доступа к публикации](../../../relational-databases/replication/security/manage-logins-in-the-publication-access-list.md)   
 [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md)  (Модель безопасности агента репликации)  
 [Защита топологии репликации](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [Защита издателя](../../../relational-databases/replication/security/secure-the-publisher.md)  
  
  

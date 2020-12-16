---
description: Идентификатор и управление доступом (репликация)
title: Идентификатор и контроль доступа (репликация) | Документация Майкрософт
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- access controls [SQL Server replication]
- security [SQL Server replication], identity and access control
- authentication [SQL Server replication]
- identity [Replication]
ms.assetid: 4da0e793-1ee4-4f69-a80b-45c6732a238d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 8737fa7c4781110c4a9169a7334e88f7df734291
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479765"
---
# <a name="identity-and-access-control-replication"></a>Идентификатор и управление доступом (репликация)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Проверка подлинности представляет собой процесс проверки сущностью (в данном контексте, как правило, компьютером) другой сущности, также называемой *участником*(как правило, это другой компьютер или пользователь). Авторизация — это процесс, с помощью которого авторизованный участник получает доступ к ресурсам, например к файлу в файловой системе или таблице в базе данных.  
  
 Система безопасности репликации использует проверку подлинности и авторизацию для контроля доступа к реплицируемым объектам базы данных, к компьютерам и агентам, участвующим в процессе репликации. Эти действия выполняются с помощью трех механизмов.  
  
-   Безопасность агентов  
  
     Модель безопасности агентов репликации обеспечивает точный контроль учетных записей, под которыми запускаются и подключаются агенты репликации. Подробные сведения о модели безопасности агентов см. в разделе [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md). 
  
-   Роли администрирования  
  
     Убедитесь, что для настройки, обслуживания и обработки репликации используются правильные роли серверов и баз данных. Дополнительные сведения см. в статье [Security Role Requirements for Replication](../../../relational-databases/replication/security/security-role-requirements-for-replication.md).  
  
-   Список доступа к публикации (PAL)  
  
     Предоставьте доступ к публикациям с помощью списка доступа к публикации. Он работает так же, как и список управления доступом [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows. Когда подписчик подключается к издателю или распространителю и запрашивает доступ к публикации, данные для проверки подлинности, переданные агентом, проверяются согласно списку доступа к публикации. Дополнительные сведения и рекомендации по использованию списков доступа к публикации см. в [этой статье](../../../relational-databases/replication/security/secure-the-publisher.md).  
  
## <a name="filtering-published-data"></a>Фильтрование опубликованных данных  
 В дополнение к использованию проверки подлинности и авторизации для контроля доступа к реплицируемым данным и объектам репликация имеет два параметра для управления доступными данными на подписчике: фильтрация по столбцам и строкам. Дополнительные сведения о фильтрации см. в статье [Фильтрация опубликованных данных](../../../relational-databases/replication/publish/filter-published-data.md).  
  
 При определении статьи можно опубликовать только те столбцы, которые необходимы для публикации, и пропустить несущественные столбцы или столбцы, которые содержат конфиденциальные данные. Например, при публикации таблицы **Заказчик** из базы данных Adventure Works для продавцов на выезде можно пропустить столбец **AnnualSales** , который может быть важен лишь для администрации компании.  
  
 Фильтрация опубликованных данных позволяет ограничить доступ к данным и указать данные, доступные на подписчике. Например, можно отфильтровать таблицу **Заказчик** так, чтобы партнеры компании получали сведения только о тех заказчиках, для которых в столбце **ShareInfo** имеется значение «да». При репликации слиянием может возникать проблема безопасности, если используется параметризованный фильтр, содержащий HOST_NAME(). Дополнительные сведения см. в подразделе «Фильтрация с использованием HOST_NAME()» раздела [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  

## <a name="manage-logins-and-passwords-in-replication"></a>Управление именами входа и паролями в репликации
При настройке репликации укажите имена входа и пароли для агентов репликации. После настройки репликации можно изменить имена входа и пароли. Дополнительные сведения см. в статье [View and Modify Replication Security Settings](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md). Если вы измените пароль для учетной записи, которая используется агентом репликации, выполните процедуру [sp_changereplicationserverpasswords &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changereplicationserverpasswords-transact-sql.md).  

Поддержка использования групповых управляемых учетных записей служб (gMSA) впервые появилась в SQL Server 2014. Дополнительные сведения см. в записи блога [Управляемые учетные записи служб для групп и репликации](https://repltalk.com/2019/03/26/replication-and-group-managed-service-accounts/)
  
## <a name="see-also"></a>См. также  
 [Предотвращение угроз и устранение уязвимостей (репликация)](../../../relational-databases/replication/security/threat-and-vulnerability-mitigation-replication.md) [Модель безопасности агента репликации](../../../relational-databases/replication/security/replication-agent-security-model.md)   
 [Рекомендации по защите репликации](../../../relational-databases/replication/security/replication-security-best-practices.md)   

  
  

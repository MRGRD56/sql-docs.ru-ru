---
title: SQL Server Service Broker | Документы Майкрософт
description: Ознакомьтесь с Service Broker. Узнайте, каким образом этот компонент предоставляет встроенную поддержку для обмена сообщениями в ядре СУБД SQL Server и Управляемом экземпляре SQL Azure.
ms.custom: ''
ms.date: 03/17/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.SSBMSGTYPEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBCONTRACTPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBQUEUEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBREMSVCBINDPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBROUTEPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBPRIORITYPROPERTIES.GENERAL.F1
- SQL13.SWB.SSBSERVICEPROPERTIES.GENERAL.F1
helpviewer_keywords:
- Broker See Service Broker
- SQL Server Service Broker
- Service Broker
ms.assetid: 8b8b3b57-fd46-44de-9a4e-e3a8e3999c1e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 287e3c0abfc083607b96598da5e83cd5ab0b58dd
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611174"
---
# <a name="service-broker"></a>Компонент Service Broker
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSB](../../includes/sssb-md.md)] предоставляют встроенную поддержку для обмена сообщениями и очередей в [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и [Управляемом экземпляре SQL Azure](/azure/sql-database/sql-database-managed-instance-index). Это облегчает разработчикам создание сложных, надежных распределенных приложений, использующих компоненты [!INCLUDE[ssDE](../../includes/ssde-md.md)] для связи между разнородными базами данных.  
  
## <a name="when-to-use-service-broker"></a>Когда следует использовать компонент Service Broker

 Используйте компонент Service Broker для реализации собственных функций обработки асинхронных сообщений в базе данных. Разработчики приложений, использующие компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] , могут распределять рабочую нагрузку между несколькими базами данных без программирования сложного взаимодействия и создания внутреннего обмена сообщениями. Service Broker сокращает разработку и проверочную работу, потому что компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] обеспечивает взаимодействие в контексте диалога. Кроме того, это повышает производительность. Например, сервер, обслуживающий клиентские запросы базы данных, поддерживающие веб-сайты, может записывать информацию и отправлять ресурсоемкие задачи в очереди серверных баз данных. [!INCLUDE[ssSB](../../includes/sssb-md.md)] гарантирует, что управление всеми задачами ведется в контексте транзакций, чтобы обеспечить надежность и техническое единообразие.  
  
## <a name="overview"></a>Обзор

  Компонент Service Broker — это инфраструктура доставки сообщений, дающая возможность создания ориентированных на службы приложений в базе данных. В отличие от функциональных возможностей классической обработки, которые требуют постоянно считывать данные из таблиц и обрабатывать их во время жизненного цикла запроса, в ориентированных на службы приложениях имеются службы базы данных, которые позволяют обмениваться сообщениями. Каждая служба имеет очередь, куда помещаются сообщения до их обработки.
  
![Service Broker](media/service-broker.png)
  
  Сообщения в очередях можно извлечь с помощью команды Transact-SQL `RECEIVE` или процедуры активации, которая будет вызываться всякий раз, когда сообщение поступает в очередь.
  
### <a name="creating-services"></a>Создание служб
 
  Службы базы данных создаются с помощью инструкции Transact SQL [CREATE SERVICE](../../t-sql/statements/create-service-transact-sql.md). Службы могут быть связаны с очередью сообщений, созданной с помощью инструкции [CREATE QUEUE](../../t-sql/statements/create-queue-transact-sql.md):
  
```sql
CREATE QUEUE dbo.ExpenseQueue;
GO
CREATE SERVICE ExpensesService
    ON QUEUE dbo.ExpenseQueue; 
```

### <a name="sending-messages"></a>Отправка сообщений
  
  Сообщения отправляются во время диалога между службами с помощью инструкции Transact-SQL [SEND](../../t-sql/statements/send-transact-sql.md). Диалог — канал связи, который устанавливается между службами с помощью инструкции Transact-SQL `BEGIN DIALOG`. 
  
```sql
DECLARE @dialog_handle UNIQUEIDENTIFIER;

BEGIN DIALOG @dialog_handle  
FROM SERVICE ExpensesClient  
TO SERVICE 'ExpensesService';  
  
SEND ON CONVERSATION @dialog_handle (@Message) ;  
```
   Сообщение будет отправлено в `ExpenssesService` и помещено в `dbo.ExpenseQueue`. Так как процедуры активации, связанной с этой очередью, нет, сообщение будет оставаться в очереди, пока кто-то не считает его.

### <a name="processing-messages"></a>Обработка сообщений

   Сообщения, помещенные в очередь, можно выбрать с помощью стандартного запроса `SELECT`. Инструкция `SELECT` не изменяет очередь и не удаляет сообщения. Для чтения и извлечения сообщений из очереди можно использовать инструкции Transact-SQL [RECEIVE](../../t-sql/statements/receive-transact-sql.md).

```sql
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue; 
```

  После обработки всех сообщений из очереди необходимо закрыть диалог с помощью инструкции Transact-SQL [END CONVERSATION](../../t-sql/statements/end-conversation-transact-sql.md).

## <a name="where-is-the-documentation-for-service-broker"></a>Где найти документацию по компоненту Service Broker?  
 Справочная документация по компоненту [!INCLUDE[ssSB](../../includes/sssb-md.md)] входит в документацию по [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] . В эту справочную документацию входят следующие разделы:  
  
-   См. информацию об инструкциях CREATE, ALTER и DROP в разделе [Инструкции на языке описания данных (DDL) (Transact-SQL)](../../t-sql/statements/statements.md)  
  
-   [Инструкции компонента Service Broker](../../t-sql/statements/statements.md)  
  
-   [Представления каталога компонента Service Broker (Transact-SQL)](../../relational-databases/system-catalog-views/service-broker-catalog-views-transact-sql.md)  
  
-   [Динамические административные представления, связанные с компонентом Service Broker (Transact-SQL)](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
-   [Программа ssbdiagnose (компонент Service Broker)](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
 Сведения об основных понятиях компонента [, а также задачах разработки и управления см. в](/previous-versions/sql/sql-server-2008-r2/bb522893(v=sql.105)) ранее опубликованной документации [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Эта документация не повторяется в документации по [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] из-за малого числа изменений в компоненте [!INCLUDE[ssSB](../../includes/sssb-md.md)] в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
## <a name="whats-new-in-service-broker"></a>Новые возможности (компонент Service Broker)  

### <a name="service-broker-and-azure-sql-managed-instance"></a>Service Broker и Управляемый экземпляр SQL Azure

Обмен сообщениями через Service Broker поддерживается только между управляемыми экземплярами SQL Azure:

- `CREATE ROUTE`: нельзя использовать CREATE ROUTE с аргументом ADDRESS, для которого значение отличается от LOCAL или указано DNS-имя другого управляемого экземпляра SQL. Должен быть указан порт 4022. См. статью о [CREATE ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: нельзя использовать ALTER ROUTE с аргументом ADDRESS, для которого значение отличается от LOCAL или указано DNS-имя другого управляемого экземпляра SQL. Должен быть указан порт 4022. См. статью об [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).

Защита транспорта поддерживается, а защита обмена данными — нет:

- Тип `CREATE REMOTE SERVICE BINDING` не поддерживается.

Компонент Service Broker включен по умолчанию и его нельзя отключить. Следующие параметры ALTER DATABASE не поддерживаются:

- `ENABLE_BROKER`
- `DISABLE_BROKER`

В [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] не были внесены значимые изменения.  В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]появились следующие изменения. 

### <a name="messages-can-be-sent-to-multiple-target-services-multicast"></a>Сообщения могут отправляться в несколько целевых служб (многоадресная рассылка)  
 Синтаксис инструкции [SEND (Transact-SQL)](../../t-sql/statements/send-transact-sql.md) расширен для включения многоадресной рассылки благодаря поддержке нескольких дескрипторов диалога.  
  
### <a name="queues-expose-the-message-enqueued-time"></a>Очереди предоставляют время нахождения сообщения в очереди  
 Очереди содержат новый столбец **message_enqueue_time**, в котором показано время нахождения сообщения в очереди.  
  
### <a name="poison-message-handling-can-be-disabled"></a>Можно отключить обработку сообщений о сбое  
 Теперь в инструкциях [CREATE QUEUE (Transact-SQL)](../../t-sql/statements/create-queue-transact-sql.md) и [ALTER QUEUE (Transact-SQL)](../../t-sql/statements/alter-queue-transact-sql.md) можно включать или отключать обработку сообщений о сбое, добавляя предложение `POISON_MESSAGE_HANDLING (STATUS = ON | OFF)`. Представление каталога **sys.service_queues** теперь содержит столбец **is_poison_message_handling_enabled** , показывающий, включено ли сообщение об ошибке.  
  
### <a name="always-on-support-in-service-broker"></a>Поддержка AlwaysOn в компоненте Service Broker  
 Дополнительные сведения см. в статье [Компонент Service Broker с группами доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/service-broker-with-always-on-availability-groups-sql-server.md).  
  
  
## <a name="next-steps"></a>Дальнейшие действия

Чаще всего Service Broker используется для [уведомлений о событиях](../../relational-databases/service-broker/event-notifications.md). Узнайте, как [реализовать уведомления о событиях](../../relational-databases/service-broker/implement-event-notifications.md), [настроить безопасность диалога](../../relational-databases/service-broker/configure-dialog-security-for-event-notifications.md) или [получить дополнительные сведения](../../relational-databases/service-broker/get-information-about-event-notifications.md).

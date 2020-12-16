---
description: Класс событий Broker:Message Undeliverable
title: Класс событий Broker:Message Undeliverable | Документация Майкрософт
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Broker:Message Undeliverable event class
ms.assetid: f532b7c9-ca34-4bac-8dc3-53f9895fd6af
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d3096ecd47192b56743bc3cd9eb925a34aa1784
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476195"
---
# <a name="brokermessage-undeliverable-event-class"></a>Класс событий Broker:Message Undeliverable

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает событие **Broker:Message Undeliverable**, когда компонент Service Broker не может сохранить полученное сообщение, которое нужно доставить службе в этом экземпляре. Сведения о сообщениях, которые должны были быть перенаправлены, см. в разделе [Broker:Forwarded Message Dropped Event Class](../../relational-databases/event-classes/broker-forwarded-message-dropped-event-class.md).  
  
## <a name="brokermessage-undeliverable-event-class-data-columns"></a>Столбцы данных для класса событий Broker:Message Undeliverable  
  
|Столбец данных|Тип|Описание|Номер столбца|Фильтруемый|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**Имя приложения**|**nvarchar**|Имя клиентского приложения, установившего соединение с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот столбец заполняется значениями, передаваемыми приложением, а не отображаемым именем программы.|10|Да|  
|**BigintData1**|**bigint**|Порядковый номер недоставленного сообщения.|52|Нет|  
|**BigintData2**|**bigint**|Порядковый номер последнего успешно подтвержденного сообщения.|53|Нет|  
|**ClientProcessID**|**int**|Идентификатор, присвоенный компьютером сервера процессу, в котором работает клиентское приложение. Этот столбец данных заполняется в том случае, если клиент вводит идентификатор клиентского процесса.|9|Да|  
|**DatabaseID**|**int**|Идентификатор базы данных, указанной в инструкции USE *database* , или базы данных по умолчанию, если для данного экземпляра инструкция USE *database* не выполнялась. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] отображает имя базы данных, если столбец данных **ServerName** захвачен при трассировке и сервер доступен. Определите значение для базы данных, используя функцию DB_ID.|3|Да|  
|**Error**|**int**|Идентификационный номер сообщения в **sys.messages** для текста в событии.|31|Нет|  
|**EventClass**|**int**|Тип захваченного класса событий. Всегда равен **160** для класса событий **Broker:MessageUndeliverable**.|27|нет|  
|**EventSequence**|**int**|Порядковый номер этого события.|51|нет|  
|**EventSubClass**|**nvarchar**|Указывает, было ли недоставленное сообщение упорядоченным. Может принимать одно из следующих двух значений:<br /><br /> **Упорядоченное сообщение**. Недоставленное сообщение было упорядоченным.<br /><br /> **Неупорядоченное сообщение**. Недоставленное сообщение не было упорядоченным.|21|Да|  
|**GUID**|**uniqueidentifier**|Идентификатор диалога, которому принадлежит недоставленное сообщение. Этот идентификатор передается в составе сообщения и является общим для обоих участников диалога.|54|нет|  
|**HostName**|**nvarchar**|Имя компьютера, на котором выполняется клиентская программа. Заполнение этого столбца данных производится в том случае, если клиент предоставляет имя узла. Чтобы определить имя узла, используйте функцию HOST_NAME.|8|Да|  
|**IntegerData**|**int**|Номер фрагмента недоставленного сообщения.|25|Нет|  
|**IntegerData2**|**int**|Номер фрагмента сообщения, подтверждаемого недоставленным сообщением.|55|Нет|  
|**IsSystem**|**int**|Указывает, произошло событие в системном или в пользовательском процессе. 1 = системный, 0 = пользовательский.|60|нет|  
|**LoginName**|**nvarchar**|Имя входа пользователя (имя входа системы безопасности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или учетные данные входа Windows в формате ДОМЕН\имя_пользователя).|11|нет|  
|**LoginSid**|**image**|Идентификатор безопасности вошедшего в систему пользователя. Значение идентификатора безопасности уникально для каждого имени входа на сервере.|41|Да|  
|**NTDomainName**|**nvarchar**|Домен Windows, к которому принадлежит пользователь.|7|Да|  
|**NTUserName**|**nvarchar**|Имя пользователя, которому принадлежит соединение, создавшее это событие.|6|Да|  
|**ObjectName**|**nvarchar**|Дескриптор диалога.|34|нет|  
|**RoleName**|**nvarchar**|Роль дескриптора диалога. Это либо **initiator** , либо **target**.|38|нет|  
|**ServerName**|**nvarchar**|Имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , подвергаемого трассировке.|26|нет|  
|**Уровень серьезности**|**int**|Уровень серьезности для текста в событии.|29|нет|  
|**SPID**|**int**|Идентификатор процесса сервера, который [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] присвоил процессу, связанному с клиентом.|12|Да|  
|**StartTime**|**datetime**|Время начала события, если доступно.|14|Да|  
|**Состояние**|**int**|Указывает место в исходном коде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которое вызвало это событие. Каждое место, которое может вызвать это событие, обозначается отдельным кодом состояния. По этому коду сотрудник отдела технической поддержки корпорации Майкрософт может найти место, вызвавшее данное событие.|30|нет|  
|**TextData**|**ntext**|Причина, по которой компоненту [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удалось доставить сообщение.|1|Да|  
|**TransactionID**|**bigint**|Назначенный системой идентификатор транзакции.|4|нет|  
  
## <a name="see-also"></a>См. также  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  

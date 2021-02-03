---
description: Broker:Forwarded Message Dropped, класс событий
title: Класс событий Broker:Forwarded Message Dropped | Документация Майкрософт
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Broker:Forwarded Message Dropped event class
ms.assetid: ec242d0b-77b0-45f5-8b12-186a14b173a8
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 14a917d6a6faec85cbc712f612e593fdfe19bc06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197861"
---
# <a name="brokerforwarded-message-dropped-event-class"></a>Broker:Forwarded Message Dropped, класс событий

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает событие Broker:Forwarded Message Dropped при удалении компонентом Service Broker сообщения, предназначенного для переадресации.  
  
## <a name="brokerforwarded-message-dropped-event-class-data-columns"></a>Столбцы данных класса событий Broker:Forwarded Message Dropped  
  
|Столбец данных|Тип|Описание|Номер столбца|Фильтруемый|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Имя клиентского приложения, установившего соединение с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот столбец заполняется значениями, передаваемыми приложением, а не отображаемым именем программы.|10|Да|  
|BigintData1|**bigint**|Порядковый номер сообщения.|52|Нет|  
|ClientProcessID|**int**|Идентификатор, присвоенный компьютером сервера процессу, в котором работает клиентское приложение. Этот столбец данных заполняется в том случае, если клиент вводит идентификатор клиентского процесса.|9|Да|  
|DatabaseID|**int**|Идентификатор базы данных, указанной в инструкции USE *database* , или базы данных по умолчанию, если для данного экземпляра инструкция USE *database* не выполнялась. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] отображает имя базы данных, если столбец данных «Имя сервера» захвачен при трассировке и сервер доступен. Определите значение для базы данных, используя функцию DB_ID.|3|Да|  
|имя_базы_данных|**nvarchar**|Имя базы данных, в которой выполняется инструкция пользователя.|35|Да|  
|DBUserName|**nvarchar**|Идентификатор экземпляра брокера, отправившего это сообщение.|40|Нет|  
|Error|**int**|Идентификатор сообщения в sys.messages для текста в событии.|31|Нет|  
|EventClass|**int**|Тип захваченного класса событий. Всегда равен 191 для Broker:Forwarded Message Dropped.|27|Нет|  
|EventSequence|**int**|Порядковый номер этого события.|51|Нет|  
|FileName|**nvarchar**|Имя службы, для которой предназначено сообщение.|36|нет|  
|Идентификатор GUID|**uniqueidentifier**|Идентификатор диалога. Этот идентификатор передается в составе сообщения и является общим для обоих участников диалога.|54|Нет|  
|HostName|**nvarchar**|Имя компьютера, на котором выполняется клиентская программа. Заполнение этого столбца данных производится в том случае, если клиент предоставляет имя узла. Чтобы определить имя узла, используйте функцию HOST_NAME.|8|Да|  
|IndexID|**int**|Число прыжков, оставшихся у перенаправленного сообщения.|24|Нет|  
|IntegerData|**int**|Номер фрагмента перенаправленного сообщения.|25|Нет|  
|LoginSid|**image**|Идентификатор безопасности вошедшего в систему пользователя. Значение идентификатора безопасности уникально для каждого имени входа на сервере.|41|Да|  
|NTDomainName|**nvarchar**|Домен Windows, к которому принадлежит пользователь.|7|Да|  
|NTUserName|**nvarchar**|Имя пользователя, которому принадлежит соединение, создавшее это событие.|6|Да|  
|ObjectId|**int**|Оставшееся время существования перенаправленного сообщения.|22|Нет|  
|ObjectName|**nvarchar**|Идентификатор перенаправленного сообщения.|34|нет|  
|OwnerName|**nvarchar**|Идентификатор экземпляра брокера, которому направлено сообщение.|37|нет|  
|RoleName|**nvarchar**|Роль дескриптора диалога. Одно из двух значений:<br /><br /> -Initiator. Данный брокер начал диалог.<br /><br /> -Target. Данный брокер является назначением диалога.|38|нет|  
|ServerName|**nvarchar**|Имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , подвергаемого трассировке.|26|Нет|  
|Severity|**int**|Уровень серьезности для текста в событии.|29|Нет|  
|SPID|**int**|Идентификатор процесса сервера, который [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] присвоил процессу, связанному с клиентом.|12|Да|  
|StartTime|**datetime**|Время начала события, если доступно.|14|Да|  
|Состояние|**int**|Указывает место в исходном коде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которое вызвало это событие. Каждое место, которое может вызвать это событие, обозначается отдельным кодом состояния. Сотрудник службы технической поддержки Microsoft может использовать этот код состояния для обнаружения участка, выполнение которого привело к событию.|30|нет|  
|Успешное завершение|**int**|Текущее время существования сообщения. Если это значение больше или равно времени жизни сообщения, то сообщение удаляется.|23|Нет|  
|TargetLoginName|**nvarchar**|Сетевой адрес, на который переадресовано данное сообщение.|42|Нет|  
|TargetUserName|**nvarchar**|Имя вызывающей службы для сообщения.|39|нет|  
|TextData|**ntext**|Описание причины удаления сообщения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|1|Да|  
|Transaction ID|**bigint**|Назначенный системой идентификатор транзакции.|4|нет|  
  
 Столбец TextData данного события содержит описание причины удаления сообщения, выполненного [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  
  
  

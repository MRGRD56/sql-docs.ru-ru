---
description: OLEDB DataRead, класс событий
title: Класс событий OLEDB DataRead | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- OLEDB DataRead event class
ms.assetid: fb6869ba-3199-4e32-a650-60a5dda2571e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ad4603f5179755a8d6acac79cec2bdb888188be0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185072"
---
# <a name="oledb-dataread-event-class"></a>OLEDB DataRead, класс событий
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Класс событий OLEDB DataRead происходит, когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вызывает поставщик OLE DB для распределенных запросов и удаленных хранимых процедур. Этот класс событий следует включать в трассировки, которые наблюдают за выполнением запросов данных от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] к поставщику OLE DB.  
  
 Если класс событий OLEDB DataRead включен в трассировку, то объем дополнительной нагрузки повышается. Рекомендуется ограничить использование этого класса событий трассировками, отслеживающими конкретные неполадки в течение короткого периода времени.  
  
## <a name="oledb-dataread-event-class-data-columns"></a>Столбцы данных класса событий OLEDB DataRead  
  
|Имя столбца данных|Тип данных|Описание|Идентификатор столбца|Фильтруемый|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|Имя клиентского приложения, установившего соединение с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот столбец заполняется значениями, передаваемыми приложением, а не отображаемым именем программы.|10|Да|  
|ClientProcessID|**int**|Идентификатор, присвоенный главным компьютером сервера процессу, в котором работает клиентское приложение. Этот столбец данных заполняется в том случае, если клиент предоставляет идентификатор клиентского процесса.|9|Да|  
|DatabaseID|**int**|Идентификатор базы данных, указанной в инструкции USE *database* , или базы данных по умолчанию, если для данного экземпляра инструкция USE *database* не выполнялась. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] отображает имя базы данных, если столбец данных ServerName захвачен при трассировке и сервер доступен. Определите значение для базы данных, используя функцию DB_ID.|3|Да|  
|имя_базы_данных|**nvarchar**|Имя базы данных, в которой выполняется пользовательская инструкция.|35|Да|  
|Duration|**bigint**|Время, необходимое для выполнения события «Вызов OLE DB».|13|нет|  
|EndTime|**datetime**|Время окончания события.|15|Да|  
|Error|**int**|Номер ошибки для данного события. Часто это номер ошибки, который хранится в представлении каталога sys.messages.|31|Да|  
|EventClass|**int**|Тип события = 121.|27|Нет|  
|EventSequence|**int**|Порядковый номер класса событий OLE DB в пакете.|51|Нет|  
|EventSubClass|**int**|Тип подкласса события.<br /><br /> 0 = начало<br /><br /> 1 = завершение|21|Нет|  
|GroupID|**int**|Идентификатор группы рабочей нагрузки, в которой запускается событие трассировки SQL.|66|Да|  
|HostName|**nvarchar**|Имя компьютера, на котором выполняется клиентская программа. Этот столбец данных заполняется, если клиент предоставляет имя узла. Чтобы определить имя узла, используйте функцию HOST_NAME.|8|Да|  
|IsSystem|**int**|Указывает, произошло событие в системном или в пользовательском процессе. 1 = системный, 0 = пользовательский.|60|Да|  
|LinkedServerName|**nvarchar**|Имя связанного сервера.|45|Да|  
|LoginName|**nvarchar**|Имя входа пользователя (либо защищенное имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , либо учетные данные входа [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows в формате домен\имя_пользователя).|11|Да|  
|LoginSid|**image**|Идентификатор безопасности вошедшего в систему пользователя. Эти сведения можно найти в представлении каталога sys.server_principals. Значение идентификатора безопасности уникально для каждого имени входа на сервере.|41|Да|  
|MethodName|**nvarchar**|Имя вызывающего метода.|47|Нет|  
|NTDomainName|**nvarchar**|Домен Windows, к которому принадлежит пользователь.|7|Да|  
|NTUserName|**nvarchar**|Имя пользователя Windows.|6|Да|  
|ProviderName|**nvarchar**|Имя поставщика OLE DB.|46|Да|  
|RequestID|**int**|Идентификатор запроса, содержащего инструкцию.|49|Да|  
|SessionLoginName|**nvarchar**|Имя входа пользователя, создавшего этот сеанс. Например, при соединении с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] под именем Login1 и при выполнении инструкции под именем Login2 SessionLoginName будет содержать значение Login1, а LoginName — значение Login2. В этом столбце отображаются как имена входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , так и имена входа Windows.|64|Да|  
|SPID|**Int**|Идентификатор сеанса, в котором произошло событие.|12|Да|  
|StartTime|**datetime**|Время начала события, если оно известно.|14|Да|  
|TextData|**nvarchar**|Параметры, которые отправляются и принимаются в вызове OLE DB.|1|Нет|  
|TransactionID|**bigint**|Назначенный системой идентификатор транзакции.|4|Да|  
  
## <a name="see-also"></a>См. также:  
 [Расширенные события](../../relational-databases/extended-events/extended-events.md)   
 [Хранимая процедура sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Объекты OLE-автоматизации в Transact-SQL](../../relational-databases/stored-procedures/ole-automation-objects-in-transact-sql.md)  
  
  

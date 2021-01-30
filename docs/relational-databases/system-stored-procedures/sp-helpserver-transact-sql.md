---
description: sp_helpserver (Transact-SQL)
title: sp_helpserver (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpserver
- sp_helpserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpserver
ms.assetid: e8f42de7-c738-41c3-8bf5-dbd559dc7184
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83d212a6cfb817f3aa28c87ab0999d0488cec5e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211922"
---
# <a name="sp_helpserver-transact-sql"></a>sp_helpserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения об определенном удаленном сервере, сервере репликации либо обо всех серверах обоих типов. Выдает имя сервера, сетевое имя сервера, состояние репликации сервера, его идентификационный номер, а также имя параметров сортировки. Кроме того, возвращает значения интервалов ожидания для подключения к связанным серверам или выполнения запросов к ним.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helpserver [ [ @server = ] 'server' ]   
  [ , [ @optname = ] 'option' ]   
  [ , [ @show_topology = ] 'show_topology' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @server = ] 'server'` — Это сервер, о котором сообщается информация. Если параметр *Server* не указан, отчеты обо всех серверах в **master.sys. Servers**. *Server* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @optname = ] 'option'` Параметр, описывающий сервер. *параметр имеет тип* **varchar (** 35 **)** и значение по умолчанию NULL и должен иметь одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**совместимые параметры сортировки**|Влияет на выполнение распределенных запросов на связанных серверах. Если значение этого параметра равно true.|  
|**доступ к данным**|Разрешает и запрещает доступ распределенных запросов к связанному серверу.|  
|**dist**|Распространитель.|  
|**dpub**|Удаленный издатель для этого распространителя.|  
|**lazy schema validation**|Пропускает проверку схемы удаленных таблиц в начале запроса.|  
|**pub**|Издатель.|  
|**удаленного**|Включает RPC с определенного сервера.|  
|**RPC out**|Включает RPC на определенный сервер.|  
|**sub**|Подписчик.|  
|**система**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**использовать удаленные параметры сортировки**|Применение параметров сортировки удаленного столбца вместо параметров сортировки локального сервера.|  
  
`[ @show_topology = ] 'show_topology'` Связь указанного сервера с другими серверами. *show_topology* имеет тип **varchar (** 1 **)** и значение по умолчанию NULL. Если *show_topology* не равно **t** или имеет значение null, **sp_helpserver** возвращает столбцы, перечисленные в разделе результирующие наборы. Если *show_topology* равно **t**, то в дополнение к столбцам, перечисленным в результирующих наборах, **sp_helpserver** также возвращает сведения о **топкс** и **топи** .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя сервера.|  
|**network_name**|**sysname**|Сетевое имя сервера.|  
|**status**|**varchar (** 70 **)**|Состояние сервера.|  
|**id**|**char (** 4 **)**|Идентификационный номер сервера.|  
|**collation_name**|**sysname**|Параметры сортировки сервера.|  
|**connect_timeout**|**int**|Значение времени ожидания для подключения к связанному серверу.|  
|**query_timeout**|**int**|Значение времени ожидания для запросов к связанному серверу.|  
  
## <a name="remarks"></a>Замечания  
 У сервера может быть несколько состояний.  
  
## <a name="permissions"></a>Разрешения  
 Разрешения не проверяются.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-displaying-information-about-all-servers"></a>A. Вывод сведений обо всех серверах  
 В следующем примере сведения обо всех серверах выводятся с помощью команды `sp_helpserver` без аргументов.  
  
```  
USE master;  
GO  
EXEC sp_helpserver;  
```  
  
### <a name="b-displaying-information-about-a-specific-server"></a>Б. Вывод сведений об определенном сервере  
 В следующем примере отображаются все сведения о сервере `SEATTLE2`.  
  
```  
USE master;  
GO  
EXEC sp_helpserver 'SEATTLE2';  
```  
  
## <a name="see-also"></a>См. также:  
 [Ядро СУБД хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_addsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_changesubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_dropsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpsubscriberinfo (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [sp_serveroption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

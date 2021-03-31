---
description: sp_adddistpublisher (Transact-SQL)
title: sp_adddistpublisher (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistpublisher
- sp_adddistpublisher_TSQL
helpviewer_keywords:
- sp_adddistpublisher
ms.assetid: 04e15011-a902-4074-b38c-3ec2fc73b838
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: c25bc50edbbf1c6826f091d2ea1570b423ddaf30
ms.sourcegitcommit: 851f47e27512651f809540b77bfbd09e6ddb5362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937828"
---
# <a name="sp_adddistpublisher-transact-sql"></a>sp_adddistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Настраивает издатель для использования указанной базы данных распространителя. Эта хранимая процедура выполняется на распространителе в любой базе данных. Обратите внимание, что хранимые процедуры [sp_adddistributor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) и [sp_adddistributiondb &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) должны быть выполнены перед использованием этой хранимой процедуры.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_adddistpublisher [ @publisher= ] 'publisher'   
        , [ @distribution_db= ] 'distribution_db'   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @working_directory= ] 'working_directory' ]   
    [ , [ @storage_connection_string= ] 'storage_connection_string']
    [ , [ @trusted= ] 'trusted' ]   
    [ , [ @encrypted_password= ] encrypted_password ]   
    [ , [ @thirdparty_flag = ] thirdparty_flag ]  
    [ , [ @publisher_type = ] 'publisher_type' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Имя сервера можно указать как `<Hostname>,<PortNumber>` . Может потребоваться указать номер порта для подключения, если SQL Server развертывается в Linux или Windows с помощью настраиваемого порта, а служба браузера отключена. Использование настраиваемых номеров портов для удаленного распространителя применимо только к SQL Server 2019.

::: moniker-end
  
`[ @distribution_db = ] 'distribution_db'` Имя базы данных распространителя. Аргумент *distributor_db* имеет тип **sysname** и не имеет значения по умолчанию. Он используется агентами репликации для подключения к издателю.  
  
`[ @security_mode = ] security_mode` Реализованный режим безопасности. Этот параметр используется только агентами репликации для соединения с издателем подписок, обновляемых посредством очередей, или с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателем, отличным от. *security_mode* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**0**|Агенты репликации на распространителе используют для подключения к издателю проверку подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**1** (по умолчанию)|Агенты репликации на распространителе используют для подключения к издателю проверку подлинности Windows.|  
  
`[ @login = ] 'login'` Имя входа. Этот параметр является обязательным, если *security_mode* равен **0**. Аргумент *login* имеет тип **sysname** и значение по умолчанию NULL. Он используется агентами репликации для подключения к издателю.  
  
`[ @password = ] 'password']` Пароль. Аргумент *Password* имеет тип **sysname** и значение по умолчанию NULL. Он используется агентами репликации для подключения к издателю.  
  
> [!IMPORTANT]  
>  Не используйте пустые пароли. Выбирайте надежные пароли.  
  
`[ @working_directory = ] 'working_directory'` Имя рабочего каталога, используемого для хранения файлов данных и схемы для публикации. *working_directory* имеет тип **nvarchar (255)**, а по умолчанию — папку ReplData для этого экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , например `C:\Program Files\Microsoft SQL Server\MSSQL\MSSQ.1\ReplData` . Имя должно быть задано в формате UNC.  

 Для базы данных SQL Azure используйте `\\<storage_account>.file.core.windows.net\<share>` .

`[ @storage_connection_string = ] 'storage_connection_string'` Требуется для базы данных SQL. Используйте ключ доступа на портале Azure в разделе Параметры > хранилища.

 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]

`[ @trusted = ] 'trusted'` Этот параметр является устаревшим и предоставляется только для обратной совместимости. *Trusted* имеет тип **nvarchar (5)** и задает для него любое значение, но **false** приведет к ошибке.  
  
`[ @encrypted_password = ] encrypted_password` Параметр *encrypted_password* больше не поддерживается. Попытка присвоить этому параметру **bit** значение **1** приведет к ошибке.  
  
`[ @thirdparty_flag = ] thirdparty_flag` Имеет значение, когда издатель имеет значение [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *thirdparty_flag* имеет **бит** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**0** (по умолчанию)|База данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**1**|База данных, отличная от базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
`[ @publisher_type = ] 'publisher_type'` Указывает тип издателя, если не является издателем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Аргумент *publisher_type* имеет тип sysname и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**MSSQLSERVER**<br /><br /> (по умолчанию).|Используется издатель [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**СУБД**|Задает стандартного издателя Oracle.|  
|**ORACLE GATEWAY.**|Используется издатель Oracle Gateway.|  
  
 Дополнительные сведения о различиях между издателем Oracle и издателем шлюза Oracle см. в разделе [Настройка издателя Oracle](../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Remarks  
 **sp_adddistpublisher** используется репликацией моментальных снимков, репликацией транзакций и репликацией слиянием.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistpublisher-tran_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_adddistpublisher**.  
  
## <a name="see-also"></a>См. также:  
 [Настройка публикации и распространения](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Настройка распространения](../../relational-databases/replication/configure-distribution.md)  
  
  

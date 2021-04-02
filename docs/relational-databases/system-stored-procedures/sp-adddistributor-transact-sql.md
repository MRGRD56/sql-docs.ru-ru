---
description: sp_adddistributor (Transact-SQL)
title: sp_adddistributor (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistributor
- sp_adddistributor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adddistributor
ms.assetid: 35415502-68d0-40f6-993c-180e50004f1e
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a6a47480afbb16cd37bd260ac20c97d2b532165a
ms.sourcegitcommit: 851f47e27512651f809540b77bfbd09e6ddb5362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937812"
---
# <a name="sp_adddistributor-transact-sql"></a>sp_adddistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Создает запись в таблице [ серверовsys.sys](../../relational-databases/system-compatibility-views/sys-sysservers-transact-sql.md) (если она не существует), помечает запись сервера как распространитель и сохраняет сведения о свойствах. Данная хранимая процедура выполняется на распространителе в базе данных master, чтобы зарегистрировать и пометить сервер как распространитель. В случае с удаленным распространителем хранимая процедура выполняется также и на издателе от базы данных master, чтобы зарегистрировать удаленный распространитель.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_adddistributor [ @distributor= ] 'distributor'   
    [ , [ @heartbeat_interval= ] heartbeat_interval ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @from_scripting= ] from_scripting ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @distributor = ] 'distributor'` Имя сервера распространения. Аргумент *распространитель* имеет тип **sysname** и не имеет значения по умолчанию. Этот аргумент используется только при настройке удаленного распространителя. Он добавляет записи для свойств распространителя в **базе данных msdb. Таблица Мсдистрибутор** .  

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Имя сервера можно указать как `<Hostname>,<PortNumber>` . Может потребоваться указать номер порта для подключения, если SQL Server развертывается в Linux или Windows с помощью настраиваемого порта, а служба браузера отключена. Использование настраиваемых номеров портов для удаленного распространителя применимо только к SQL Server 2019.

::: moniker-end

`[ @heartbeat_interval = ] heartbeat_interval` Максимальное количество минут, в течение которых агент может пройти без записи сообщения о ходе выполнения. *heartbeat_interval* имеет **тип int** и значение по умолчанию 10 минут. Для проверки состояния запущенных агентов репликации создается задание агента SQL Server, выполняемое с заданным интервалом.  
  
`[ @password = ] 'password']` Пароль **distributor_admin** имени входа. Аргумент *Password* имеет тип **sysname** и значение по умолчанию NULL. Если аргумент имеет значение NULL или для него используется пустая строка, то для пароля выбирается случайное значение. Пароль должен быть настроен при добавлении первого удаленного распространителя. **distributor_admin** имя входа и *пароль* хранятся для записи связанного сервера, используемой для подключения RPC *распространителя* , включая локальные соединения. Если *распространитель* является локальным, в качестве пароля для **distributor_admin** задается новое значение. Для издателей с удаленным распространителем необходимо указать то же значение *пароля* при выполнении **Sp_adddistributor** как на издателе, так и на распространителе. для изменения пароля распространителя можно использовать [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) .  
  
> [!IMPORTANT]  
>  По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Комментарии  
 **sp_adddistributor** используется в репликации моментальных снимков, репликации транзакций и репликации слиянием.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistributor-transa_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_adddistributor**.  
  
## <a name="see-also"></a>См. также:  
 [Настройка публикации и распространения](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributor_property &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_dropdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)   
 [sp_helpdistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Настройка распространения](../../relational-databases/replication/configure-distribution.md)  
  
  

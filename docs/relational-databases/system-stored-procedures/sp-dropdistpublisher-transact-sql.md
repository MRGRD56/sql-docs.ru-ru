---
description: sp_dropdistpublisher (Transact-SQL)
title: sp_dropdistpublisher (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistpublisher
- sp_dropdistpublisher_TSQL
helpviewer_keywords:
- sp_dropdistpublisher
ms.assetid: c0bdd3de-3be0-455c-898a-98d4660e7ce3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bb6e7351bedf62dc358222adf8d27468d7e49c1e
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298847"
---
# <a name="sp_dropdistpublisher-transact-sql"></a>sp_dropdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Удаляет распространяющего издателя. Эта хранимая процедура выполняется на распространителе в любой базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_dropdistpublisher [ @publisher = ] 'publisher'  
    [ , [ @no_checks = ] no_checks ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Удаляемый издатель. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  
 
> [!NOTE]
> Если для SQL Server издателя настроен настраиваемый порт, то при удалении такого издателя на распространителе укажите имя сервера издателя вместо `<Hostname>,<PortNumber>` . 
  
`[ @no_checks = ] no_checks` Указывает, будет ли **sp_dropdistpublisher** проверять, что издатель удалил сервер в качестве распространителя. *no_checks* имеет **бит** и значение по умолчанию **0**.  
  
 Если значение **равно 0**, то при репликации проверяется, что удаленный издатель удалил локальный сервер в качестве распространителя. Если издатель является локальным, репликация проверит отсутствие на локальном сервере объектов публикации или распространителя.  
  
 Если значение равно **1**, все объекты репликации, связанные с издателем распространения, удаляются, даже если удаленный издатель недоступен. После этого удаленный издатель должен удалить репликацию с помощью [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) с **\@ ignore_distributor**  =  **1**.  
  
`[ @ignore_distributor = ] ignore_distributor` Указывает, остались ли на распространителе объекты распространителя при удалении издателя. *ignore_distributor* имеет **бит** и может принимать одно из следующих значений:  
  
 **1** = объекты распространения, принадлежащие *издателю* , остаются на распространителе.  
  
 **0** = объекты распространения для *издателя* очищаются на распространителе.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Remarks  
 **sp_dropdistpublisher** используется во всех типах репликации.  
  
 При удалении издателя Oracle, если не удается удалить издатель **sp_dropdistpublisher** возвращает ошибку, а объекты распространителя для издателя удаляются.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistpublisher-tra_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_dropdistpublisher**.  
  
## <a name="see-also"></a>См. также:  
 [Disable Publishing and Distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)  (Отключение публикации и распространения)  
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_helpdistpublisher (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

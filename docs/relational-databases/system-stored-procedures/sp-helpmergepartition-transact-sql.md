---
description: sp_helpmergepartition (Transact-SQL)
title: sp_helpmergepartition (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepartition
- sp_helpmergepartition_TSQL
helpviewer_keywords:
- sp_helpmergepartition
ms.assetid: 184188cc-f519-445d-97ce-aae38f1eb550
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 06994bb4f0e606f0d67e797603e6f4a23899076b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185732"
---
# <a name="sp_helpmergepartition-transact-sql"></a>sp_helpmergepartition (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о секции для указанной публикации слиянием. Эта хранимая процедура выполняется на подписчике в любой базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helpmergepartition [ @publication= ] 'publication'   
    [ , [ @suser_sname = ] 'suser_sname' ]  
    [ , [ @host_name = ] 'host_name' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @suser_sname = ] 'suser_sname'` Значение SUSER_SNAME, используемое для определения секции. Аргумент *SUSER_SNAME* имеет тип **sysname** и значение по умолчанию NULL. Введите этот аргумент для ограничения результирующего набора. При этом будут отображаться только те секции, в которых SUSER_SNAME разрешается в указанное значение.  
  
> [!NOTE]  
>  При указании *suser_sname* *HOST_NAME* должен иметь значение null.  
  
`[ @host_name = ] 'host_name'` Значение HOST_NAME, используемое для определения секции. Аргумент *HOST_NAME* имеет тип **sysname** и значение по умолчанию NULL. Введите этот аргумент для ограничения результирующего набора. При этом будут отображаться только те секции, в которых HOST_NAME разрешается в указанное значение.  
  
> [!NOTE]  
>  При указании *suser_sname* *HOST_NAME* должен иметь значение null.  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**диска**|**int**|Идентифицирует секцию подписчика.|  
|**host_name**|**sysname**|Значение, используемое при создании секции для подписки, которая фильтруется по значению функции [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) на подписчике.|  
|**suser_sname**|**sysname**|Значение, используемое при создании секции для подписки, которая фильтруется по значению функции [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) на подписчике.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Расположение моментального снимка отфильтрованных данных для секции подписчика.|  
|**date_refreshed**|**datetime**|Дата последнего запуска задания моментального снимка, создающего моментальный снимок отфильтрованных данных для секции.|  
|**dynamic_snapshot_jobid**|**uniqueidentifier**|Идентифицирует задание, которое создает моментальный снимок отфильтрованных данных для секции.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_helpmergepartition** используется в репликации слиянием.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** и **db_owner** предопределенной роли базы данных могут выполнять **sp_helpmergepartition**.  
  
## <a name="see-also"></a>См. также:  
 [sp_addmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_dropmergepartition &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md)  
  
  

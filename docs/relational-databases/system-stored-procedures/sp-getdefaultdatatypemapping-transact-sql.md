---
description: sp_getdefaultdatatypemapping (Transact-SQL)
title: sp_getdefaultdatatypemapping (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getdefaultdatatypemapping_TSQL
- sp_getdefaultdatatypemapping
helpviewer_keywords:
- sp_getdefaultdatatypemapping
ms.assetid: b8401de1-f135-41d0-ba79-ce8fe1f48c00
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 64345f8471caf5d757e0c8fbdbc843b2a18680ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183164"
---
# <a name="sp_getdefaultdatatypemapping-transact-sql"></a>sp_getdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о сопоставлении по умолчанию для указанного типа данных между [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и системой управления, отличной от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базы данных (СУБД). Эта хранимая процедура выполняется на распространителе в любой базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_getdefaultdatatypemapping [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
        , [ @source_type = ] 'source_type'    
    [ , [ @source_length = ] source_length ]  
    [ , [ @source_precision = ] source_precision ]  
    [ , [ @source_scale = ] source_scale ]  
    [ , [ @source_nullable = ] source_nullable ]  
        , [ @destination_dbms = ] 'destination_dbms'   
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' OUTPUT ]  
    [ , [ @destination_length = ] destination_length OUTPUT ]  
    [ , [ @destination_precision = ] destination_precision OUTPUT ]  
    [ , [ @destination_scale = ] destination_scale OUTPUT ]  
    [ , [ @destination_nullable = ] source_nullable OUTPUT ]  
    [ , [ @dataloss = ] dataloss OUTPUT ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @source_dbms = ] 'source_dbms'` Имя СУБД, с которой сопоставляются типы данных. Аргумент *source_dbms* имеет тип **sysname** и может принимать одно из следующих значений:  
  
|Значение|Описание|  
|-----------|-----------------|  
|**MSSQLSERVER**|Источником является база данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**СУБД**|Источником является база данных Oracle.|  
  
 Необходимо указать значение для этого параметра.  
  
`[ @source_version = ] 'source_version'` Номер версии исходной СУБД. *source_version* имеет тип **varchar (10)** и значение по умолчанию NULL.  
  
`[ @source_type = ] 'source_type'` Тип данных в исходной СУБД. Аргумент *source_type* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @source_length = ] source_length` Длина типа данных в исходной СУБД. *source_length* имеет тип **bigint** и значение по умолчанию NULL.  
  
`[ @source_precision = ] source_precision` Точность типа данных в исходной СУБД. *source_precision* имеет тип **bigint** и значение по умолчанию NULL.  
  
`[ @source_scale = ] source_scale` Масштаб типа данных в исходной СУБД. *source_scale* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @source_nullable = ] source_nullable` Если тип данных в исходной СУБД поддерживает значение NULL. *source_nullable* имеет **бит** и значение по умолчанию **1**, что означает, что значения NULL поддерживаются.  
  
`[ @destination_dbms = ] 'destination_dbms'` Имя целевой СУБД. Аргумент *destination_dbms* имеет тип **sysname** и может принимать одно из следующих значений:  
  
|Значение|Описание|  
|-----------|-----------------|  
|**MSSQLSERVER**|Целевая база данных — [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**СУБД**|Целевой является база данных Oracle.|  
|**DB2**|Целевой является база данных IBM DB2.|  
|**SYBASE**|Целевой является база данных Sybase.|  
  
 Необходимо указать значение для этого параметра.  
  
`[ @destination_version = ] 'destination_version'` Версия продукта целевой СУБД. *destination_version* имеет тип **varchar (10)** и значение по умолчанию NULL.  
  
`[ @destination_type = ] 'destination_type' OUTPUT` Тип данных, указанный в целевой СУБД. Аргумент *destination_type* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @destination_length = ] destination_length OUTPUT` Длина типа данных в целевой СУБД. *destination_length* имеет тип **bigint** и значение по умолчанию NULL.  
  
`[ @destination_precision = ] destination_precision OUTPUT` Точность типа данных в целевой СУБД. *destination_precision* имеет тип **bigint** и значение по умолчанию NULL.  
  
`[ @destination_scale = ] _destination_scaleOUTPUT` Масштаб типа данных в целевой СУБД. *destination_scale* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @destination_nullable = ] _destination_nullableOUTPUT` Если тип данных в целевой СУБД поддерживает значение NULL. *destination_nullable* имеет **бит** и значение по умолчанию NULL. **1** означает, что значения NULL поддерживаются.  
  
`[ @dataloss = ] _datalossOUTPUT` Имеет значение, если сопоставление может привести к потере данных. *потери* данных имеют **бит** и значение по умолчанию NULL. **1** означает, что возможна потеря данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_getdefaultdatatypemapping** используется во всех типах репликации между [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и не [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] СУБД.  
  
 **sp_getdefaultdatatypemapping** возвращает целевой тип данных по умолчанию, который является ближайшим соответствием с указанным исходным типом данных.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_getdefaultdatatypemapping**.  
  
## <a name="see-also"></a>См. также:  
 [sp_helpdatatypemap &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)  
  
  

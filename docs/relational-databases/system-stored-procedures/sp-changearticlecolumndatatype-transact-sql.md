---
description: sp_changearticlecolumndatatype (Transact-SQL)
title: sp_changearticlecolumndatatype (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changearticlecolumndatatype
- sp_changearticlecolumndatatype_TSQL
helpviewer_keywords:
- sp_changearticlecolumndatatype
ms.assetid: 0db80e08-fb77-4d0c-aa41-455b13ffa9b4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 34f3f1878fcae59601a6ac94ff8536f3d22d6ea5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203707"
---
# <a name="sp_changearticlecolumndatatype-transact-sql"></a>sp_changearticlecolumndatatype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Меняет сопоставление типа данных столбца статьи для публикации Oracle. Эта хранимая процедура выполняется на распространителе в любой базе данных.  
  
> [!NOTE]  
>  Сопоставления типов данных между поддерживаемыми типами издателей обеспечиваются по умолчанию. Используйте **sp_changearticlecolumndatatype** только при переопределении этих параметров по умолчанию.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_changearticlecolumndatatype [ @publication= ] 'publication'  
    [ @article = ] 'article'   
    [ @column = ] 'column'  
    [ , [ @type = ] 'type' ]  
    [ , [ @length = ] length ]  
    [ , [ @precision = ] precision ]  
    [ , [ @scale = ] scale ]  
    [ , [ @publisher = ] 'publisher'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя публикации Oracle. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @article = ] 'article'` Имя статьи. Аргумент *article* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @column = ] 'column'` Имя столбца, для которого необходимо изменить сопоставление типов данных. *столбец* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @type = ] 'type'` Имя [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типа данных в целевом столбце. Аргумент *Type имеет тип* **sysname** и значение по умолчанию NULL.  
  
`[ @length = ] length` Длина [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типа данных в целевом столбце. *length* имеет тип **bigint** и значение по умолчанию NULL.  
  
`[ @precision = ] precision` Точность [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типа данных в целевом столбце. *точность* имеет тип **bigint** и значение по умолчанию NULL.  
  
`[ @publisher = ] 'publisher'` Указывает издателя, отличного от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Аргумент *Publisher* имеет тип **sysname** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **Sp_changearticlecolumndatatype** используется для переопределения сопоставлений типов данных по умолчанию между поддерживаемыми типами издателей (Oracle и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ). Чтобы просмотреть сопоставления типов данных по умолчанию, выполните [sp_getdefaultdatatypemapping](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md).  
  
 **sp_changearticlecolumndatatype** поддерживается только для издателей Oracle. Выполнение этой хранимой процедуры для публикации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] завершится ошибкой.  
  
 **sp_changearticlecolumndatatype** должны выполняться для каждого сопоставления столбцов статьи, которое необходимо изменить.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_changearticlecolumndatatype**.  
  
## <a name="see-also"></a>См. также:  
 [Изменение свойств публикации и статьи](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

---
description: sys.http_endpoints (Transact-SQL)
title: sys.http_endpoints (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.http_endpoints
- http_endpoints
- sys.http_endpoints_TSQL
- http_endpoints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.http_endpoints catalog view
ms.assetid: 16f59695-ecd9-457e-8874-055af63f8ea7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 60b9aa9ad9ef11b88e80a13bfb7469050034cfca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191425"
---
# <a name="syshttp_endpoints-transact-sql"></a>sys.http_endpoints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  

  Содержит строку для всех конечных точек, созданных на сервере, который работает по протоколу HTTP.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**< наследуемые столбцы>**||Наследует столбцы из представления [sys. endpoints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md).|  
|**места**|**nvarchar(128)**|Имя сервера для веб-сайта, заданное в параметре SITE.|  
|**url_path**|**nvarchar(4000)**|Часть URL-адреса, содержащая только путь к данной конечной точке HTTP, как указано в параметре PATH. |  
|**is_clear_port_enabled**|**bit**|1 = очистка порта разрешена с помощью параметра PORT = CLEAR.|  
|**clear_port**|**int**|Номер порта, указанный в параметре CLEAR PORT.<br /><br /> NULL = номер не указан.|  
|**is_ssl_port_enabled**|**bit**|1 = порт SSL разрешен с помощью параметра PORT = SSL.|  
|**ssl_port**|**int**|Номер порта, указанный в параметре SSL PORT.<br /><br /> NULL = номер не указан.|  
|**is_anonymous_enabled**|**bit**|1 = анонимный доступ разрешен с помощью параметра AUTHENTICATION = ANONYMOUS.|  
|**is_basic_auth_enabled**|**bit**|1 = обычная проверка подлинности включена с помощью параметра AUTHENTICATION = BASIC.|  
|**is_digest_auth_enabled**|**bit**|1 = дайджест-проверка подлинности включена с помощью параметра AUTHENTICATION = DIGEST.|  
|**is_kerberos_auth_enabled**|**bit**|1 = встроенная проверка подлинности включена с помощью параметра AUTHENTICATION = KERBEROS.|  
|**is_ntlm_auth_enabled**|**bit**|1 = встроенная проверка подлинности включена с помощью параметра AUTHENTICATION = NTLM.|  
|**is_integrated_auth_enabled**|**bit**|1 = встроенная проверка подлинности включена с помощью параметра AUTHENTICATION = INTEGRATED.|  
|**authorization_realm**|**nvarchar(128)**|Указание, которое возвращается клиенту как часть оклика при дайджест-проверке подлинности. Значение параметра AUTH REALM.<br /><br /> NULL, если значение не указано или если дайджест-проверка подлинности не включена.|  
|**default_logon_domain**|**nvarchar(128)**|Домен имени входа по умолчанию, если включена обычная проверка подлинности. Значение параметр DEFAULT LOGON DOMAIN.<br /><br /> NULL, если значение не указано или если обычная проверка подлинности не включена.|  
|**is_compression_enabled**|**bit**|1 = COMPRESSION = ENABLED параметр задан.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога конечных точек (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)  
  
  

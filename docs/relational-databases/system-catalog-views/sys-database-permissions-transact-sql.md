---
description: sys.database_permissions (Transact-SQL)
title: sys.database_permissions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_permissions
- sys.database_permissions_TSQL
- database_permissions_TSQL
- sys.database_permissions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_permissions catalog view
ms.assetid: c1e261f8-6cb0-4759-b5f1-5ec233602655
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4896a671fd9dd0cf28829b57a1ec7a4f85ad2a1f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750064"
---
# <a name="sysdatabase_permissions-transact-sql"></a>sys.database_permissions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по одной строке для каждого разрешения или разрешения-исключения уровня столбца в базе данных. Для столбцов в представлении каталога содержится по одной строке на каждое разрешение, которое отличается от соответствующего разрешения уровня объекта. Если разрешение столбца совпадает с разрешением соответствующего объекта, то для него не существует строки и к примененному разрешению применяется объект.  
  
> [!IMPORTANT]  
>  Разрешения уровня столбца переопределяют разрешения уровня объекта на ту же сущность.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Указывает класс, на который существует разрешение.<br /><br /> 0 = база данных;<br />1 = объект или столбец<br />3 = схема<br />4 = Участник базы данных<br />5 = сборка — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />6 = Тип<br />10 = коллекция схем XML — <br />                      **Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br />15 = тип сообщения — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />16 = контракт службы — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />17 = служба — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />18 = привязка удаленной службы — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />19 = маршрут — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />23 = полнотекстовый каталог — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />24 = симметричный ключ — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />25 = сертификат **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.<br />26 = асимметричный ключ — **применяется к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздним версиям.|  
|**class_desc**|**nvarchar(60)**|Описание класса, на который существует разрешение.<br /><br /> DATABASE<br /><br /> OBJECT_OR_COLUMN<br /><br /> SCHEMA<br /><br /> DATABASE_PRINCIPAL<br /><br /> ASSEMBLY<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> MESSAGE_TYPE<br /><br /> SERVICE_CONTRACT<br /><br /> SERVICE<br /><br /> REMOTE_SERVICE_BINDING<br /><br /> ROUTE<br /><br /> FULLTEXT_CATALOG<br /><br /> SYMMETRIC_KEYS<br /><br /> CERTIFICATE<br /><br /> ASYMMETRIC_KEY|  
|**major_id**|**int**|Идентификатор предмета, на который существует разрешение, интерпретируется в соответствии с классом. Обычно **major_id** — это просто тип идентификатора, который применяется к представлению класса. <br /><br /> 0 = сама база данных; <br /><br /> >0 = Object-IDs для объектов пользователя <br /><br /> \<0 = Object-IDs для системных объектов |  
|**minor_id**|**int**|Вторичный идентификатор предмета, на который существует разрешение, интерпретируется согласно классу. Часто **minor_id** равен нулю, так как для класса объекта нет доступной подкатегории. В противном случае это идентификатор столбца таблицы.|  
|**grantee_principal_id**|**int**|Идентификатор участника базы данных, которому предоставлено разрешение.|  
|**grantor_principal_id**|**int**|Идентификатор участника базы данных, который предоставил данное разрешение.|  
|**type**|**char (4)**|Тип разрешения в базе данных. Список типов разрешений см. в следующей таблице.|  
|**permission_name**|**nvarchar(128)**|Имя разрешения.|  
|**state**|**char(1)**|Состояние разрешения:<br /><br /> D = запретить<br /><br /> R = отменить<br /><br /> G = предоставить<br /><br /> W = параметр Grant With Grant|  
|**state_desc**|**nvarchar(60)**|Описание состояния разрешения:<br /><br /> DENY<br /><br /> REVOKE<br /><br /> GRANT<br /><br /> GRANT_WITH_GRANT_OPTION|  

## <a name="database-permissions"></a>Разрешения базы данных   
Возможны следующие типы разрешений.
  
|Тип разрешения|Имя разрешения|Применяется к защищаемому объекту|  
|---------------------|---------------------|--------------------------|  
|AADS |ALTER ANY DATABASE EVENT SESSION |DATABASE |  
|AAMK |ALTER ANY MASK |DATABASE |  
|AEDS |ALTER ANY EXTERNAL DATA SOURCE |DATABASE |  
|AEFF |ALTER ANY EXTERNAL FILE FORMAT |DATABASE |  
|AL|ALTER|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, USER, XML SCHEMA COLLECTION|  
|ALAK|ALTER ANY ASYMMETRIC KEY|DATABASE|  
|ALAR|ALTER ANY APPLICATION ROLE|DATABASE|  
|ALAS|ALTER ANY ASSEMBLY|DATABASE|  
|ALCF|ALTER ANY CERTIFICATE|DATABASE|  
|ALDS|ALTER ANY DATASPACE|DATABASE|  
|ALED|ALTER ANY DATABASE EVENT NOTIFICATION|DATABASE|  
|ALFT|ALTER ANY FULLTEXT CATALOG|DATABASE|  
|ALMT|ALTER ANY MESSAGE TYPE|DATABASE|  
|ALRL|ALTER ANY ROLE|DATABASE|  
|ALRT|ALTER ANY ROUTE|DATABASE|  
|ALSB|ALTER ANY REMOTE SERVICE BINDING|DATABASE|  
|ALSC|ALTER ANY CONTRACT|DATABASE|  
|ALSK|ALTER ANY SYMMETRIC KEY|DATABASE|  
|ALSM|ALTER ANY SCHEMA|DATABASE|  
|ALSV|ALTER ANY SERVICE|DATABASE|  
|ALTG|ALTER ANY DATABASE DDL TRIGGER|DATABASE|  
|ALUS|ALTER ANY USER|DATABASE|  
|AUTH|AUTHENTICATE|DATABASE|  
|BADB|BACKUP DATABASE|DATABASE|  
|BALO|BACKUP LOG|DATABASE|  
|CL|CONTROL|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, USER, XML SCHEMA COLLECTION|  
|CO|CONNECT|DATABASE|  
|CORP|CONNECT REPLICATION|DATABASE|  
|CP|CHECKPOINT|DATABASE|  
|CRAG|CREATE AGGREGATE|DATABASE|  
|CRAK|CREATE ASYMMETRIC KEY|DATABASE|  
|CRAS|CREATE ASSEMBLY|DATABASE|  
|CRCF|CREATE CERTIFICATE|DATABASE|  
|CRDB|CREATE DATABASE|DATABASE|  
|CRDF|CREATE DEFAULT|DATABASE|  
|CRED|CREATE DATABASE DDL EVENT NOTIFICATION|DATABASE|  
|CRFN|CREATE FUNCTION|DATABASE|  
|CRFT|CREATE FULLTEXT CATALOG|DATABASE|  
|CRMT|CREATE MESSAGE TYPE|DATABASE|  
|CRPR|CREATE PROCEDURE|DATABASE|  
|CRQU|CREATE QUEUE|DATABASE|  
|CRRL|CREATE ROLE|DATABASE|  
|CRRT|CREATE ROUTE|DATABASE|  
|CRRU|CREATE RULE|DATABASE|  
|CRSB|CREATE REMOTE SERVICE BINDING|DATABASE|  
|CRSC|CREATE CONTRACT|DATABASE|  
|CRSK|CREATE SYMMETRIC KEY|DATABASE|  
|CRSM|CREATE SCHEMA|DATABASE|  
|CRSN|CREATE SYNONYM|DATABASE|  
|CRSO|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> CREATE SEQUENCE|DATABASE|  
|CRSV|CREATE SERVICE|DATABASE|  
|CRTB|CREATE TABLE|DATABASE|  
|CRTY|CREATE TYPE|DATABASE|  
|CRVW|CREATE VIEW|DATABASE|  
|CRXS|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.<br /><br /> CREATE XML SCHEMA COLLECTION|DATABASE|  
|DABO |ADMINISTER DATABASE BULK OPERATIONS | DATABASE |
|DL|DELETE|DATABASE, OBJECT, SCHEMA|  
|EAES |EXECUTE ANY EXTERNAL SCRIPT |DATABASE |
|EX|EXECUTE|ASSEMBLY, DATABASE, OBJECT, SCHEMA, TYPE, XML SCHEMA COLLECTION|  
|IM|IMPERSONATE|Пользователь|  
|IN|INSERT|DATABASE, OBJECT, SCHEMA|  
|RC|RECEIVE|OBJECT|  
|RF;|REFERENCES|ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, SCHEMA, SYMMETRIC KEY, TYPE, XML SCHEMA COLLECTION|  
|SL|SELECT|DATABASE, OBJECT, SCHEMA|  
|SN|SEND|SERVICE|  
|SPLN|SHOWPLAN|DATABASE|  
|SUQN|SUBSCRIBE QUERY NOTIFICATIONS|DATABASE|  
|TO|TAKE OWNERSHIP|ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, XML SCHEMA COLLECTION|  
|UP|UPDATE|DATABASE, OBJECT, SCHEMA|  
|VW|VIEW DEFINITION|APPLICATION ROLE, ASSEMBLY, ASYMMETRIC KEY, CERTIFICATE, CONTRACT, DATABASE, FULLTEXT CATALOG, MESSAGE TYPE, OBJECT, REMOTE SERVICE BINDING, ROLE, ROUTE, SCHEMA, SERVICE, SYMMETRIC KEY, TYPE, USER, XML SCHEMA COLLECTION|  
|VWCK |VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|DATABASE |  
|ввкм |VIEW ANY COLUMN MASTER KEY DEFINITION|DATABASE |  
|VWCT|VIEW CHANGE TRACKING|TABLE, SCHEMA|  
|VWDS|VIEW DATABASE STATE|DATABASE|  
  
## <a name="permissions"></a>Разрешения  
 Любой пользователь может видеть свои собственные разрешения. Для просмотра разрешений для другого пользователя необходимо иметь разрешение VIEW DEFINITION или ALTER ANY USER либо любое разрешение на доступ к данным пользователя. Для просмотра определяемых пользователем ролей необходимо иметь разрешение ALTER ANY ROLE или быть членом роли (например, public).  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-listing-all-the-permissions-of-database-principals"></a>Ответ. Перечисление всех разрешений участников базы данных  
 Следующий запрос перечисляет разрешения, явно предоставленные или отклоненные для участников базы данных.  
  
> [!IMPORTANT]  
>  Разрешения предопределенных ролей базы данных не отображаются в sys.database_permissions. Поэтому участники базы данных могут иметь дополнительные разрешения, не перечисленные здесь.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc, pe.permission_name  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
### <a name="b-listing-permissions-on-schema-objects-within-a-database"></a>Б. Вывод списка разрешений на объекты схемы в базе данных  
 Следующий запрос объединяет sys.database_principals и sys.database_permissions с sys.objects и sys.schemas, чтобы перечислить разрешения, предоставленные или отклоненные для определенных объектов схемы.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pr.authentication_type_desc, pe.state_desc,   
    pe.permission_name, s.name + '.' + o.name AS ObjectName  
FROM sys.database_principals AS pr  
JOIN sys.database_permissions AS pe  
    ON pe.grantee_principal_id = pr.principal_id  
JOIN sys.objects AS o  
    ON pe.major_id = o.object_id  
JOIN sys.schemas AS s  
    ON o.schema_id = s.schema_id;  
```  
    
  
## <a name="see-also"></a>См. также:  
 [Securables](../../relational-databases/security/securables.md)   
 [Иерархия разрешений (ядро СУБД)](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  



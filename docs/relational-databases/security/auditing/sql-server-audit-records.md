---
title: Записи подсистемы аудита SQL Server | Документация Майкрософт
description: Аудиты SQL Server содержат элементы действия аудита, которые записываются в целевой объект аудита. Проверьте эту сводку на наличие записей, которые могут быть отправлены в целевой объект.
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- audit records [SQL Server]
ms.assetid: 7a291015-df15-44fe-8d53-c6d90a157118
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1683231db68ea20fda3082a8ade8f945fcae4c29
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868556"
---
# <a name="sql-server-audit-records"></a>Записи подсистемы аудита SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Подсистема аудита [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] позволяет выполнять аудит событий и групп событий на уровне сервера и уровне базы данных. Дополнительные сведения см. в статье [Подсистема аудита SQL Server (ядро СУБД)](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md). [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Аудит содержит ноль или более элементов действия аудита, записываемых в *цель*аудита. Цель аудита может быть двоичным файлом, журналом событий приложений Windows или журналом событий безопасности Windows. Записи, переданные в цель, могут содержать элементы, описанные в приведенной ниже таблице.  
  
|Имя столбца|Описание|Тип|Доступна всегда|  
|-----------------|-----------------|----------|----------------------|  
|**event_time**|Дата-время срабатывания действия, доступного для аудита.|**datetime2**|Да|  
|**sequence_no**|Отслеживает последовательность записей в одной записи аудита, слишком большой, чтобы уместиться в буфере записи для аудитов.|**int**|Да|  
|**action_id**|Идентификатор действия.<br /><br /> Совет. Для использования значения **action_id** в качестве предиката его необходимо преобразовать из строки символов в числовое значение. Дополнительные сведения см. в статье [Фильтрация подсистемы аудита SQL Server по предикату action_id / class_type](/archive/blogs/sqlsecurity/filter-sql-server-audit-on-action_id-class_type-predicate).|**varchar(4)**|Да|  
|**succeeded**|Указывает, была ли проверка разрешений для действия, инициировавшего событие аудита, пройдена успешно. |**bit**<br /> –1 = успешное завершение. <br />0 = неуспешное завершение.|Да|  
|**permission_bitmask**|Когда применимо, отображаются предоставленные, запрещенные или отмененные разрешения.|**bigint**|Нет|  
|**is_column_permission**|Флаг, обозначающий разрешение уровня столбца.|**bit** <br />–1 = True. <br />0 = False.|Нет|  
|**session_id**|Идентификатор сеанса, в котором произошло событие.|**int**|Да|  
|**server_principal_id**|Идентификатор контекста имени входа, в котором выполнено действие.|**int**|Да|  
|**database_principal_id**|Идентификатор контекста пользователя базы данных, в котором выполнено действие.|**int**|Нет|  
|**object_id**|Основной идентификатор сущности, над которой произведен аудит. Этот идентификатор может принадлежать следующим объектам:<br /><br /> объекты серверов;<br /><br /> базы данных<br /><br /> объекты базы данных<br /><br /> объекты схемы;|**int**|Нет|  
|**target_server_principal_id**|Участник на уровне сервера, к которому применимо действие, доступное для аудита.|**int**|Да|  
|**target_database_principal_id**|Участник базы данных, к которому применимо действие, доступное для аудита.|**int**|Нет|  
|**class_type**|Тип доступной для аудита сущности, для которой проводится аудит.|**varchar(2)**|Да|  
|**session_server_principal_name**|Участник сервера для данного сеанса.|**sysname**|Да|  
|**server_principal_name**|Текущее имя входа.|**sysname**|Да|  
|**server_principal_sid**|Идентификатор безопасности текущего имени входа.|**varbinary**|Да|  
|**database_principal_name**|Текущий пользователь.|**sysname**|Нет|  
|**target_server_principal_name**|Целевое имя входа действия.|**sysname**|Нет|  
|**target_server_principal_sid**|Идентификатор безопасности целевого имени входа.|**varbinary**|Нет|  
|**target_database_principal_name**|Целевой пользователь действия.|**sysname**|Нет|  
|**server_instance_name**|Имя экземпляра сервера, где проводился аудит. Используется стандартный формат «компьютер\экземпляр».|**nvarchar(120)**|Да|  
|**database_name**|Контекст базы данных, в котором выполнялось действие.|**sysname**|Нет|  
|**schema_name**|Контекст схемы, в котором выполнялось действие.|**sysname**|Нет|  
|**object_name**|Имя сущности, для которой проводился аудит. Это имя может принадлежать следующим объектам:<br /><br /> объекты серверов;<br /><br /> базы данных<br /><br /> объекты базы данных<br /><br /> объекты схемы;<br /><br /> инструкция TSQL (если имеется)|**sysname**|Нет|  
|**инструкция**|инструкция TSQL (если имеется)|**nvarchar(4000)**|Нет|  
|**additional_information**|Любые дополнительные сведения о событии хранятся в виде XML.|**nvarchar(4000)**|Нет|  
  
## <a name="remarks"></a>Remarks  
 Некоторые действия имеют незаполненное значение в столбце, так как оно может быть неприменимо к действию.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] хранит 4 000 символов данных для символьных полей в записи аудита. Если значения **additional_information** и **statement** , возвращенные из действия, доступного для аудита, возвращают более 4000 символов, то столбец **sequence_no** используется для записи нескольких записей в отчет аудита для одного действия аудита, чтобы зарегистрировать эти данные. Применяется следующая обработка.  
  
-   Столбец **statement** делится на фрагменты по 4 000 символов.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] записывает в первую строку запись аудита с частичными данными. Все другие поля повторяются в каждой строке.  
  
-   Значение **sequence_no** увеличивается.  
  
-   Этот процесс повторяется до тех пор, пока не будут записаны все данные.  
  
 Можно подключиться к данным, последовательно считывая строки с помощью значения **sequence_no** и столбцов **event_Time**, **action_id** и **session_id** для идентификации действия.  
  
## <a name="related-content"></a>См. также  
 [CREATE SERVER AUDIT (Transact-SQL)](../../../t-sql/statements/create-server-audit-transact-sql.md)  
  
 [ALTER SERVER AUDIT (Transact-SQL)](../../../t-sql/statements/alter-server-audit-transact-sql.md)  
  
 [DROP SERVER AUDIT (Transact-SQL)](../../../t-sql/statements/drop-server-audit-transact-sql.md)  
  
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/create-server-audit-specification-transact-sql.md)  
  
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/alter-server-audit-specification-transact-sql.md)  
  
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/drop-server-audit-specification-transact-sql.md)  
  
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/create-database-audit-specification-transact-sql.md)  
  
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/alter-database-audit-specification-transact-sql.md)  
  
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../../t-sql/statements/drop-database-audit-specification-transact-sql.md)  
  
 [ALTER AUTHORIZATION (Transact-SQL)](../../../t-sql/statements/alter-authorization-transact-sql.md)  
  
 [sys.fn_get_audit_file (Transact-SQL)](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)  
  
 [sys.server_audits (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)  
  
 [sys.server_file_audits (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)  
  
 [sys.server_audit_specifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)  
  
 [sys.server_audit_specification_details (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)  
  
 [sys.database_audit_specifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)  
  
 [sys.database_audit_specification_details (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)  
  
 [sys.dm_server_audit_status (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)  
  
 [sys.dm_audit_actions (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)  
  
 [sys.dm_audit_class_type_map (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)  
  

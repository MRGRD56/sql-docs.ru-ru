---
description: sys.database_scoped_configurations (Transact-SQL)
title: sys.database_scoped_configurations (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- database_scoped_configurations
- database_scoped_configurations_TSQL
- sys.database_scoped_configurations
- sys.database_scoped_configurations_TSQL
helpviewer_keywords:
- sys.database_scoped_configurations catalog view
dev_langs:
- TSQL
ms.assetid: 8899310a-3464-4d38-9f2f-88396c4e7dc2
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current||= azure-sqldw-latest
ms.openlocfilehash: ad4e9036670f90f126ad9c1f693de43ff9660517
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364656"
---
# <a name="sysdatabase_scoped_configurations-transact-sql"></a>sys.database_scoped_configurations (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-addw-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Содержит по одной строке для каждой конфигурации. 

|Имя столбца|Тип данных|Описание|
|-----------------|---------------|-----------------|
|**configuration_id**|**int**|Идентификатор параметра конфигурации.|
|**name**|**nvarchar(60)**|Имя параметра конфигурации. Дополнительные сведения о возможных конфигурациях см. в разделе [ALTER DATABASE scoped CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).|
|**value**|**SQLVARIANT**|Значение, заданное для параметра конфигурации первичной реплики.|
|**value_for_secondary**|**SQLVARIANT**|Значение, заданное для параметра конфигурации вторичных реплик.|
|**is_value_default**|**bit** |Указывает, является ли набор значений значением по умолчанию.|

## <a name="permissions"></a><a name="Permissions"></a> Permissions

Необходимо быть членом роли **public**.

## <a name="remarks"></a>Комментарии

Если в качестве значения для **value_for_secondary** возвращается значение null, это означает, что для базы данных-получателя задан первичный.
 
Параметры конфигурации уровня базы данных будут перенесены вместе с базой данных. Это означает, что при восстановлении или прикреплении заданной базы данных существующие параметры конфигурации будут сохранены.

## <a name="see-also"></a>См. также:

[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)

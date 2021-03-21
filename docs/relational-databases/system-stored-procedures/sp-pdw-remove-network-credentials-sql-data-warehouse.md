---
description: sp_pdw_remove_network_credentials (Azure синапсе Analytics)
title: sp_pdw_remove_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.service: sql-data-warehouse
ms.subservice: design
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: c12696a2-5939-402b-9866-8a837ca4c0a3
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: 1007555e8c4b16ded12a4d6ebb17b85c8f794711
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104739024"
---
# <a name="sp_pdw_remove_network_credentials-azure-synapse-analytics"></a>sp_pdw_remove_network_credentials (Azure синапсе Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  При этом удаляются сетевые учетные данные, хранящиеся в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] , для доступа к общей сетевой папке. Например, эта хранимая процедура используется для удаления разрешения на [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] выполнение операций резервного копирования и восстановления на сервере, который находится в вашей сети.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_remove_network_credentials 'target_server_name'  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Аргументы  
 "*target_server_name*"  
 Указывает имя узла или IP-адрес целевого сервера. Учетные данные для доступа к этому серверу будут удалены из [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Это не приводит к изменению или удалению каких-либо разрешений на фактическом целевом сервере, который управляется вашей группой.  
  
 *target_server_name* определяется как nvarchar (337).  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение **ALTER Server State** .  
  
## <a name="error-handling"></a>Обработка ошибок  
 Ошибка возникает, если не удается удалить учетные данные на управляющем узле и на всех вычислений.  
  
## <a name="general-remarks"></a>Общие замечания  
 Эта хранимая процедура удаляет сетевые учетные данные из учетной записи NetworkService для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Учетная запись NetworkService запускает каждый экземпляр SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на управляющем узле и на вычислительных узлах. Например, при выполнении операции резервного копирования управляющий узел и каждый узел вычислений будут использовать учетные данные учетной записи NetworkService для доступа к целевому серверу.  
  
## <a name="metadata"></a>Метаданные  
 Чтобы получить список всех учетных данных и убедиться, что учетные данные были удалены, используйте [sys.dm_pdw_network_credentials &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-network-credentials-transact-sql.md).  
  
 Чтобы добавить учетные данные, используйте [sp_pdw_add_network_credentials &#40;Azure синапсе Analytics&#41;](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md).  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-remove-credentials-for-performing-a-database-backup"></a>A. Удаление учетных данных для выполнения резервного копирования базы данных  
 В следующем примере удаляются учетные данные имени пользователя и пароля для доступа к целевому серверу с IP-адресом 10.192.147.63.  
  
```sql  
EXEC sp_pdw_remove_network_credentials '10.192.147.63';  
```  
  
  


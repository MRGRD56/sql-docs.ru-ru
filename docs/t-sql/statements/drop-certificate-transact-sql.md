---
description: DROP CERTIFICATE (Transact-SQL)
title: DROP CERTIFICATE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/18/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP CERTIFICATE
- DROP_CERTIFICATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], removing
- removing certificates
- dropping certificates
- DROP CERTIFICATE statement
- deleting certificates
ms.assetid: 5704aa04-68a3-4b29-b62b-8868af487817
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 509bcb0c334b7293d52a6e4ba1445000ffdfdaaf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353950"
---
# <a name="drop-certificate-transact-sql"></a>DROP CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-asdb-pdw](../../includes/applies-to-version/sql-asdb-pdw.md)]

  Удаляет сертификат из базы данных.  
  
> [!IMPORTANT]  
>  Резервная копия сертификата, используемого для шифрования базы данных, должна быть сохранена, даже если она больше не включена в базе данных. Даже если база данных больше не зашифрована, части журнала транзакций могут по-прежнему оставаться защищенными, а для некоторых операций будет требоваться сертификат до выполнения полного резервного копирования базы данных. Сертификат также потребуется для восстановления из резервных копий, созданных в то время, когда база данных была зашифрована.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 

  
 
## <a name="syntax"></a>Синтаксис  
  
```synaxsql  
DROP CERTIFICATE certificate_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *certificate_name*  
 Уникальное имя, под которым сертификат известен в базе данных.  
  
## <a name="remarks"></a>Remarks  
 Сертификаты можно удалять только при условии, что с ними не связано никаких сущностей.  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения CONTROL для сертификата.  
  
## <a name="examples"></a>Примеры  
 В следующем примере сертификат `Shipping04` удаляется из базы данных `AdventureWorks`.  
  
```sql  
USE AdventureWorks2012;  
DROP CERTIFICATE Shipping04;  
```  
  
## <a name="examples-sspdw"></a>Примеры: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В следующем примере удаляется сертификат `Shipping04`.  
  
```sql
USE master;  
DROP CERTIFICATE Shipping04;  
```  
  
## <a name="see-also"></a>См. также:  
 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [ALTER CERTIFICATE (Transact-SQL)](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)

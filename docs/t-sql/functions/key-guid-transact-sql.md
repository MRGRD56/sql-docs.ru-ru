---
description: KEY_GUID (Transact-SQL)
title: KEY_GUID (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Key_GUID_TSQL
- Key_GUID
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], GUIDs
- KEY_GUID function
- GUIDs [SQL Server]
ms.assetid: 9246c7b2-7098-42c4-a222-cbf30267c46a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a41bc9595d6969b161a160691570125a8204b6d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183970"
---
# <a name="key_guid-transact-sql"></a>KEY_GUID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает идентификатор GUID симметричного ключа в базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
Key_GUID( 'Key_Name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 **'** *Key_Name* **'**  
 Имя симметричного ключа в базе данных.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **uniqueidentifier**  
  
## <a name="remarks"></a>Комментарии  
 Если значение идентификатора было указано при создании ключа, его идентификатор GUID — это MD5-хэш этого значения идентификатора. Если значение идентификатора не было указано, то идентификатор GUID был сформирован сервером.  
  
 Если ключ является временным, имя ключа должно начинаться с символа #.  
  
## <a name="permissions"></a>Разрешения  
 Так как временные ключи доступны только во время сеанса, в котором они были созданы, никаких разрешений для доступа к ним не требуется. Чтобы получить доступ к ключу, не являющемуся временным, вызывающей программе необходимо соответствующее разрешение для ключа, и она не должна была получить запрет разрешения VIEW для ключа.  
  
## <a name="examples"></a>Примеры  
 В нижеследующем примере возвращается идентификатор GUID симметричного ключа под именем `ABerglundKey1`.  
  
```sql  
SELECT Key_GUID('ABerglundKey1');  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [sys.key_encryptions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)  
  
  

---
description: SET NOCOUNT (Transact-SQL)
title: SET NOCOUNT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NOCOUNT
- SET_NOCOUNT_TSQL
- NOCOUNT_TSQL
- SET NOCOUNT
dev_langs:
- TSQL
helpviewer_keywords:
- NOCOUNT option
- number of rows affected by statement
- row affected by statements [SQL Server]
- counting rows
- SET NOCOUNT statement
ms.assetid: eb3e6727-cb26-4bc2-84c7-171cbac02029
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 42311516c1ecfd9871c8e54b28409e5b5aef8030
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161136"
---
# <a name="set-nocount-transact-sql"></a>SET NOCOUNT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Запрещает вывод количества строк, на которые влияет инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] или хранимая процедура, в составе результирующего набора.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
SET NOCOUNT { ON | OFF }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
 Если значение инструкции SET NOCOUNT равно ON, то количество строк не возвращается. Если значение инструкции SET NOCOUNT равно OFF, то количество строк возвращается.  
  
 Функция @@ROWCOUNT обновляется, даже если значение SET NOCOUNT равно ON.  
  
 Инструкция SET NOCOUNT ON запрещает всем инструкциям хранимой процедуры отправлять клиенту сообщения DONE_IN_PROC. Для хранимых процедур с несколькими инструкциями, не возвращающих большое количество фактических данных, или для процедур, содержащих циклы [!INCLUDE[tsql](../../includes/tsql-md.md)], установка в инструкции SET NOCOUNT параметра ON может значительно повысить производительность за счет существенного снижения объема сетевого трафика.  
  
 Инструкция SET NOCOUNT устанавливается во время выполнения, а не на этапе синтаксического анализа.  
  
 Чтобы просмотреть текущее значение для этого параметра, выполните следующий запрос.  
  
```sql
DECLARE @NOCOUNT VARCHAR(3) = 'OFF';  
IF ( (512 & @@OPTIONS) = 512 ) SET @NOCOUNT = 'ON';  
SELECT @NOCOUNT AS NOCOUNT;  
  
```  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере запрещается вывод сообщения о количестве измененных строк.  
  
```sql
USE AdventureWorks2012;  
GO  
SET NOCOUNT OFF;  
GO  
-- Display the count message.  
SELECT TOP(5)LastName  
FROM Person.Person  
WHERE LastName LIKE 'A%';  
GO  
-- SET NOCOUNT to ON to no longer display the count message.  
SET NOCOUNT ON;  
GO  
SELECT TOP(5) LastName  
FROM Person.Person  
WHERE LastName LIKE 'A%';  
GO  
-- Reset SET NOCOUNT to OFF  
SET NOCOUNT OFF;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md)   
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

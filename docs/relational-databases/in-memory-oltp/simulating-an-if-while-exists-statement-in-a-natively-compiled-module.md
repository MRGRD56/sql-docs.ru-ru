---
title: Имитация инструкции IF-WHILE EXISTS — модуль, скомпилированный в собственном коде
description: Узнайте, как имитировать предложение EXISTS в условных инструкциях, которые не поддерживаются хранимыми процедурами, скомпилированными в собственном коде, в SQL Server.
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: c0e187c1-cbd9-463c-b417-8a734574f102
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0438806d15caf806c4598f3d2b3882011d77d0f5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485206"
---
# <a name="simulating-an-if-while-exists-statement-in-a-natively-compiled-module"></a>Имитация инструкции IF-WHILE EXISTS в модуле, скомпилированном в собственном коде
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Скомпилированные в собственном коде хранимые процедуры не поддерживают предложение **EXISTS** в условных инструкциях, таких как IF и WHILE.  
  
 В следующем примере показано обходное решение, заключающееся в использовании переменной BIT с инструкцией SELECT для имитации предложения EXISTS:  
  
```sql  
DECLARE @exists BIT = 0  
SELECT TOP 1 @exists = 1 FROM MyTable WHERE ...  
IF @exists = 1  
```  
  
## <a name="see-also"></a>См. также:  
 [Проблемы миграции, связанные с хранимыми процедурами, скомпилированными в собственном коде](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Конструкции языка Transact-SQL, не поддерживаемые в выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  

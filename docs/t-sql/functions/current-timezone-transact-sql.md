---
description: CURRENT_TIMEZONE (Transact-SQL)
title: CURRENT_TIMEZONE (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CURRENT_TIMEZONE
- CURRENT_TIMEZONE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- current time zone [SQL Server]
- current timezone [SQL Server]
- system time zone [SQL Server]
- system timezone [SQL Server]
- functions [SQL Server], time zone
- functions [SQL Server], timezone
- timezone [SQL Server], functions
- time zone [SQL Server], functions
- CURRENT_TIMEZONE function [SQL Server]
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 84b9c6c51831c911fdd90741839a49a621225bae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184040"
---
# <a name="current_timezone-transact-sql"></a>CURRENT_TIMEZONE (Transact-SQL)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

Эта функция возвращает имя часового пояса, отслеживаемого сервером или экземпляром. Для Управляемого экземпляра SQL возвращаемое значение зависит от часового пояса, автоматически назначенного экземпляру во время его создания, а не часового пояса базовой операционной системы.
  
> [!NOTE]  
> Для базы данных SQL часовой пояс всегда устанавливается в формате UTC, а `CURRENT_TIMEZONE` возвращает название часового пояса UTC.
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CURRENT_TIMEZONE ( )  
```
  
## <a name="arguments"></a>Аргументы

Эта функция не имеет аргументов.
  
## <a name="return-type"></a>Тип возвращаемых данных  

**varchar**
  
## <a name="remarks"></a>Remarks  

`CURRENT_TIMEZONE` является недетерминированной функцией. Невозможно проиндексировать представления и выражения, ссылающиеся на этот столбец.
  
## <a name="example"></a>Пример

Обратите внимание, что возвращаемое значение будет отражать фактические часовой пояс и языковые параметры сервера или экземпляра.

```sql
SELECT CURRENT_TIMEZONE();  
/* Returned:  
(UTC+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna 
*/
```  
  
## <a name="see-also"></a>См. также раздел

[Часовой пояс в Управляемом экземпляре SQL](/azure/sql-database/sql-database-managed-instance-timezone)

[CURRENT_TIMEZONE_ID()](./current-timezone-id-transact-sql.md)
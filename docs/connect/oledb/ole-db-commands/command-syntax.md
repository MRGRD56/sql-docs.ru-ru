---
title: Синтаксис команд (драйвер OLE DB) | Документация Майкрософт
description: Узнайте о синтаксисе команд, распознаваемых OLE DB Driver for SQL Server, и выполнении хранимой процедуры SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, commands
- commands [OLE DB]
- OLE DB Driver for SQL Server, stored procedures
- stored procedures [OLE DB], command syntax
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a8210c1131d928da03bb6b580a5f6218cbfd97af
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751924"
---
# <a name="command-syntax"></a>Синтаксис команды
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server распознает синтаксис команд, определенный макросом DBGUID_SQL. Описатель сообщает OLE DB Driver for SQL Server, что сочетание ODBC SQL, ISO и [!INCLUDE[tsql](../../../includes/tsql-md.md)] является допустимым синтаксисом. Например, следующая инструкция SQL использует escape-последовательность ODBC SQL, чтобы указать строковую функцию LCASE.  
  
```  
SELECT customerid={fn LCASE(CustomerID)} FROM Customers  
```  
  
 Функция LCASE возвращает строковое выражение, в котором все символы приведены к нижнему регистру. Строковая функция LOWER стандарта ISO выполняет ту же операцию, поэтому следующая инструкция SQL является эквивалентом ISO для представленной выше инструкции ODBC.  
  
```  
SELECT customerid=LOWER(CustomerID) FROM Customers  
```  
  
 Драйвер OLE DB для SQL Server успешно обрабатывает любую форму инструкции при указании в виде текста для команды.  
  
## <a name="stored-procedures"></a>Хранимые процедуры  
 При выполнении хранимой процедуры [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью команды драйвера OLE DB для SQL Server используйте escape-последовательность ODBC CALL в тексте команды. В этом случае драйвер OLE DB для SQL Server использует механизм удаленного вызова процедуры [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], чтобы оптимизировать обработку команды. Например, следующая инструкция ODBC SQL является более предпочтительным текстом команды, нежели форма [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
-   ODBC SQL  
  
    ```  
    {call SalesByCategory('Produce', '1995')}  
    ```  
  
-   Transact-SQL  
  
    ```  
    EXECUTE SalesByCategory 'Produce', '1995'  
    ```  
  
## <a name="see-also"></a>См. также:  
 [Команды](../../oledb/ole-db-commands/commands.md)  
  
  

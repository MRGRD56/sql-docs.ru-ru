---
description: Уровень изоляции транзакций курсора
title: Уровень изоляции транзакций курсора | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- isolation levels [ODBC]
- ODBC applications, row versioning
- cursors [ODBC], isolation levels
- ODBC cursors, isolation levels
- row versioning [SQL Server], ODBC
ms.assetid: 0c6663a4-5a25-44aa-8fe4-e35af9bf4a83
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 77c7f10faed8bacefe20cd46b5375b85cae3f6f7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756354"
---
# <a name="cursor-transaction-isolation-level"></a>Уровень изоляции транзакций курсора
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Режим полной блокировки курсоров основывается на взаимодействии между атрибутами параллелизма и уровнем изоляции транзакций, установленным клиентом. Клиенты ODBC устанавливают уровень изоляции транзакции с помощью [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) SQL_ATTR_TXN_ISOLATION или атрибутов SQL_COPT_SS_TXN_ISOLATION. Режим блокировки специфической среды курсора определяется комбинацией режимов блокировки параллелизма и параметров уровня изоляции транзакции.  
  
 Драйвер ODBC для собственного клиента поддерживает следующие уровни изоляции транзакций курсора [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   Зафиксированная операция чтения (SQL_TXN_READ_COMMITTED)  
  
-   Незафиксированная операция чтения (SQL_TXN_READ_COMMITTED)  
  
-   Операция чтения с возможностью повторения (SQL_TXN_REPEATABLE_READ)  
  
-   Сериализуемый (SQL_TXN_SERIALIZABLE)  
  
-   Моментальный снимок (SQL_TXN_SS_SNAPSHOT)  
  
 Обратите внимание, что API ODBC указывает дополнительные уровни изоляции транзакций, но они не поддерживаются [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] драйвером ODBC для собственного клиента.  
  
## <a name="see-also"></a>См. также:  
 [Свойства курсора](../../../relational-databases/native-client-odbc-cursors/properties/cursor-properties.md)  
  
  

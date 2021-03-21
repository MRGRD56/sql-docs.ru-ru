---
title: Собственные номера ошибок | Документация Майкрософт
description: Для ошибок драйвер SQL Server Native Client ODBC возвращает номер собственной ошибки из SQL Server или для ошибок, обнаруженных драйвером, 0.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC error handling, native error numbers
- SQL Server Native Client ODBC driver, errors
- native error numbers [SQL Server Native Client]
- messages [ODBC], native error numbers
- errors [ODBC], native error numbers
ms.assetid: 77cbc826-f47f-4803-8e7a-223d6df069b1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b3fc025e009f1301c108ce349e9857eb81be12af
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737624"
---
# <a name="native-error-numbers"></a>Собственные коды ошибок
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Для ошибок, возникающих в источнике данных (возвращенном [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента возвращает номер собственной ошибки, возвращенный ей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Для ошибок, обнаруженных драйвером, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента возвращает номер собственной ошибки, равный 0. Дополнительные сведения о списке машинных номеров ошибок см. в столбце "ошибка" системной таблицы **sysmessages** в базе данных **master** в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Сведения о кодах ошибок состояния см. в разделе [SQLSTATE &#40;коды ошибок ODBC&#41;](../../relational-databases/native-client-odbc-error-messages/sqlstate-odbc-error-codes.md). В случае ошибок, возвращаемых библиотекой Net-Library, собственный код ошибки можно получить из базового сетевого программного обеспечения.  
  
## <a name="see-also"></a>См. также:  
 [Обработка ошибок и сообщений](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  

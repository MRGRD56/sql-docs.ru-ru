---
description: сообщения об ошибках
title: Сообщения об ошибках | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, errors
- messages [ODBC], types
- ODBC error handling, message types
- errors [ODBC], types
ms.assetid: 46c0c22e-d105-4d5b-bb9d-5694472e8651
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e86cf87c114bd08e8e1f32189ca9d0755b1764e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745874"
---
# <a name="error-messages"></a>сообщения об ошибках
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Текст сообщений, возвращаемых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвером ODBC для собственного клиента, помещается в параметр *MessageText* объекта **SQLGetDiagRec**. На источник ошибки указывает заголовок сообщения.  
  
 [Майкрософт][Диспетчер драйверов ODBC]  
 Эти ошибки формируются диспетчером драйверов ODBC.  
  
 [Майкрософт][Библиотека курсоров ODBC]  
 Эти ошибки формируются библиотекой курсоров ODBC.  
  
 [Майкрософт][Собственный клиент SQL Server]  
 Эти ошибки вызываются [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвером ODBC для собственного клиента. Если не существует других узлов с именем сетевой библиотеки или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], то ошибка произошла в драйвере.  
  
 NNTP [SQL Server Native Client] [*Net-транспорта*]  
 Эти ошибки вызываются [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сетевой библиотекой, где *net-транспорта* — отображаемое имя [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сетевого транспорта клиента (например, именованные каналы, Общая память, сокеты TCP/IP или Via). Остальная часть сообщения об ошибке содержит вызванную функцию сетевой библиотеки и функцию, вызванную в базовом сетевом API-интерфейсе функцией потока табличных данных. Код ошибки *pfNative* , возвращенный с этими ошибками, является кодом ошибки из базового стека сетевых протоколов.  
  
 NNTP [SQL Server Native Client] [ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]]  
 Эти ошибки формируются в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Остальная часть сообщения об ошибке — текст сообщения об ошибке из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Код *pfNative* , возвращенный с этими ошибками, является номером ошибки из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения о списке сообщений об ошибках (и их номерах), которые могут быть возвращены [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , см. в столбцах Description и Error системной таблицы **sysmessages** в базе данных **master** в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Обработка ошибок и сообщений](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  

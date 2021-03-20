---
description: Выделение дескриптора соединения
title: Выделение маркера подключения | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC applications, passwords
- ODBC applications, connections
- handles [SQL Server Native Client]
- expiration [SQL Server Native Client]
- passwords [SQL Server], modifying
- SQL Server Native Client ODBC driver, connection handles
- connection handles [SQL Server Native Client]
- modifying passwords
- SQLAllocHandle function
ms.assetid: 471d8a31-199c-4f92-bb10-004fc7733b35
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9f77f6d5598eda5c94e46bfcf6efbfa868eaa197
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746014"
---
# <a name="allocating-a-connection-handle"></a>Выделение дескриптора соединения
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Прежде, чем приложение может подключиться к источнику данных или драйверу, оно должно выделить дескриптор соединения. Это делается путем вызова **функцию SQLAllocHandle** с параметром *параметром handletype* , для которого задано значение SQL_HANDLE_DBC, а *инпусандле* указывает на инициализированный обработчик среды.  
  
 Характеристики соединения управляются заданием атрибутов соединения. Например, поскольку транзакции выполняются на уровне соединения, уровень изоляции транзакции является атрибутом соединения. Аналогично, время ожидания входа в систему, или количество секунд, в течение которых следует ждать при попытках соединения перед истечением времени ожидания, является атрибутом соединения.  
  
 Атрибуты соединения задаются с помощью [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md), а их текущие параметры извлекаются с помощью [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md). Если **SQLSetConnectAttr** вызывается до попытки подключения, диспетчер драйверов ODBC сохраняет атрибуты в структуре соединения и устанавливает их в драйвере как часть процесса подключения. Некоторые атрибуты соединения необходимо установить прежде, чем приложение попытается установить соединение; другие могут быть установлены после завершения соединения. Например, атрибут SQL_ATTR_ODBC_CURSORS должен быть установлен прежде, чем установлено соединение, а SQL_ATTR_AUTOCOMMIT может быть установлен после соединения.  
  
 Производительность приложений, работающих с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версии 7.0 и более поздними, может быть повышена путем изменения размера сетевых пакетов потока табличных данных (TDS). Размер пакета по умолчанию устанавливается на сервере и равен 4 КБ. Лучшая производительность, как правило, достигается при размере пакета от 4 КБ до 8 КБ. Если в результате тестирования выяснилось, что более высокая производительность достигается при других размерах пакета, приложение может изменить размер пакета. Приложения ODBC могут сделать это перед подключением, вызвав **SQLSetConnectAttr** с параметром SQL_ATTR_PACKET_SIZE. Производительность некоторых приложений повышается при пакетах большего размера, но выигрыш в производительности обычно минимален для пакетов размером более 8 КБ.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента имеет ряд расширенных атрибутов подключения, которые приложение может использовать для повышения его функциональности. Некоторые из этих атрибутов управляют теми же параметрами, которые могут быть указаны в источниках данных и использованы для переопределения любого параметра, заданного в источнике данных. Например, если приложение использует заключенные в кавычки идентификаторы, оно может назначить специфическому для драйвера атрибуту SQL_COPT_SS_QUOTED_IDENT значение SQL_QI_ON, чтобы этот параметр был установлен всегда, независимо от настройки источника данных.  
  
## <a name="see-also"></a>См. также:  
 [Взаимодействие с SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  

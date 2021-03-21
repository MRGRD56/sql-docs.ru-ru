---
description: Расширения драйвера SQL Server — функции массового копирования
title: Функции с массовым копированием | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC], functions
- ODBC, bulk copy operations
- functions [ODBC]
ms.assetid: 6526b892-1d58-4f55-8335-f09887f6ea02
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 533446dd9409ad6716bd1a6cf1ca6fcdc37d8ee9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752524"
---
# <a name="sql-server-driver-extensions---bulk-copy-functions"></a>Расширения драйвера SQL Server — функции массового копирования
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Интерфейс ODBC — это прикладной программный интерфейс Microsoft Win32, используемый приложениями для доступа к данным в источниках данных ODBC. Справочник по драйверу ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не содержит описания всех вызовов функций ODBC. Обсуждаются только те функции, параметры которых специфичны для драйвера или для поведения при использовании с драйвером ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] соответствует требованиям спецификации ODBC 3.51. Для получения исчерпывающего справочника по ODBC 3,51 Скачайте пакет SDK Microsoft Data Access Components из [центра разработчиков доступа к данным и хранилища](https://go.microsoft.com/fwlink?linkid=4173)или просмотрите [Справочник программиста по ODBC](../../odbc/reference/odbc-programmer-s-reference.md) в Интернете.  
 
 Зависящее от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] расширение API-интерфейса массового копирования драйвера ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет клиентскому приложению быстро добавлять или извлекать строки данных из таблицы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  При использовании [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client вы можете ссылаться на функции массового копирования (BCP), объявленные в файлах SQLNCLI11.LIB и SQLNCLI.H.  
  
 Приложение, использующее вызовы функции BCP API, необходимо связать с библиотекой (LIB-файлом), которая поставлялась с драйвером (DLL-файлом), используемым приложением. Приложение BCP не должно быть связано более чем с одной библиотекой драйвера.  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md)  
  
-   [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)  
  
-   [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)  
  
-   [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md)  
  
-   [bcp_colptr](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colptr.md)  
  
-   [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md)  
  
-   [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)  
  
-   [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md)  
  
-   [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md)  
  
-   [bcp_getcolfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-getcolfmt.md)  
  
-   [bcp_gettypename](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-gettypename.md)  
  
-   [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md)  
  
-   [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md)  
  
-   [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md)  
  
-   [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)  
  
-   [bcp_setbulkmode](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setbulkmode.md)  
  
-   [bcp_setcolfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setcolfmt.md)  
  
-   [Функция bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md)  
  
## <a name="see-also"></a>См. также:  
 [Расширения драйверов SQL Server]()   
 [Выполнение операций с массовым копированием &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  

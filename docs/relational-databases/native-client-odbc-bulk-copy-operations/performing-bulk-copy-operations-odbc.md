---
title: Выполнение операций с массовым копированием (ODBC) | Документация Майкрософт
description: Узнайте, как драйвер ODBC для SQL Server Native Client поддерживает функции DB-Library, выполняющие операции копирования SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC]
- ODBC, bulk copy operations
- minimally logged operations [SQL Server Native Client]
- bulk copy [ODBC], about bulk copy
ms.assetid: 5c793405-487c-4f52-88b8-0091d529afb3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8b6481e980a257958bc5e0f74720683feeb924c7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754754"
---
# <a name="performing-bulk-copy-operations-odbc"></a>Выполнение операций массового копирования (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Стандарт ODBC напрямую не поддерживает операции массового копирования [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. При соединении с экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версии 7.0 или более поздней, драйвер ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает функции библиотеки DB-Library, выполняющие операции массового копирования в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот собственный модуль драйвера обеспечивает легкий путь обновления для существующих приложений DB-Library, использующих функции массового копирования. Специализированная поддержка массового копирования реализована в следующих файлах.  
  
-   sqlncli.h  
  
     Включает прототипы функций и определения констант для функций массового копирования. Файл sqlncli.h должен входить в состав приложения ODBC, выполняющего операции массового копирования, и при компиляции приложения должен находиться в пути поиска включаемых файлов.  
  
-   sqlncli11.lib  
  
     Должна находиться в пути к библиотекам компоновщика и определена как файл для связывания. Файл sqlncli11.lib поставляется вместе с драйвером ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
-   sqlncli11.dll  
  
     Необходима во время выполнения. Файл sqlncli11.dll поставляется вместе с драйвером ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
> [!NOTE]  
>  Функция ODBC **SQLBulkOperations** не имеет отношения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] функциям с массовым копированием. Для выполнения операций массового копирования приложения должны использовать собственные функции массового копирования [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="minimally-logging-bulk-copies"></a>Массовое копирование с минимальным ведением журнала  
 В модели полного восстановления все операции вставки строк, выполняемые при массовой загрузке, полностью регистрируются в журнале транзакций. При загрузке большого количества данных это может привести к быстрому заполнению журнала транзакций. При определенных условиях возможно минимальное протоколирование. Минимальное ведение журнала снижает вероятность заполнения журнала в результате массовой загрузки и является также более эффективным, чем полное ведение журнала.  
  
 Сведения об использовании минимального ведения журнала см. [в разделе Предварительные требования для минимального ведения журнала при выполнении массового импорта](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
  
## <a name="remarks"></a>Remarks  
 При использовании программы bcp.exe в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] или более поздних версий могут возникать ошибки в ситуациях, в которых в версиях, предшествующих [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], ошибок не возникало. Это происходит по той причине, что в более поздних версиях программа bcp.exe больше не выполняет явное преобразование типов данных. До версии [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] программа bcp.exe преобразовывала числовые данные в тип money, если целевая таблица имела тип данных money. Однако в этой ситуации программа bcp.exe просто усекала лишние поля. Начиная с версии [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], если типы данных в файле и целевой таблице не совпадают, программа bcp.exe выдает ошибку, если присутствуют данные, которые придется усечь, чтобы поместить в целевую таблицу. Для устранения этой ошибки преобразуйте данные в целевой тип данных. При необходимости используйте программу bcp.exe версии, предшествующей [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Использование файлов данных и файлов форматирования](../../relational-databases/native-client-odbc-bulk-copy-operations/using-data-files-and-format-files.md)  
  
-   [Массовое копирование из переменных приложения](../../relational-databases/native-client-odbc-bulk-copy-operations/bulk-copying-from-program-variables.md)  
  
-   [Управление размером пакета массового копирования](../../relational-databases/native-client-odbc-bulk-copy-operations/managing-bulk-copy-batch-sizes.md)  
  
-   [Массовое копирование данных text и image](../../relational-databases/native-client-odbc-bulk-copy-operations/bulk-copying-text-and-image-data.md)  
  
-   [Перевод массового копирования с DB-Library на ODBC](../../relational-databases/native-client-odbc-bulk-copy-operations/converting-from-db-library-to-odbc-bulk-copy.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  

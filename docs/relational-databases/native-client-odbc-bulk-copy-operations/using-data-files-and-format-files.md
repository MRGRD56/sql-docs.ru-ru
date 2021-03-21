---
description: Использование файлов данных и файлов форматирования
title: Использование файлов данных и файлов форматирования | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], file formats
- ODBC, functions
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [SQL Server Native Client]
- ODBC, bulk copy operations
- bulk copy [ODBC], data files
ms.assetid: c01b7155-3f0a-473d-90b7-87a97bc56ca5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9ca6f51e9d81f3b2f40b2d4c049ba100667dfd4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746114"
---
# <a name="using-data-files-and-format-files"></a>Использование файлов данных и файлов форматирования
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Простейшая программа массового копирования выполняет следующие действия.  
  
1.  Вызывает [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) , чтобы указать групповое копирование (задание BCP_OUT) из таблицы или представления в файл данных.  
  
2.  Вызывает [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) для выполнения операции с массовым копированием.  
  
 Файл данных создается в собственном режиме; следовательно, данные из всех столбцов таблицы или представления хранятся в файле данных в том же формате, что и в базе данных. Затем файл можно с помощью операции массового копирования скопировать на сервер, выполнив те же шаги и установив значение DB_IN вместо DB_OUT. Это возможно только в случае, когда структура исходной и целевой таблиц идентична. Полученный файл данных также может быть введен в служебную программу **bcp** с помощью параметра **/n** (собственный режим).  
  
 Для массового копирования результирующего набора из инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], а не непосредственно из таблицы или представления.  
  
1.  Вызовите **bcp_init** , чтобы указать групповое копирование, но укажите значение NULL для имени таблицы.  
  
2.  Вызовите [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) с параметром *eOption* , для которого задано значение bcphints а, а *iValue* — указателем на строку SQLTCHAR, содержащую инструкцию Transact-SQL.  
  
3.  Вызовите **bcp_exec** , чтобы выполнить операцию с массовым копированием.  

 В качестве инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] подходит любая инструкция, которая создает результирующий набор. Создается файл данных, содержащий первый результирующий набор инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Если инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] создает несколько результирующих наборов, операция массового копирования пропускает все результирующие наборы, следующие за первым.  
  
 Чтобы создать файл данных, в котором данные столбца хранятся в другом формате, чем в таблице, вызовите [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) , чтобы указать, сколько столбцов будет изменено, а затем вызовите [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md) для каждого столбца, формат которого необходимо изменить. Это выполняется после вызова **bcp_init** , но перед вызовом **bcp_exec**. **bcp_colfmt** указывает формат, в котором данные столбца хранятся в файле данных. Его можно использовать при выполнении операций копирования или извлечения. Для установки признаков конца строки и столбца можно также использовать **bcp_colfmt** . Например, если в данных нет символов табуляции, можно создать файл с разделителями-символами табуляции с помощью **bcp_colfmt** , чтобы задать символ табуляции в качестве признака конца для каждого столбца.  
  
 При выполнении операций с массовым копированием и использованием **bcp_colfmt** можно легко создать файл форматирования с описанием созданного файла данных, вызвав [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md) после последнего вызова **bcp_colfmt**.  
  
 При выполнении операции с массовым копированием из файла данных, описанного в файле форматирования, прочтите файл форматирования, вызвав [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) после **bcp_init** , но до **bcp_exec**.  
  
 Функция **bcp_control** управляет несколькими параметрами при выполнении операции копирования [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из файла данных. **bcp_control** задает параметры, такие как максимальное количество ошибок перед завершением, строка в файле, с которой начинается копирование, строка, которую нужно присвоить, и размер пакета.  
  
## <a name="see-also"></a>См. также:  
 [Выполнение операций с массовым копированием &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  

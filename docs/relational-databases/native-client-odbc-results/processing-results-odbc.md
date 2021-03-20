---
title: Обработка результатов (ODBC) | Документация Майкрософт
description: Сведения об обработке данных, которые SQL Server возвращают, когда приложение ODBC отправляет инструкцию SQL.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- result sets [ODBC], about result sets
- SQLRowCount function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- COMPUTE clause
- result sets [ODBC]
- COMPUTE BY clause
ms.assetid: 61a8db19-6571-47dd-84e8-fcc97cb60b45
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c3c5b8bad410418a95a9ba180ab3921eb02e0644
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752774"
---
# <a name="processing-results-odbc"></a>Обработка результатов (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  После передачи приложением инструкции SQL, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает все данные результата в виде одного или нескольких результирующих наборов. Результирующий набор — это набор строк и столбцов, соответствующих критерию запроса. Инструкции SELECT, функции работы с каталогами и некоторые хранимые процедуры создают результирующий набор, доступный для приложения в табличной форме. Если выполняемая инструкция SQL является хранимой процедурой, пакетом из нескольких команд либо инструкцией SELECT, содержащей ключевые слова, то необходимо выполнять обработку нескольких результирующих наборов.  
  
 Функции ODBC для работы с каталогами также могут получать данные. Например, [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md) извлекает данные о столбцах в источнике данных. Эти результирующие наборы могут содержать нуль или более строк.  
  
 Некоторые инструкции SQL, например GRANT или REVOKE, не возвращают результирующие наборы. Для этих инструкций код возврата из **SQLExecute** или **SQLExecDirect** обычно указывает на то, что инструкция выполнена успешно.  
  
 Каждая из инструкций INSERT, UPDATE и DELETE возвращает результирующий набор, содержащий только количество строк, затронутых изменением. Это число становится доступным, когда приложение вызывает [SQLRowCount](../../relational-databases/native-client-odbc-api/sqlrowcount.md). ODBC 3. приложения *x* должны либо вызывать **SQLRowCount** для получения результирующего набора, либо [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) , чтобы отменить его. Когда приложение выполняет пакет или хранимую процедуру, содержащую несколько инструкций INSERT, UPDATE или DELETE, результирующий набор каждой инструкции изменения должен обрабатываться с помощью **SQLRowCount** или отменяться с помощью **SQLMoreResults**. Эти счетчики можно сбросить, включив в пакет или хранимую процедуру инструкцию SET NOCOUNT ON.  
  
 Transact-SQL включает инструкцию SET NOCOUNT. Если параметр NOCOUNT установлен в значение ON, SQL Server не возвращает количество строк, затронутых инструкцией, а **SQLRowCount** возвращает 0. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Версия драйвера ODBC для собственного клиента вводит параметр [SQLGetStmtAttr](../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) , зависящий от драйвера, SQL_SOPT_SS_NOCOUNT_STATUS, чтобы сообщить о том, включен ли параметр NOCOUNT. Когда **SQLRowCount** в любое время возвращает 0, приложение должно протестировать SQL_SOPT_SS_NOCOUNT_STATUS. Если возвращается SQL_NC_ON, значение 0 из **SQLRowCount** указывает, что SQL Server не вернул число строк. Если возвращается SQL_NC_OFF, это означает, что параметр NOCOUNT отключен и значение 0 из **SQLRowCount** указывает на то, что инструкция не повлияла ни на какие строки. Приложения не должны отображать значение **SQLRowCount** , если SQL_SOPT_SS_NOCOUNT_STATUS SQL_NC_OFF. Большие пакеты или хранимые процедуры могут содержать несколько инструкций SET NOCOUNT, следовательно, программисты не должны предполагать, что параметр SQL_SOPT_SS_NOCOUNT_STATUS останется неизменным. Этот параметр следует проверять каждый раз, когда **SQLRowCount** возвращает 0.  
  
 Несколько других инструкций Transact-SQL возвращают данные в сообщениях, а не в результирующих наборах. Когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента получает эти сообщения, он возвращает SQL_SUCCESS_WITH_INFO, чтобы сообщить приложению о доступности информационных сообщений. Затем приложение может вызвать **SQLGetDiagRec** для получения этих сообщений. Инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], работающие таким способом, перечислены ниже.  
  
-   DBCC  
  
-   SET SHOWPLAN (доступна в ранних версиях SQL Server)  
  
-   SET STATISTICS  
  
-   PRINT  
  
-   RAISERROR  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента возвращает SQL_ERROR для RAISERROR с уровнем серьезности 11 или выше. При уровне серьезности RAISERROR от 19 и выше соединение отключается.  
  
 Чтобы обработать результирующие наборы инструкции SQL, приложение выполняет следующие действия.  
  
-   Определяет характеристики результирующего набора.  
  
-   Привязывает столбцы к переменным программы.  
  
-   Получает одно значение, целую строку значений, или несколько строк значений.  
  
-   Проверяет наличие результирующих наборов, и в случае их существования, начинает цикл снова для определения характеристик нового результирующего набора.  
  
 Процесс получения строк из источника данных и передачи их в приложение называется выборкой.  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Определение характеристик результирующего набора &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/determining-the-characteristics-of-a-result-set-odbc.md)  
  
-   [Назначение хранилища](../../relational-databases/native-client-odbc-results/assigning-storage.md)  
  
-   [Выборка итоговых данных](../../relational-databases/native-client-odbc-results/fetching-result-data.md)  
  
-   [Сопоставление типов данных &#40;ODBC&#41;](../../relational-databases/native-client-odbc-results/mapping-data-types-odbc.md)  
  
-   [Использование типов данных](../../relational-databases/native-client-odbc-results/data-type-usage.md)  
  
-   [Автоматическое преобразование символьных данных](../../relational-databases/native-client-odbc-results/autotranslation-of-character-data.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Разделы руководства по обработке результатов &#40;ODBC&#41;](../native-client-odbc-how-to/processing-results-process-results.md)  
  

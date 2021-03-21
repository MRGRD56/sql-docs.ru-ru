---
title: Управление размерами пакетов с пакетным копированием | Документация Майкрософт
description: Узнайте, как размер пакета для операции копирования с помощью пакетной службы определяет область транзакции, что влияет на поведение при возникновении ошибок и нагрузку на блокировку в SQL Server Native Client ODBC.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- ODBC, bulk copy operations
- batches [ODBC]
- bulk copy [ODBC], batch sizes
ms.assetid: 4b24139f-788b-45a6-86dc-ae835435d737
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 41b6d411b0a0b82bb440f7bf1d16fe9e2d153eae
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754734"
---
# <a name="managing-bulk-copy-batch-sizes"></a>Управление размером пакета массового копирования
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Основным назначением пакета в операциях массового копирования является определение области транзакции. Если размер пакета не задан, то функции массового копирования рассматривают все массовое копирование как единую транзакцию. Если указан размер пакета, каждый пакет представляет собой транзакцию, которая фиксируется после завершения работы пакета.  
  
 Если при выполнении массового копирования размер пакета не указан и возникает ошибка, то происходит откат всего массового копирования. Восстановление массового копирования с продолжительным временем выполнения может занять значительное время. Если размер пакета задан, массовое копирование рассматривает каждый пакет как транзакцию и фиксирует каждый пакет. Если происходит ошибка, то необходим откат только одного последнего необработанного пакета.  
  
 Размер пакета может также влиять на затраты ресурсов, связанные с управлением блокировками. При выполнении операции с массовым копированием [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Указание TABLOCK можно указать с помощью [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) , чтобы получить блокировку таблицы вместо блокировки строк. Минимальные издержки всей операции массового копирования может дать одна блокировка таблицы. Если подсказка TABLOCK не указана, выполняются блокировки для отдельных строк и издержки обслуживания всех блокировок во время массового копирования могут уменьшить производительность. Поскольку блокировки удерживаются только во время транзакции, указание размера пакета приводит к регулярному формированию фиксации, которая освобождает текущие блокировки.  
  
 Количество строк в пакете может значительно повлиять на производительность при массовом копировании большого числа строк. Рекомендуемый размер пакета зависит от типа выполняемого массового копирования.  
  
-   При массовом копировании в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] укажите подсказку TABLOCK и задайте большой размер пакета.  
  
-   Если подсказка TABLOCK не указана, ограничьте размер пакета числом менее 1000 строк.  
  
 При выполнении операции с массовым копированием из файла данных размер пакета указывается путем вызова **bcp_control** с параметром бкпбатч перед вызовом [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md). При выполнении операций с массовым копированием из переменных программы с помощью [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) и [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)размер пакета контролируется путем вызова [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md) после вызова [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) *x* раз, где *x* — число строк в пакете.  
  
 Помимо указания размера транзакции, пакеты также оказывают влияние при отправке строк по сети серверу. Функции с массовым копированием обычно кэшируют строки из **bcp_sendrow** до тех пор, пока не будет заполнен сетевой пакет, а затем отправит полный пакет на сервер. Однако когда приложение вызывает **bcp_batch**, текущий пакет отправляется на сервер независимо от того, был ли он заполнен. Использование очень маленького размера пакета может снизить производительность, если оно приведет к отправке на сервер большого числа частично заполненных пакетов. Например, вызов **bcp_batch** после каждой **bcp_sendrow** вызывает отправку каждой строки в отдельном пакете и, если строки не слишком велики, занимают место в каждом пакете. По умолчанию размер сетевых пакетов для SQL Server составляет 4 КБ, хотя приложение может изменить размер, вызвав [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) , указав атрибут SQL_ATTR_PACKET_SIZE.  
  
 Другой побочный результат пакетов состоит в том, что каждый пакет считается необработанным результирующим набором до тех пор, пока не завершится с помощью **bcp_batch**. При попытке выполнить какие-либо другие операции с помощью обработчика соединения, когда пакет недоступен, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента выдает ошибку с SQLState = "HY000" и строку сообщения об ошибке:  
  
```  
"[Microsoft][SQL Server Native Client] Connection is busy with  
results for another hstmt."  
```  
  
## <a name="see-also"></a>См. также:  
 [Выполнение операций с массовым копированием &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)   
 [Массовый импорт и экспорт данных (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  

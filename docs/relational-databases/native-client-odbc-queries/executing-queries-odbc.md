---
title: Исполнение запросов (ODBC) | Документация Майкрософт
description: Приложение ODBC может запускать инструкции на экземпляре SQL Server путем инициализации обработчика соединения и подключения к источнику данных.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC applications, executing queries
- SQL Server Native Client ODBC driver, statements
- ODBC applications, statements
- SQL Server Native Client ODBC driver, queries
- queries [ODBC]
ms.assetid: d935bcba-8ce6-4159-8395-6c86431602ad
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b686da10b222ed26c5a234c81f6f2314d9e9a2f1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751294"
---
# <a name="executing-queries-odbc"></a>Выполнение запросов (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  После того, как приложение ODBC инициализирует дескриптор соединения и подключается к источнику данных, оно выделяет один или несколько дескрипторов инструкций на дескриптор соединения. Приложение может затем выполнять [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] инструкции в обработчике инструкций. Общая последовательность событий при выполнении инструкции SQL.  
  
1.  Установите необходимые атрибуты инструкции.  
  
2.  Сформируйте инструкцию.  
  
3.  Выполните инструкцию.  
  
4.  Получите результирующие наборы.  
  
 После того, как приложение получит все строки во всех результирующих наборах, возвращенных инструкцией SQL, оно может выполнить другой запрос на том же дескрипторе инструкции. Если приложение определяет, что не требуется извлекать все строки в определенном результирующем наборе, он может отменить оставшуюся часть результирующего набора, вызвав либо [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) , либо [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md).  
  
 Если в приложении ODBC необходимо несколько раз выполнить одну инструкцию SQL с различными данными, используйте маркер параметра, обозначенный вопросительным знаком (?) при построении инструкции SQL:  
  
```  
INSERT INTO MyTable VALUES (?, ?, ?)  
```  
  
 Затем каждый маркер параметра может быть привязан к программной переменной путем вызова [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md).  
  
 После того, как будут вызваны все инструкции SQL и обработаны их результирующие наборы, приложение освобождает дескриптор инструкции.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента поддерживает несколько дескрипторов инструкций для каждого дескриптора соединения. Управление транзакциями осуществляется на уровне соединения, поэтому вся работа, выполняемая со всеми дескрипторами инструкций на одном дескрипторе соединения, управляется как часть одной транзакции.  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Выделение дескриптора инструкции](../../relational-databases/native-client-odbc-queries/allocating-a-statement-handle.md)  
  
-   [Создание инструкции SQL &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/constructing-an-sql-statement-odbc.md)  
  
-   [Конструирование инструкций SQL для курсоров](../../relational-databases/native-client-odbc-queries/constructing-sql-statements-for-cursors.md)  
  
-   [Использование параметров инструкции](../../relational-databases/native-client-odbc-queries/using-statement-parameters.md)  
  
-   [Исполнение инструкций &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
-   [Освобождение дескриптора инструкции](../../relational-databases/native-client-odbc-queries/freeing-a-statement-handle.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (ODBC)](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  

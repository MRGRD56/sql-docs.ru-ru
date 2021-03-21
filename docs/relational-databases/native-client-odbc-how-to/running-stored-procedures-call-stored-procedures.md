---
title: Вызов хранимых процедур (ODBC) | Документация Майкрософт
description: Узнайте, как добавить источник данных с помощью администратора ODBC, программно или с помощью файла, прежде чем использовать приложения ODBC с SQL Server 2005 или более поздней версии.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], calling
ms.assetid: 31176be8-d40e-4f93-8d44-a46e804a3e2d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 152ecb6304a4049418e82dee42d9dd236fc38f4b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751844"
---
# <a name="running-stored-procedures---call-stored-procedures"></a>Выполнение хранимых процедур — вызов хранимых процедур
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Драйвер ODBC для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает выполнение хранимых процедур как удаленных хранимых процедур. Выполнение хранимых процедур как удаленных хранимых процедур позволяет драйверу и серверу повысить производительность при выполнении процедуры.  
  
  Когда инструкция SQL вызывает хранимую процедуру при помощи escape-последовательности ESCAPE ODBC CALL, драйвер Microsoft® SQL Server™ отправляет процедуру на SQL Server при помощи механизма удаленного вызова хранимой процедуры. Запросы RPC пропускают большую часть синтаксической проверки и обработки параметров инструкции в SQL Server; они быстрее, чем инструкция Transact-SQL EXECUTE.  
  
 Пример приложения, демонстрирующий эту функцию, см. в разделе [обработка кодов возврата и выходных параметров &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md).  
  
### <a name="to-run-a-procedure-as-an-rpc"></a>Выполнение процедуры с помощью RPC  
  
1.  Сконструируйте инструкцию SQL, использующую escape-последовательность ODBC CALL. В этой инструкции для каждого входного, входного-выходного и выходного параметров, а также для возвращаемого процедурой значения (при его наличии) используются маркеры параметров.  
  
    ```  
    {? = CALL procname (?,?)}  
    ```  
  
2.  Вызовите [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) для каждого параметра входа, входа-выхода и выхода, а также для значения, возвращаемого процедурой (если оно имеется).  
  
3.  Выполните инструкцию с помощью [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md).  
  
> [!NOTE]  
>  Если приложение отправляет процедуру при помощи синтаксиса Transact-SQL EXECUTE (в отличие от escape-последовательности ODBC CALL), драйвер SQL Server ODBC передает этот вызов процедуры SQL Server в виде инструкции SQL, а не RPC. Кроме того, при использовании инструкции Transact-SQL EXECUTE выходные параметры не возвращаются.  
  
## <a name="see-also"></a>См. также:  
  [Пакетирование вызовов хранимых процедур](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)   
 [Выполнение хранимых процедур](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Вызов хранимой процедуры](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)   
 [Процедуры](../../relational-databases/native-client-odbc-queries/executing-statements/procedures.md)  
  

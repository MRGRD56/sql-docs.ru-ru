---
description: Использование координатора распределенных транзакции Майкрософт (ODBC)
title: Координатор распределенных транзакций (ODBC)
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- MS DTC, using
ms.assetid: 12a275e1-8c7e-436d-8a4e-b7bee853b35c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a60437fe027f93bbadff502eb5bec63fdc5c16c8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104733929"
---
# <a name="use-microsoft-distributed-transaction-coordinator-odbc"></a>Использование координатора распределенных транзакции Майкрософт (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-update-two-or-more-sql-servers-by-using-ms-dtc"></a>Обновление двух и более экземпляров SQL Server с помощью координатора распределенных транзакций (MS DTC)  
  
1.  Подключитесь к координатору распределенных транзакций (MS DTC), используя функцию DtcGetTransactionManager MS DTC OLE. Дополнительные сведения о координаторе распределенных транзакций (MS DTC) см. в разделе «Координатор распределенных транзакций (Майкрософт)».  
  
2.  Вызывайте SQL Дриверконнект один раз для каждого устанавливаемого соединения SQL Server.  
  
3.  Вызовите функцию MS DTC OLE ITransactionDispenser::BeginTransaction, чтобы начать транзакцию координатора распределенных транзакций (MS DTC) и получить объект Transaction, представляющий транзакцию.  
  
4.  Вызовите функцию [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) один или несколько раз для каждого из соединений ODBC, которые необходимо прикрепить к транзакции MS DTC. Второй параметр [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) должен иметь значение SQL_ATTR_ENLIST_IN_DTC, а третий параметр должен быть объектом Transaction (полученным на шаге 3).  
  
5.  Вызовите [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) по одному разу для каждого экземпляра SQL Server, который нужно обновить.  
  
6.  Вызовите функцию MS DTC OLE ITransaction::Commit, чтобы зафиксировать транзакцию MS DTC. Объект Transaction более не действителен.  

 Чтобы выполнить ряд транзакций MS DTC, повторите шаги с 3 по 6.  
  
 Чтобы освободить ссылку на объект Transaction, вызовите функцию MS DTC OLE ITransaction::Return.  
  
 Чтобы использовать соединение ODBC с транзакцией распределенного координатора транзакций (MS DTC), а затем использовать то же соединение с транзакцией локального SQL Server, вызовите функцию [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) с параметром SQL_DTC_DONE.  
  
> [!NOTE]  
>  Можно также вызывать [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) и [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) по очереди для каждого экземпляра SQL Server, а не вызывать их описанным выше в шагах 4 и 5 способом.  
  
## <a name="see-also"></a>См. также:  
 [Выполнение транзакций &#40;ODBC&#41;](../native-client/odbc/performing-transactions-in-odbc.md)  
  

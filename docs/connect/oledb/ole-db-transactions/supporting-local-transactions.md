---
title: Поддержка локальных транзакций (драйвер OLE DB)
description: Сведения о поддержке локальных транзакций драйвером OLE DB Driver for SQL Server и использовании ITransactionLocal для более точного управления областью локальных транзакций.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, transactions
- autocommit mode
- transactions [OLE DB]
- ITransactionLocal interface
- OLE DB Driver for SQL Server, transactions
- local transactions [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41f797115262d928e9ec47ab13144dba1a98e6bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754214"
---
# <a name="supporting-local-transactions"></a>Поддержка локальных транзакций
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Сеанс ограничивает область действия локальных транзакций Microsoft OLE DB Driver for SQL Server Если OLE DB Driver for SQL Server передает в направлении потребителя запрос к подключенному экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], этот запрос представляет собой единицу работы OLE DB Driver for SQL Server Локальные транзакции всегда содержат одну или несколько единиц работы в одном сеансе OLE DB Driver for SQL Server  
  
 При использовании режима автоматической фиксации, установленного для драйвера OLE DB для SQL Server по умолчанию, одна единица работы рассматривается как область действия локальной транзакции. В локальной транзакции участвует только одна единица работы. При создании сеанса OLE DB Driver for SQL Server начинает транзакцию для этого сеанса. При успешном завершении единицы работы выполненная работа фиксируется. При сбое выполняется откат всей начатой работы; потребителю сообщается об ошибке. В любом случае драйвер OLE DB для SQL Server начинает новую локальную транзакцию для сеанса, чтобы выполнить всю работу в транзакции.  
  
 Потребитель драйвера OLE DB для SQL Server может более детально управлять областью действия локальной транзакции с помощью интерфейса **ITransactionLocal**. Если сеанс потребителя инициирует транзакцию, все рабочие единицы сеанса между точкой начала транзакции и последующими неизбежными вызовами методов **Commit** или **Abort** рассматриваются как единая операция. OOLE DB Driver for SQL Server неявно начинает транзакцию, если этого требует потребитель. Если потребитель не запрашивает хранения, сеанс восстанавливает режим родительской транзакции. Как правило, это режим автозавершения.  
  
 OLE DB Driver for SQL Server поддерживает параметры метода **ITransactionLocal::StartTransaction** следующим образом.  
  
|Параметр|Description|  
|---------------|-----------------|  
|*isoLevel*[in]|Уровень изоляции, который должен использоваться с этой транзакцией. В локальных транзакциях OLE DB Driver for SQL Server поддерживает следующие действия:<br /><br /> **ISOLATIONLEVEL_UNSPECIFIED**<br /><br /> **ISOLATIONLEVEL_CHAOS**<br /><br /> **ISOLATIONLEVEL_READUNCOMMITTED**<br /><br /> **ISOLATIONLEVEL_READCOMMITTED**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_CURSORSTABILITY**<br /><br /> **ISOLATIONLEVEL_REPEATABLEREAD**<br /><br /> **ISOLATIONLEVEL_SERIALIZABLE**<br /><br /> **ISOLATIONLEVEL_ISOLATED**<br /><br /> **ISOLATIONLEVEL_SNAPSHOT**<br /><br /> <br /><br /> Начиная с [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] уровень изоляции ISOLATIONLEVEL_SNAPSHOT допустим для аргумента *isoLevel* независимо от того, включено ли в базе данных управление версиями. Однако произойдет ошибка, если пользователь попытается выполнить инструкцию, когда управление версиями не включено, а база данных предназначена не только для чтения. Кроме того, ошибка XACT_E_ISOLATIONLEVEL возникнет, если для *isoLevel* задано значение ISOLATIONLEVEL_SNAPSHOT при соединении с версией [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ранее [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].|  
|*isoFlags*[in]|OLE DB Driver for SQL Server возвращает ошибку для ненулевых значений.|  
|*pOtherOptions*[in]|Если значение не равно NULL, то OLE DB Driver for SQL Server запрашивает через интерфейс объект параметров. Если элемент объекта параметров *ulTimeout* не равен нулю, то OLE DB Driver for SQL Server возвращает XACT_E_NOTIMEOUT. OLE DB Driver for SQL Server игнорирует значение элемента *szDescription*.|  
|*pulTransactionLevel*[out]|Если значение не равно NULL, OLE DB Driver for SQL Server возвращает уровень вложенности транзакции.|  
  
 Для локальных транзакций OLE DB Driver для SQL Server реализует параметры метода **ITransaction::Abort** следующим образом.  
  
|Параметр|Description|  
|---------------|-----------------|  
|*pboidReason*[in]|При установке не учитывается. Может иметь значение NULL.|  
|*fRetaining*[in]|Если задано значение TRUE, то для сеанса неявным образом запускается новая транзакция. Эта транзакция должна быть зафиксирована или завершена объектом-получателем. Если задано значение FALSE, то OLE DB Driver for SQL Server восстанавливает для сеанса режим автоматической фиксации.|  
|*fAsync*[in]|Асинхронное аварийное завершение не поддерживается OLE DB Driver for SQL Server. OLE DB Driver for SQL Server возвращает XACT_E_NOTSUPPORTED, если значение не равно FALSE.|  
  
 Для локальных транзакций OLE DB Driver for SQL Server реализует параметры метода **ITransaction::Commit** следующим образом.  
  
|Параметр|Description|  
|---------------|-----------------|  
|*fRetaining*[in]|Если задано значение TRUE, то для сеанса неявным образом запускается новая транзакция. Эта транзакция должна быть зафиксирована или завершена объектом-получателем. Если задано значение FALSE, то OLE DB Driver for SQL Server восстанавливает для сеанса режим автоматической фиксации.|  
|*grfTC*[in]|Асинхронные возвраты и возвраты на первом этапе не поддерживаются OLE DB Driver for SQL Server. OLE DB Driver for SQL Server возвращает XACT_E_NOTSUPPORTED, если значение не равно XACTTC_SYNC.|  
|*grfRM*[in]|Должно быть равно 0.|  
  
 Наборы строк в сеансе драйвера OLE DB для SQL Server сохраняются в локальной операции фиксации или отмены на основе значений набора строк свойств DBPROP_ABORTPRESERVE и DBPROP_COMMITPRESERVE. По умолчанию эти свойства имеют значение VARIANT_FALSE, и все наборы строк в сеансе драйвера OLE DB для SQL Server теряются после операции фиксации или отмены.  
  
 OLE DB Driver for SQL Server не реализует интерфейс **ITransactionObject**. При попытке потребителя получить ссылку на интерфейс возвращается E_NOINTERFACE.  
  
 В этом примере используется интерфейс **ITransactionLocal**.  
  
```  
// Interfaces used in the example.  
IDBCreateSession*   pIDBCreateSession   = NULL;  
ITransaction*       pITransaction       = NULL;  
IDBCreateCommand*   pIDBCreateCommand   = NULL;  
IRowset*            pIRowset            = NULL;  
  
HRESULT             hr;  
  
// Get the command creation and local transaction interfaces for the  
// session.  
if (FAILED(hr = pIDBCreateSession->CreateSession(NULL,  
     IID_IDBCreateCommand, (IUnknown**) &pIDBCreateCommand)))  
    {  
    // Process error from session creation. Release any references and  
    // return.  
    }  
  
if (FAILED(hr = pIDBCreateCommand->QueryInterface(IID_ITransactionLocal,  
    (void**) &pITransaction)))  
    {  
    // Process error. Release any references and return.  
    }  
  
// Start the local transaction.  
if (FAILED(hr = ((ITransactionLocal*) pITransaction)->StartTransaction(  
    ISOLATIONLEVEL_REPEATABLEREAD, 0, NULL, NULL)))  
    {  
    // Process error from StartTransaction. Release any references and  
    // return.  
    }  
  
// Get data into a rowset, then update the data. Functions are not  
// illustrated in this example.  
if (FAILED(hr = ExecuteCommand(pIDBCreateCommand, &pIRowset)))  
    {  
    // Release any references and return.  
    }  
  
// If rowset data update fails, then terminate the transaction, else  
// commit. The example doesn't retain the rowset.  
if (FAILED(hr = UpdateDataInRowset(pIRowset, bDelayedUpdate)))  
    {  
    // Get error from update, then terminate.  
    pITransaction->Abort(NULL, FALSE, FALSE);  
    }  
else  
    {  
    if (FAILED(hr = pITransaction->Commit(FALSE, XACTTC_SYNC, 0)))  
        {  
        // Get error from failed commit.  
        }  
    }  
  
if (FAILED(hr))  
    {  
    // Update of data or commit failed. Release any references and  
    // return.  
    }  
  
// Release any references and continue.  
```  
  
## <a name="see-also"></a>См. также:  
 [Транзакции](../../oledb/ole-db-transactions/transactions.md)   
 [Работа с изоляцией моментального снимка](../../oledb/features/working-with-snapshot-isolation.md)  
  
  

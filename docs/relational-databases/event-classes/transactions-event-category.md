---
description: Категория событий Transactions
title: Категория событий TransactionLog | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL Server event classes, Transactions event category
- event classes [SQL Server], Transactions event category
- Transactions event category [SQL Server]
ms.assetid: bfc75c5b-7115-49d8-9148-a0c84ee66a9a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 086ea0a7206c667242d3aa631fa0f46f86220b2d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470648"
---
# <a name="transactions-event-category"></a>Категория событий Transactions
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Классы событий **Transactions** могут использоваться для наблюдения за состоянием транзакций. Имена классов событий с префиксом **TM:** используются для отслеживания операций, связанных с транзакциями и передающихся через интерфейс управления транзакциями.  
  
## <a name="in-this-section"></a>в этом разделе  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Класс событий DTCTransaction](../../relational-databases/event-classes/dtctransaction-event-class.md)|Отслеживает транзакции, управляемые координатором распределенных транзакций ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] ) (MS DTC). Эти транзакции распределяются между двумя или более экземплярами компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].|  
|[Класс событий SQLTransaction](../../relational-databases/event-classes/sqltransaction-event-class.md)|Отслеживает инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] BEGIN TRAN, COMMIT TRAN, SAVE TRAN и ROLLBACK TRAN.|  
|[Класс событий TM: Begin Tran Completed](../../relational-databases/event-classes/tm-begin-tran-completed-event-class.md)|Указывает, что запрос BEGIN TRANSACTION завершен.|  
|[Класс событий TM: Begin Tran Starting](../../relational-databases/event-classes/tm-begin-tran-starting-event-class.md)|Указывает, что запрос BEGIN TRANSACTION начался.|  
|[Класс событий TM: Commit Tran Completed](../../relational-databases/event-classes/tm-commit-tran-completed-event-class.md)|Указывает, что запрос COMMIT TRANSACTION завершен.|  
|[Класс событий TM: Commit Tran Starting](../../relational-databases/event-classes/tm-commit-tran-starting-event-class.md)|Указывает, что запрос COMMIT TRANSACTION начался.|  
|[Класс событий TM: Promote Tran Completed](../../relational-databases/event-classes/tm-promote-tran-completed-event-class.md)|Указывает, что запрос PROMOTE TRANSACTION завершен.|  
|[Класс событий TM: Promote Tran Starting](../../relational-databases/event-classes/tm-promote-tran-starting-event-class.md)|Указывает, что запрос PROMOTE TRANSACTION начался.|  
|[Класс событий TM: Rollback Tran Completed](../../relational-databases/event-classes/tm-rollback-tran-completed-event-class.md)|Указывает, что запрос ROLLBACK TRANSACTION завершен.|  
|[Класс событий TM: Rollback Tran Starting](../../relational-databases/event-classes/tm-rollback-tran-starting-event-class.md)|Указывает, что запрос ROLLBACK TRANSACTION начался.|  
|[Класс событий TM: Save Tran Completed](../../relational-databases/event-classes/tm-save-tran-completed-event-class.md)|Указывает, что запрос SAVE TRANSACTION завершен.|  
|[Класс событий TM: Save Tran Starting](../../relational-databases/event-classes/tm-save-tran-starting-event-class.md)|Указывает, что запрос SAVE TRANSACTION начался.|  
|[Класс событий TransactionLog](../../relational-databases/event-classes/transactionlog-event-class.md)|Отслеживает, когда записи о транзакциях появляются в журнале транзакций базы данных.|  
  
  

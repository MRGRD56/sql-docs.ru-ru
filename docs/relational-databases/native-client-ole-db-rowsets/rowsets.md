---
title: Наборы строк (поставщик собственного клиента OLE DB)
description: Разрешить всем поставщикам данных предоставлять данные результирующего набора в табличной форме. В OLE DB используется функция набора строк — набор строк, содержащий столбцы данных.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [OLE DB], about rowsets
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets
- OLE DB rowsets, about rowsets
- rowsets [OLE DB]
ms.assetid: 5e7b3cbe-3670-4e18-8172-2226e0b6b142
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7b149038a7129b2327156cc28967520f6386d1b6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755884"
---
# <a name="rowsets-native-client-ole-db-provider"></a>Наборы строк (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Набор строк — это несколько строк, содержащих столбцы данных. Наборы строк — это основные объекты, позволяющие всем поставщикам данных OLE DB представлять данные результирующих наборов в виде таблиц.  
  
 После создания сеанса с помощью метода **IDBCreateSession::CreateSession** потребитель может использовать интерфейс **IOpenRowset** или **IDBCreateCommand** этого сеанса для создания набора строк. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента поддерживает оба этих интерфейса. Оба эти метода описаны здесь.  
  
-   Создайте набор строк, вызвав метод **IOpenRowset::OpenRowset**.  
  
     Это эквивалентно созданию набора строк из одной таблицы. Этот метод открывает и возвращает набор строк, включающий все строки одной базовой таблицы. Одним из аргументов метода **OpenRowset** является идентификатор таблицы, указывающий таблицу, на основе которой создается набор строк.  
  
-   Создайте объект команд с помощью метода **IDBCreateCommand::CreateCommand**.  
  
     Объект команд выполняет команды, поддерживаемые поставщиком. С помощью поставщика OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] потребитель может указать любую инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)], например SELECT, или вызов хранимой процедуры. Создание набора строк с помощью объекта команд включает следующие шаги.  
  
    1.  Потребитель вызывает метод **IDBCreateCommand::CreateCommand** для сеанса, чтобы получить объект команд, запрашивающий интерфейс **ICommandText** для объекта команд. Этот интерфейс **ICommandText** задает и получает действительный текст команды. Потребитель заполняет текст команды путем вызова метода **ICommandText::SetCommandText**.  
  
    2.  Пользователь вызывает метод **ICommand::Execute** для команды. Объект набора строк, создаваемый во время выполнения команды, содержит результирующий набор этой команды.  
  
 Потребитель может использовать интерфейс **ICommandProperties** для получения или задания свойств набора строк, возвращаемого командой, которая выполняется интерфейсом **ICommand::Execute**. Наиболее часто запрашиваемыми свойствами являются интерфейсы, которые должны поддерживаться набором строк. Кроме интерфейсов, потребитель может запросить свойства, изменяющие поведение набора строк или интерфейса.  
  
 Потребители освобождают наборы строк с помощью метода **IRowset::Release**. При освобождении набора строк освобождаются все дескрипторы строк, удерживаемые потребителем для данного набора строк. При освобождении набора строк методы доступа не освобождаются. Если используется интерфейс **IAccessor**, его нужно освободить отдельно.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Создание набора строк с помощью интерфейса IOpenRowset](../../relational-databases/native-client-ole-db-rowsets/creating-a-rowset-with-iopenrowset.md)  
  
-   [Создание наборов строк с помощью метода ICommand::Execute](../../relational-databases/native-client-ole-db-rowsets/creating-rowsets-with-icommand-execute.md)  
  
-   [Свойства и поведение наборов строк](../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)  
  
-   [Наборы строк и курсоры SQL Server](../../relational-databases/native-client-ole-db-rowsets/rowsets-and-sql-server-cursors.md)  
  
-   [Выборка строк](../../relational-databases/native-client-ole-db-rowsets/fetching-rows.md)  
  
-   [Выборка одной строки с помощью интерфейса IRow](../../relational-databases/native-client-ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
-   [Закладки](../../relational-databases/native-client-ole-db-rowsets/bookmarks.md)  
  
-   [Обновление данных в наборах строк](../../relational-databases/native-client-ole-db-rowsets/updating-data-in-rowsets.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  

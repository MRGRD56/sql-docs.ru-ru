---
title: OLE DB
description: Поставщик SQL Server Native Client OLE DB — это COM-интерфейс API для доступа к данным, используемый для инструментов, служебных программ или низкоуровневых компонентов, требующих высокой производительности.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, about SQL Server Native Client OLE DB provider
- OLE DB, SQL Server Native Client OLE DB provider
- data access [SQL Server Native Client], OLE DB
- SQLNCLI, OLE DB
- OLE DB
- SQL Server Native Client OLE DB provider
- SQL Server Native Client, OLE DB
ms.assetid: da846da4-ec19-4a4f-81fb-7d5a2b2bf80a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6749bffc5903f2fd0345e6770dd18d00213103bb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104744934"
---
# <a name="sql-server-native-client-ole-db"></a>Собственный клиент SQL Server (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента (SQLNCLI) — это COM-API низкого уровня, который используется для доступа к данным. Поставщик OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] рекомендуется для разработки средств, программ и низкоуровневых компонентов, требующих высокой производительности. Поставщик OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] — собственный, высокопроизводительный поставщик, который напрямую обращается к протоколу потока табличных данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Собственный клиент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] обеспечивает поддержку OLE DB для приложений, соединяющихся с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента — это поставщик, соответствующий OLE DB версии 2,0.  
 
> [!IMPORTANT]
> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Собственный клиент OLE DB (SQLNCLI) остается устаревшим и не рекомендуется использовать его для новых задач разработки. Вместо этого используйте новый драйвер [Microsoft OLE DB для SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), который будет обновлен с самыми последними серверными компонентами.
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Создание приложения поставщика OLE DB для собственного клиента SQL Server](../../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)  
  
-   [Объекты источников данных (OLE DB)](../../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
-   [Команды](../../../relational-databases/native-client-ole-db-commands/commands.md)  
  
-   [Наборы строк](../../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
-   [Хранимые процедуры](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
-   [Большие двоичные объекты и объекты OLE](../../../relational-databases/native-client-ole-db-blobs/blobs-and-ole-objects.md)  
  
-   [Таблицы и индексы](../../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
-   [Типы данных (OLE DB)](../../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
-   [Поддержка набора строк схемы &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/schema-rowset-support-ole-db.md)  
  
-   [Возвращающие табличные значения параметры &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)  
  
-   [Улучшения функций даты и времени &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
-   [Большие определяемые пользователем типы данных CLR &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/large-clr-user-defined-types-ole-db.md)  
  
-   [Поддержка FILESTREAM &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/filestream-support-ole-db.md)  
  
-   [Транзакции](../../../relational-databases/native-client-ole-db-transactions/transactions.md)  
  
-   [ошибки](../../../relational-databases/native-client-ole-db-errors/errors.md)  
  
-   [Имена субъектов-служб &#40;SPN&#41; в клиентских соединениях &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/service-principal-names-spns-in-client-connections-ole-db.md)  
  
-   [Поддержка разреженных столбцов &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/sparse-columns-support-ole-db.md)  
  
-   [Справочник по SQL Server Native Client &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md)  
  
-   [Инструкции по OLE DB](../../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
## <a name="see-also"></a>См. также:  
 [Программирование собственного клиента SQL Server](../../../relational-databases/native-client/sql-server-native-client-programming.md)  
  
  

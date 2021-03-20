---
description: Ошибки собственного клиента SQL Server
title: Ошибки | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, errors
- OLE/COM errors
- errors [OLE DB]
- OLE DB error handling, about error handling
- OLE DB error handling
ms.assetid: bd0612f4-96ef-4919-b0f9-b5447210fe93
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b629595c3fb26dd8a0694c068187ffc9f714fb59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755944"
---
# <a name="sql-server-native-client-errors"></a>Ошибки собственного клиента SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Объекты OLE/COM сообщают об ошибках с помощью кодов возврата HRESULT функций-членов объектов. Тип HRESULT в OLE/COM представляет собой структуру с битовой упаковкой. OLE предоставляет макросы для разыменования членов структуры.  
  
 OLE/COM задает интерфейс **IErrorInfo**. Интерфейс предоставляет доступ к методам (например, **GetDescription**). Это позволяет клиентам получать подробную информацию об ошибках у серверов OLE/COM. В OLE DB к интерфейсу **IErrorInfo** добавлена поддержка возврата сразу нескольких пакетов информации об ошибках на один вызов метода.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может возвращать несколько ошибок. Приложение может получать ошибки сервера поочередно, вызывая метод [IMultipleResults::GetResult](/previous-versions/windows/desktop/ms721289(v=vs.85)) в сочетании с ISQLErrorInfo и IErrorRecords.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента предоставляет OLE DB расширенный интерфейс **IErrorInfo** с записью, Пользовательский **ISQLErrorInfo** и интерфейсы объекта [ISQLServerErrorInfo](../native-client-ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md) Error, относящиеся к конкретному поставщику.  
  
 Сведения об ошибках трассировки см. в статье [Отслеживание доступа к данным](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). См. сведения об улучшениях трассировки ошибок, добавленных в [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], в руководстве по [получению доступа к диагностическим сведениям в расширенном журнале событий](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Коды возврата](../../relational-databases/native-client-ole-db-errors/return-codes.md)  
  
-   [Сведения в интерфейсах ошибок](../../relational-databases/native-client-ole-db-errors/information-in-error-interfaces.md)  
  
-   [Подробные сведения об ошибках SQL Server](../../relational-databases/native-client-ole-db-errors/sql-server-error-detail.md)  
  
-   [Получение информации об ошибке](../../relational-databases/native-client-ole-db-errors/retrieving-error-information.md)  
  
-   [Результаты сообщения SQL Server](../../relational-databases/native-client-ole-db-errors/sql-server-message-results.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  

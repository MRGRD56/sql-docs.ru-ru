---
title: OLE DB Errors
description: Узнайте, как ошибки возвращаются в драйвере OLE DB Driver for SQL Server и как можно получить сведения о них.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, errors
- OLE/COM errors
- errors [OLE DB]
- OLE DB error handling, about error handling
- OLE DB error handling
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: eb30ffa542d584626f762c0d0eeb990426a5f152
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741954"
---
# <a name="errors"></a>ошибки
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Объекты OLE/COM сообщают об ошибках с помощью кодов возврата HRESULT функций-членов объектов. Тип HRESULT в OLE/COM представляет собой структуру с битовой упаковкой. OLE предоставляет макросы для разыменования членов структуры.  
  
 OLE/COM задает интерфейс **IErrorInfo**. Интерфейс предоставляет доступ к методам (например, **GetDescription**). Это позволяет клиентам получать подробную информацию об ошибках у серверов OLE/COM. В OLE DB к интерфейсу **IErrorInfo** добавлена поддержка возврата сразу нескольких пакетов информации об ошибках на один вызов метода.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] может возвращать несколько ошибок. Приложение может получать ошибки сервера поочередно, вызывая метод [IMultipleResults::GetResult](/previous-versions/windows/desktop/ms721289(v=vs.85)) в сочетании с ISQLErrorInfo и IErrorRecords.  
  
 Драйвер OLE DB для SQL Server предоставляет доступ к интерфейсам объектов ошибок: улучшенному в OLE DB интерфейсу **IErrorInfo**, настраиваемому интерфейсу **ISQLErrorInfo** и специфичному для провайдера интерфейсу [ISQLServerErrorInfo](../ole-db-interfaces/isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 Сведения об ошибках трассировки см. в статье [Отслеживание доступа к данным](/previous-versions/sql/sql-server-2008/cc765421(v=sql.100)). См. сведения об улучшениях трассировки ошибок, добавленных в [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], в руководстве по [получению доступа к диагностическим сведениям в расширенном журнале событий](../../oledb/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Коды возврата](../../oledb/ole-db-errors/return-codes.md)  
  
-   [Сведения в интерфейсах ошибок](../../oledb/ole-db-errors/information-in-error-interfaces.md)  
  
-   [Подробные сведения об ошибках SQL Server](../../oledb/ole-db-errors/sql-server-error-detail.md)  
  
-   [Получение информации об ошибке](../../oledb/ole-db-errors/retrieving-error-information.md)  
  
-   [Результаты сообщения SQL Server](../../oledb/ole-db-errors/sql-server-message-results.md)  
  
## <a name="see-also"></a>См. также:  
 [Программирование драйвера OLE DB для SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  

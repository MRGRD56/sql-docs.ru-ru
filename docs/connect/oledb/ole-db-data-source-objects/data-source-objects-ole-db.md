---
title: Объекты источника данных (драйвер OLE DB) | Документация Майкрософт
description: Узнайте, как объект-получатель OLE DB Driver for SQL Server создает экземпляр объекта источника данных для поставщика.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], data source objects
- OLE DB Driver for SQL Server, data source objects
- MSOLEDBSQL, data source objects
- OLE DB Driver for SQL Server, data source objects
- OLE DB data source objects [OLE DB Driver for SQL Server]
- data source objects [OLE DB]
- CLSID
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 53208b567de362b388e5fb2183c67dccdabb2d2d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735210"
---
# <a name="data-source-objects-ole-db"></a>Объекты источников данных (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Драйвер OLE DB для SQL Server использует термин "источник данных" для обозначения набора интерфейсов OLE DB, дающих возможность устанавливать соединения с хранилищем данных, например с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Первая задача потребителя OLE DB Driver for SQL Server — создание экземпляра объекта для источника данных поставщика.  
  
 Каждый поставщик OLE DB объявляет свой идентификатор класса (CLSID). OLE DB Driver for SQL Server в C/C++ имеет уникальный идентификатор (CLSID) CLSID_MSOLEDBSQL (это означает, что символ CLSID_MSOLEDBSQL будет разрешен в правильный идентификатор progid из файла msoledbsql.h, на который вы ссылаетесь). Используя CLSID, потребитель может вызвать функцию OLE **CoCreateInstance** для создания экземпляра объекта источника данных.  
  
 OLE DB Driver for SQL Server является внутрипроцессным сервером. Экземпляры объектов драйвера OLE DB для SQL Server создаются с помощью макроса CLSCTX_INPROC_SERVER для указания на исполняемый контекст.  
  
 Объект источника данных драйвера OLE DB для SQL Server предоставляет интерфейсы инициализации OLE DB, которые позволяют потребителю подключаться к существующим базам данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Все подключения, созданные через OLE DB Driver for SQL Server, автоматически устанавливают следующие параметры:  
  
-   SET ANSI_WARNINGS ON  
  
-   SET ANSI_NULLS ON  
  
-   SET ANSI_PADDING ON  
  
-   SET ANSI_NULL_DFLT_ON ON  
  
-   SET QUOTED_IDENTIFIER ON  
  
-   SET CONCAT_OF_NULL_YIELDS_NULL ON  
  
 В этом примере используется макрос идентификатора класса для создания объекта источника данных драйвера OLE DB для SQL Server и получения ссылки на его интерфейс **IDBInitialize**.  
  
```  
IDBInitialize*   pIDBInitialize;  
HRESULT          hr;  
  
hr = CoCreateInstance(CLSID_MSOLEDBSQL, NULL, CLSCTX_INPROC_SERVER,  
    IID_IDBInitialize, (void**) &pIDBInitialize);  
  
if (SUCCEEDED(hr))  
{  
    //  Perform necessary processing with the interface.  
    pIDBInitialize->Uninitialize();  
    pIDBInitialize->Release();  
}  
else  
{  
    // Display error from CoCreateInstance.  
}  
```  
  
 После успешного создания экземпляра объекта источника данных драйвера OLE DB для SQL Server приложение-потребитель может инициализировать источник данных и создавать сеансы. Сеансы OLE DB предоставляют интерфейсы, через которые можно получать доступ к данным и манипулировать ими.  
  
 Драйвер OLE DB для SQL Server выполняет первое соединение с указанным экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в ходе успешной инициализации источника данных. Соединение с источником данных поддерживается до тех пор, пока хранятся ссылки на любой интерфейс инициализации источника данных или пока не вызван метод **IDBInitialize::Uninitialize**.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Свойства источника данных &#40;OLE DB&#41;](../../oledb/ole-db-data-source-objects/data-source-properties-ole-db.md)  
  
-   [Свойства сведений об источнике данных](../../oledb/ole-db-data-source-objects/data-source-information-properties.md)  
  
-   [Свойства инициализации и авторизации](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)  
  
-   [Сеансы](../../oledb/ole-db-data-source-objects/sessions.md)  
  
-   [Свойства сеанса — драйвер OLE DB для SQL Server](../../oledb/ole-db-data-source-objects/session-properties-oledb-driver-for-sql-server.md)  
  
-   [Материализованные объекты источника данных](../../oledb/ole-db-data-source-objects/persisted-data-source-objects.md)  
  
## <a name="see-also"></a>См. также:  
 [Программирование драйвера OLE DB для SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  

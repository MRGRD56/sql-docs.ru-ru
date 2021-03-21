---
title: Выборка одной строки через IRow (драйвер OLE DB) | Документация Майкрософт
description: Интерфейс IRow предоставляет прямой доступ к столбцам одного объекта, представляющего собой строку. Интерфейс IRow в OLE DB Driver for SQL Server упрощен для повышения производительности.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- fetching rows
- IRow interface
- single row fetching [OLE DB Driver for SQL Server]
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- OLE DB Driver for SQL Server, fetching
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 389bbbfb6e01d962f4130f5b8a06c8f6ba534fe2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753074"
---
# <a name="fetching-a-single-row-with-irow-ole-db-driver"></a>Выборка одной строки через IRow (драйвер OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Реализация интерфейса **IRow** в OLE DB Driver for SQL Server упрощена для повышения производительности. Интерфейс **IRow** предоставляет прямой доступ к столбцам одного объекта, представляющего собой строку. Если заранее известно, что результатом выполнения команды будет ровно одна строка, **IRow** даст возможность получить столбцы этой строки. Если в результирующий набор входит несколько строк, интерфейс **IRow** предоставит доступ только к первой.  
  
 Реализация интерфейса **IRow** не позволяет перемещаться по строке. Обращение к каждому столбцу строки выполняется ровно один раз, с единственным исключением: возможно одно обращение к столбцу для выяснения его размера и второе для получения данных.  
  
> [!NOTE]  
>  Метод **IRow::Open** поддерживает открытие только объектов типа DBGUID_STREAM или DBGUID_NULL.  
  
 Для получения объекта строки с помощью метода **ICommand::Execute** нужно передать в качестве параметра идентификатор IID_IRow. Обработка нескольких результирующих наборов производится с помощью интерфейса **IMultipleResults**. Интерфейс **IMultipleResults** поддерживает интерфейсы **IRow** и **IRowset**. Интерфейс **IRowset** используется для массовых операций.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Использование метода IRow::GetColumns](../../oledb/ole-db-rowsets/using-irow-getcolumns.md)   
  
## <a name="see-also"></a>См. также:  
 [Наборы строк](../../oledb/ole-db-rowsets/rowsets.md)  
  
  

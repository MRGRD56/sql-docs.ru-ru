---
title: IRowsetFastLoad::InsertRow (драйвер OLE DB) | Документация Майкрософт
description: Узнайте, как метод IRowsetFastLoad::InsertRow добавляет строку в набор строк при массовом копировании в драйвере OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IRowsetFastLoad::InsertRow (OLE DB)
apitype: COM
helpviewer_keywords:
- InsertRow method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3c48064ea8b75206d593f674335470ec6ae737dc
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2020
ms.locfileid: "88860105"
---
# <a name="irowsetfastloadinsertrow-ole-db"></a>IRowsetFastLoad::InsertRow (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Добавляет строку в набор строк для массового копирования. Примеры можно найти в статьях [Выполнение массового копирования данных с использованием интерфейса IRowsetFastLoad (OLE DB)](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) и [Отправка данных BLOB-объектов в SQL Server с помощью интерфейсов IROWSETFASTLOAD и ISEQUENTIALSTREAM (OLE DB)](../../oledb/ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT InsertRow(  
      HACCESSOR hAccessor,  
      void* pData);  
```  
  
## <a name="arguments"></a>Аргументы  
 *hAccessor*[in]  
 Дескриптор метода доступа, определяющий данные строк для массового копирования. Указанный метод доступа является методом доступа к строке, связывающий память потребителя, содержащую значения данных.  
  
 *pData*[in]  
 Указатель на память потребителя, содержащую значения данных. Дополнительные сведения см. в разделе [Структуры DBBINDING](https://go.microsoft.com/fwlink/?LinkId=65955).  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод выполнен успешно. Любые связанные значения состояния для всех столбцов имеют значение DBSTATUS_S_OK или DBSTATUS_S_NULL.  
  
 E_FAIL  
 Произошла ошибка. Сведения об ошибках можно получить с помощью интерфейса обработки ошибок набора строк.  
  
 E_INVALIDARG  
 Аргумент *pData* содержит указатель NULL.  
  
 E_OUTOFMEMORY  
 MSOLEDBSQL не удалось выделить достаточно памяти для завершения запроса.  
  
 E_UNEXPECTED  
 Этот метод был вызван применительно к набору строк массового копирования, который ранее стал недействительным в результате выполнения метода [IRowsetFastLoad::Commit](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md).  
  
 DB_E_BADACCESSORHANDLE  
 Потребитель предоставил недопустимый аргумент *hAccessor* .  
  
 DB_E_BADACCESSORTYPE  
 Указанный метод доступа не является методом доступа к строке или не указывает память потребителя.  
  
## <a name="remarks"></a>Remarks  
 Ошибка при преобразовании данных потребителя в тип данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для столбца приводит к тому, что драйвер OLE DB для SQL Server возвращает E_FAIL. Данные могут передаваться в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] любым методом **InsertRow** или только методом **Commit**. Приложение потребителя может вызывать метод **InsertRow** много раз с ошибочными данными, прежде чем получит уведомление, что при преобразовании типов данных произошла ошибка. Поскольку метод **Commit** гарантирует, что все данные были правильно указаны потребителем, потребитель может при необходимости использовать метод **Commit** для проверки данных.  
  
 Наборы строк для массового копирования в OLE DB Driver for SQL Server доступны только для записи. OLE DB Driver for SQL Server не предоставляет методов, позволяющих потребителю запрашивать наборы строк. Чтобы прервать обработку, потребитель может освободить ссылку на интерфейс [IRowsetFastLoad](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md), не вызывая метод **Commit**. Невозможно получить доступ к вставленной потребителем строке, изменить ее значения или удалить ее из набора строк.  
  
 Массово скопированные строки форматируются на сервере для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Формат строки зависит от любых параметров, которые могли быть заданы для соединения или сеанса, например ANSI_PADDING. Этот параметр по умолчанию включен для любого соединения, установленного через OLE DB Driver for SQL Server.  
  
## <a name="see-also"></a>См. также:  
 [IRowsetFastLoad (OLE DB)](../../oledb/ole-db-interfaces/irowsetfastload-ole-db.md)  
  
  

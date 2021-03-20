---
description: 'IRowsetFastLoad:: InsertRow (поставщик собственного клиента OLE DB)'
title: 'IRowsetFastLoad:: InsertRow (поставщик собственного клиента OLE DB) | Документация Майкрософт'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IRowsetFastLoad::InsertRow (OLE DB)
apitype: COM
helpviewer_keywords:
- InsertRow method
ms.assetid: 594d3461-34d2-41e7-8ad4-bd2753601ab6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d78beded73205abbfa266603c50d98fa4f35540e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755694"
---
# <a name="irowsetfastloadinsertrow-native-client-ole-db-provider"></a>IRowsetFastLoad:: InsertRow (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Добавляет строку в набор строк для массового копирования. Примеры можно найти в статьях [Выполнение массового копирования данных с использованием интерфейса IRowsetFastLoad (OLE DB)](../../relational-databases/native-client-ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) и [Отправка данных BLOB-объектов в SQL Server с помощью интерфейсов IROWSETFASTLOAD и ISEQUENTIALSTREAM (OLE DB)](../../relational-databases/native-client-ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)  
  
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
 Указатель на память потребителя, содержащую значения данных. Дополнительные сведения см. в разделе [Структуры DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)).  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод выполнен успешно. Любые связанные значения состояния для всех столбцов имеют значение DBSTATUS_S_OK или DBSTATUS_S_NULL.  
  
 E_FAIL  
 Произошла ошибка. Сведения об ошибках можно получить с помощью интерфейса обработки ошибок набора строк.  
  
 E_INVALIDARG  
 Аргумент *pData* содержит указатель NULL.  
  
 E_OUTOFMEMORY  
 SQLNCLI11 не удалось выделить достаточно памяти для завершения запроса.  
  
 E_UNEXPECTED  
 Этот метод был вызван применительно к набору строк массового копирования, который ранее стал недействительным в результате выполнения метода [IRowsetFastLoad::Commit](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-commit-ole-db.md).  
  
 DB_E_BADACCESSORHANDLE  
 Потребитель предоставил недопустимый аргумент *hAccessor* .  
  
 DB_E_BADACCESSORTYPE  
 Указанный метод доступа не является методом доступа к строке или не указывает память потребителя.  
  
## <a name="remarks"></a>Remarks  
 Ошибка при преобразовании данных потребителя в тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для столбца приводит к тому, что поставщик OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает E_FAIL. Данные могут передаваться в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] любым методом **InsertRow** или только методом **Commit**. Приложение потребителя может вызывать метод **InsertRow** много раз с ошибочными данными, прежде чем получит уведомление, что при преобразовании типов данных произошла ошибка. Поскольку метод **Commit** гарантирует, что все данные были правильно указаны потребителем, потребитель может при необходимости использовать метод **Commit** для проверки данных.  
  
 Наборы строк для массового копирования поставщиком OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] доступны только для записи. Поставщик OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не предоставляет методов, позволяющих потребителю запрашивать наборы строк. Чтобы прервать обработку, потребитель может освободить ссылку на интерфейс [IRowsetFastLoad](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md), не вызывая метод **Commit**. Невозможно получить доступ к вставленной потребителем строке, изменить ее значения или удалить ее из набора строк.  
  
 Массово скопированные строки форматируются на сервере для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Формат строки зависит от любых параметров, которые могли быть заданы для соединения или сеанса, например ANSI_PADDING. Этот параметр по умолчанию включен для любого соединения, установленного с помощью поставщика OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [IRowsetFastLoad (OLE DB)](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md)  
  

---
title: ISSAsynchStatus::Abort (драйвер OLE DB) | Документация Майкрософт
description: Узнайте, как метод ISSAsynchStatus::Abort отменяет асинхронно выполняющуюся операцию в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus::Abort (OLE DB)
apitype: COM
helpviewer_keywords:
- Abort method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 61727826f44ec4c358b75760776f6a38b372e877
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104738684"
---
# <a name="issasynchstatusabort-ole-db"></a>ISSAsynchStatus::Abort (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Отменяет операцию асинхронного выполнения.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT Abort(  
        HCHAPTER hChapter,  
        DBASYNCHOP eOperation);  
```  
  
## <a name="arguments"></a>Аргументы  
 *hChapter*[in]  
 Дескриптор раздела, для которого прерывается операция. Если вызываемый объект не является объектом набора строк или операция не применяется к разделу, вызывающий должен присвоить параметру *hChapter* значение DB_NULL_HCHAPTER.  
  
 *eOperation*[in]  
 Операция, которая должна быть прервана. Следует использовать следующее значение:  
  
 DBASYNCHOP_OPEN — запрос на отмену применяется к асинхронному открытию или заполнению набора строк или асинхронной инициализации объекта источника данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Запрос на отмену асинхронной операции обработан. Это не гарантирует, что сама операция была отменена. Чтобы определить, отменена ли операция, потребитель должен вызвать метод [ISSAsynchStatus::GetStatus](../../oledb/ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) и проверить наличие DB_E_CANCELED; однако это значение может быть не возвращено в следующем вызове.  
  
 DB_E_CANTCANCEL  
 Асинхронную операцию невозможно отменить.  
  
 DB_E_CANCELED  
 Запрос на прерывание асинхронной операции был отменен во время отправки уведомлений. Операция все еще выполняется асинхронно.  
  
 E_FAIL  
 Произошла ошибка, зависящая от поставщика.  
  
 E_INVALIDARG  
 Значение параметра *hChapter* не равно DB_NULL_HCHAPTER или значение параметра *eOperation* не равно DBASYNCH_OPEN.  
  
 E_UNEXPECTED  
 Метод `ISSAsynchStatus::Abort` был вызван для объекта источника данных, для которого не был вызван или не был завершен метод `IDBInitialize::Initialize`.  
  
 Метод `ISSAsynchStatus::Abort` был вызван для объекта источника данных, для которого был вызван метод `IDBInitialize::Initialize`, но затем он был отменен до инициализации, или для него истекло время ожидания. Объект источника данных еще не инициализирован.  
  
 Метод `ISSAsynchStatus::Abort` был вызван для набора строк, для которого ранее был вызван метод `ITransaction::Commit` или `ITransaction::Abort`, а набор строк не сохранился после фиксации или отмены и находится в состоянии зомби.  
  
 Интерфейс`ISSAsynchStatus::Abort` был вызван для набора строк, который был асинхронно отменен на стадии его инициализации. Набор строк находится в состоянии зомби.  
  
## <a name="remarks"></a>Remarks  
 При прерывании инициализации набора строк или объекта источника данных набор строк или объект источника данных может перейти в состояние зомби, когда все методы, кроме методов `IUnknown` , возвращают E_UNEXPECTED. В этом случае единственным возможным для потребителя действием является освобождение набора строк или объекта источника данных.  
  
 При вызове интерфейса `ISSAsynchStatus::Abort` и передаче параметру *eOperation* значения, отличного от DBASYNCHOP_OPEN, возвращается S_OK. Это значение не подразумевает, что сама операция была завершена или отменена.  
  
## <a name="see-also"></a>См. также:  
 [Выполнение асинхронных операций](../../oledb/features/performing-asynchronous-operations.md)  
  
  

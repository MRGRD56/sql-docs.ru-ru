---
title: Выполнение асинхронных операций | Документация Майкрософт
description: OLE DB Driver for SQL Server поддерживает асинхронные операции базы данных, которые позволяют методам возвращать управление, не блокируя вызывающий поток.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- initialization [OLE DB Driver for SQL Server]
- database connections [OLE DB Driver for SQL Server]
- data access [OLE DB Driver for SQL Server], asynchronous operations
- connections [OLE DB Driver for SQL Server]
- asynchronous operations [OLE DB Driver for SQL Server]
- rowsets [SQL Server], initializing
- MSOLEDBSQL, asynchronous operations
- OLE DB Driver for SQL Server, asynchronous operations
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d71b9884b8a87ae05060073f9c2d47eb044387a5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742294"
---
# <a name="performing-asynchronous-operations"></a>Выполнение асинхронных операций
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] позволяет приложениям выполнять асинхронные операции с базой данных. Асинхронная обработка позволяет выполнять возврат из методов немедленно, не блокируя вызывающий поток. Это позволяет использовать значительную часть мощности и гибкости многопотоковой обработки, и разработчику при этом не требуется явно создавать потоки или обрабатывать синхронизацию. Приложения запрашивают асинхронную обработку при инициализации подключения к базе данных или при инициализации результата выполнения команды.  
  
## <a name="opening-and-closing-a-database-connection"></a>Открытие и закрытие подключения к базе данных  
 При использовании драйвера OLE DB для SQL Server приложения, способные асинхронно инициализировать объект источника данных, могут задать бит DBPROPVAL_ASYNCH_INITIALIZE в свойстве DBPROP_INIT_ASYNCH до вызова функции **IDBInitialize::Initialize**. Когда это свойство задано, поставщик обеспечивает немедленный возврат из метода **Initialize** с результатом S_OK, если операция была завершена немедленно, или DB_S_ASYNCHRONOUS, если инициализация продолжается асинхронно. Приложения могут запросить интерфейс **IDBAsynchStatus** или [ISSAsynchStatus](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md) для объекта источника данных, а затем вызывать метод **IDBAsynchStatus::GetStatus** или [ISSAsynchStatus::WaitForAsynchCompletion](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md), чтобы получить состояние инициализации.  
  
 Кроме того, в набор свойств DBPROPSET_SQLSERVERROWSET добавлено свойство SSPROP_ISSAsynchStatus. Поставщики, поддерживающие интерфейс **ISSAsynchStatus**, должны реализовывать это свойство со значением VARIANT_TRUE.  
  
 Функции **IDBAsynchStatus::Abort** и [ISSAsynchStatus::Abort](../../oledb/ole-db-interfaces/issasynchstatus-abort-ole-db.md) могут быть вызваны для отмены асинхронного вызова **Initialize**. Потребитель должен явно запросить асинхронную инициализацию источника данных. В противном случае возврат из метода **IDBInitialize::Initialize** не происходит до тех пор, пока объект источника данных не будет инициализирован полностью.  
  
> [!NOTE]  
>  Объекты источников данных, используемые для организации пула соединений, не могут вызывать интерфейс **ISSAsynchStatus** в драйвере OLE DB для SQL Server. Интерфейс **ISSAsynchStatus** недоступен для объединенных в пул объектов источников данных.  
>   
>  Если приложение явно и принудительно использует ядро курсора, методы **IOpenRowset::OpenRowset** и **IMultipleResults::GetResult** не будут поддерживать асинхронную обработку.  
>   
>  Кроме того, при удаленном взаимодействии библиотека DLL посредника-заглушки (в компонентах MDAC 2.8) не может вызвать интерфейс **ISSAsynchStatus** в драйвере OLE DB для SQL Server. Интерфейс **ISSAsynchStatus** недоступен при удаленном взаимодействии.  
>   
>  Компоненты служб не поддерживают интерфейс **ISSAsynchStatus**.  
  
## <a name="execution-and-rowset-initialization"></a>Выполнение и инициализация наборов строк  
 Приложения, способные асинхронно открывать результаты выполнения команд, могут установить бит DBPROPVAL_ASYNCH_INITIALIZE в свойстве DBPROP_ROWSET_ASYNCH. Если задать этот бит перед вызовом **IDBInitialize::Initialize**, **ICommand::Execute**, **IOpenRowset::OpenRowset** или **IMultipleResults::GetResult**, значение аргумента *riid* должно быть IID_IDBAsynchStatus, IID_ISSAsynchStatus или IID_IUnknown.  
  
 Метод возвращается немедленно с результатом S_OK, если инициализация набора строк завершается немедленно, или с результатом DB_S_ASYNCHRONOUS, если инициализация набора строк продолжается асинхронно, а параметр *ppRowset* указывает запрошенный интерфейс для набора строк. Для OLE DB Driver for SQL Server таким интерфейсом может выступать только **IDBAsynchStatus** или **ISSAsynchStatus**. Пока набор строк не инициализирован полностью, этот интерфейс действует, как если бы он был приостановлен, и при вызове метода **QueryInterface** для получения интерфейсов, отличных от **IID_IDBAsynchStatus** или **IID_ISSAsynchStatus**, может быть возращена ошибка E_NOINTERFACE. Если потребитель явно не запросил асинхронную обработку, набор строк инициализируется синхронно. Все запрашиваемые интерфейсы доступны, когда метод **IDBAsynchStaus::GetStatus** или **ISSAsynchStatus::WaitForAsynchCompletion** возвращается с указанием того, что асинхронная операция завершена. Это не обязательно означает, что набор строк заполнен, но он завершен и полностью функционален.  
  
 Если выполненная команда не возвращает набор строк, она все равно возвращается немедленно с объектом, поддерживающим **IDBAsynchStatus**.  
  
 Если требуется получить несколько результатов асинхронного выполнения команды, выполните следующие действия.  
  
-   Установите бит DBPROPVAL_ASYNCH_INITIALIZE свойства DBPROP_ROWSET_ASYNCH перед выполнением команды.  
  
-   Вызовите метод **ICommand::Execute** и запросите интерфейс **IMultipleResults**.  
  
 Интерфейсы **IDBAsynchStatus** и **ISSAsynchStatus** могут быть получены путем запроса интерфейса множественных результатов с помощью метода **QueryInterface**.  
  
 Когда выполнение команды завершается, интерфейс **IMultipleResults** можно использовать обычным образом с единственным отличием от синхронной обработки: может быть возвращен результат DB_S_ASYNCHRONOUS, и в этом случае интерфейсы **IDBAsynchStatus** и **ISSAsynchStatus** можно использовать для определения того, когда завершится операция.  
  
## <a name="examples"></a>Примеры  
 В следующем примере приложение вызывает неблокирующий метод, выполняет некоторую другую обработку, а затем возвращает результаты процессу. Интерфейс **ISSAsynchStatus::WaitForAsynchCompletion** ожидает объект внутреннего события, пока не будет завершена асинхронно выполняющаяся операция или пока не истечет время, указанное в параметре *dwMilisecTimeOut*.  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
  
DBPROPSET CmdPropset[1];  
DBPROP CmdProperties[1];  
  
CmdPropset[0].rgProperties = CmdProperties;  
CmdPropset[0].cProperties = 1;  
CmdPropset[0].guidPropertySet = DBPROPSET_ROWSET;  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
CmdProperties[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
  
hr = pICommandProps->SetProperties(1, CmdPropset);  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if ( hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 Метод **ISSAsynchStatus::WaitForAsynchCompletion** ожидает объект внутреннего события, пока не будет завершена асинхронно выполняющаяся операция или пока не будет передано значение *dwMilisecTimeOut*.  
  
 В следующем примере показана асинхронная обработка с несколькими результирующими наборами.  
  
```  
DBPROP CmdProperties[1];  
  
// Set asynch mode for command.  
CmdProperties[0].dwPropertyID = DBPROP_ROWSET_ASYNCH;  
CmdProperties[0].vValue.vt = VT_I4;  
CmdProperties[0].vValue.lVal = DBPROPVAL_ASYNCH_INITIALIZE;  
  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_IMultipleResults,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pIMultipleResults);  
  
// Use GetResults for ISSAsynchStatus.  
hr = pIMultipleResults->GetResult(IID_ISSAsynchStatus, (void **) &pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work here...  
  
   hr = pISSAsynchStatus->WaitForAsynchCompletion(dwMilisecTimeOut);  
   if (hr == S_OK)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
      pISSAsynchStatus->Release();  
   }  
}  
```  
  
 Чтобы предотвратить блокировку, клиент может проверить состояние выполняющейся асинхронной операции, как показано в следующем примере.  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);   
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   do{  
      // Do some work...  
      hr = pISSAsynchStatus->GetStatus(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN, NULL, NULL, &ulAsynchPhase, NULL);  
   }while (DBASYNCHPHASE_COMPLETE != ulAsynchPhase)  
   if SUCCEEDED(hr)  
   {  
      hr = pISSAsynchStatus->QueryInterface(IID_IRowset, (void**)&pRowset);  
   }  
   pIDBAsynchStatus->Release();  
}  
```  
  
 В следующем примере показано, как можно отменить выполняющуюся в настоящее время асинхронную операцию.  
  
```  
// Set the DBPROPVAL_ASYNCH_INITIALIZE bit in the   
// DBPROP_ROWSET_ASYNCH property before calling Execute().  
hr = pICommand->Execute(  
   pUnkOuter,  
   IID_ISSAsynchStatus,  
   pParams,  
   pcRowsAffected,  
   (IUnknown**)&pISSAsynchStatus);  
  
if (hr == DB_S_ASYNCHRONOUS)  
{  
   // Do some work...  
   hr = pISSAsynchStatus->Abort(DB_NULL_HCHAPTER, DBASYNCHOP_OPEN);  
}  
```  
  
## <a name="see-also"></a>См. также:  
 [Возможности драйвера OLE DB для SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [Свойства и поведение наборов строк](../../oledb/ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [ISSAsynchStatus (OLE DB)](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  

---
title: Получение больших объемов данных (драйвер OLE DB) | Документация Майкрософт
description: Из этого примера вы узнаете, как объект-получатель OLE DB Driver for SQL Server может получить значение с большим объемом данных из одного столбца.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- BLOBs, OLE objects
- DBPROP_ACCESSORDER property
- OLE DB Driver for SQL Server, BLOBs
- large data, OLE objects
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 451500f61a27abcb73f03fa7aac2e1c7a976642a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749194"
---
# <a name="getting-large-data"></a>Возврат больших данных
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В общих случаях потребители должны изолировать код, создающий объект хранилища драйвера OLE DB для SQL Server, от другого кода, обрабатывающего данные, на который не ссылается указатель интерфейса **ISequentialStream**.  
  
 В этой статье описываются возможности, обеспечиваемые следующими функциями:  
  
-   IRowset:GetData  
  
-   IRow::GetColumns  
  
-   ICommand::Execute  
  
 Если свойство DBPROP_ACCESSORDER в группе свойств набора строк имеет значение DBPROPVAL_AO_SEQUENTIAL или DBPROPVAL_AO_SEQUENTIALSTORAGEOBJECTS, потребитель должен получать только одну строку данных в вызове метода **GetNextRows**. Это происходит из-за отсутствия буферизации для данных из больших двоичных объектов. Если свойство DBPROP_ACCESSORDER имеет значение DBPROPVAL_AO_RANDOM, потребитель может получать несколько строк данных в одном вызове метода **GetNextRows**.  
  
 OLE DB Driver for SQL Server не будет извлекать большие объемы данных из [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], пока этого не запросит потребитель. Потребитель должен связывать все короткие типы данных в методе доступа, а затем, если потребуется, использовать один или несколько временных методов доступа для получения значений больших типов данных.  
  
## <a name="example"></a>Пример  
 В этом примере значение большого типа данных получается из одного столбца.  
  
```  
HRESULT GetUnboundData  
    (  
    IRowset* pIRowset,  
    HROW hRow,  
    ULONG nCol,   
    BYTE* pUnboundData  
    )  
    {  
    UINT                cbRow = sizeof(IUnknown*) + sizeof(ULONG);  
    BYTE*               pRow = new BYTE[cbRow];  
  
    DBOBJECT            dbobject;  
  
    IAccessor*          pIAccessor = NULL;  
    HACCESSOR           haccessor;  
  
    DBBINDING           dbbinding;  
    ULONG               ulbindstatus;  
  
    ULONG               dwStatus;  
    ISequentialStream*  pISequentialStream;  
    ULONG               cbRead;  
  
    HRESULT             hr;  
  
    // Set up the DBOBJECT structure.  
    dbobject.dwFlags = STGM_READ;  
    dbobject.iid = IID_ISequentialStream;  
  
    // Create the DBBINDING, requesting a storage-object pointer from  
    // The OLE DB Driver for SQL Server.  
    dbbinding.iOrdinal = nCol;  
    dbbinding.obValue = 0;  
    dbbinding.obStatus = sizeof(IUnknown*);  
    dbbinding.obLength = 0;  
    dbbinding.pTypeInfo = NULL;  
    dbbinding.pObject = &dbobject;  
    dbbinding.pBindExt = NULL;  
    dbbinding.dwPart = DBPART_VALUE | DBPART_STATUS;  
    dbbinding.dwMemOwner = DBMEMOWNER_CLIENTOWNED;  
    dbbinding.eParamIO = DBPARAMIO_NOTPARAM;  
    dbbinding.cbMaxLen = 0;  
    dbbinding.dwFlags = 0;  
    dbbinding.wType = DBTYPE_IUNKNOWN;  
    dbbinding.bPrecision = 0;  
    dbbinding.bScale = 0;  
  
    if (FAILED(hr = pIRowset->  
        QueryInterface(IID_IAccessor, (void**) &pIAccessor)))  
        {  
        // Process QueryInterface failure.  
        return (hr);  
        }  
  
    // Create the accessor.  
    if (FAILED(hr = pIAccessor->CreateAccessor(DBACCESSOR_ROWDATA, 1,  
        &dbbinding, 0, &haccessor, &ulbindstatus)))  
        {  
        // Process error from CreateAccessor.  
        pIAccessor->Release();  
        return (hr);  
        }  
  
    // Read and process BLOCK_SIZE bytes at a time.  
    if (SUCCEEDED(hr = pIRowset->GetData(hRow, haccessor, pRow)))  
        {  
        dwStatus = *((ULONG*) (pRow + dbbinding.obStatus));  
  
        if (dwStatus == DBSTATUS_S_ISNULL)  
            {  
            // Process NULL data  
            }  
        else if (dwStatus == DBSTATUS_S_OK)  
            {  
            pISequentialStream = *((ISequentialStream**)   
                (pRow + dbbinding.obValue));  
  
            do  
                {  
                if (SUCCEEDED(hr =  
                    pISequentialStream->Read(pUnboundData,  
                    BLOCK_SIZE, &cbRead)))  
                    {  
                    pUnboundData += cbRead;  
                    }  
                }  
            while (SUCCEEDED(hr) && cbRead >= BLOCK_SIZE);  
  
            pISequentialStream->Release();  
            }  
        }  
    else  
        {  
        // Process error from GetData.  
        }  
  
    pIAccessor->ReleaseAccessor(haccessor, NULL);  
    pIAccessor->Release();  
    delete [] pRow;  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>См. также:  
 [Большие двоичные объекты и объекты OLE](../../oledb/ole-db-blobs/blobs-and-ole-objects.md)   
 [Использование типов больших значений](../../oledb/features/using-large-value-types.md)  
  
  

---
title: Материализованные объекты источника данных (драйвер OLE DB)
description: Из этих примеров вы узнаете о поддержке использования материализованных объектов источника данных с помощью интерфейса IPersistFile в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB data source objects [OLE DB Driver for SQL Server]
- data source objects [OLE DB]
- OLE DB Driver for SQL Server, persisted data source objects
- persisted data source objects
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4ebb7eeb25fb3a3b77894ddc2ec2994d5369e8a9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755574"
---
# <a name="persisted-data-source-objects"></a>Материализованные данные исходного объекта
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server поддерживает материализованные объекты источника данных с помощью интерфейса **IPersistFile**.  
  
## <a name="examples"></a>Примеры  
 **А. Сохранение инициализации источника данных:**  
  
 В данном примере показана функция, которая сохраняет свойства инициализации источника данных, определяющих сервер, базу данных и использование для соединения режима проверки подлинности Windows. Имена сервера и базы данных получаются в параметрах *pLocation* и *pDatasource* функции.  
  
```  
HRESULT SetAndSaveInitProps  
    (  
    IDBInitialize* pIDBInitialize,  
    WCHAR* pDataSource,  
    WCHAR* pCatalog,  
    BOOL bUseWinNTAuth  
    )  
    {  
    const ULONG     nProps = 4;  
    ULONG           nSSProps;  
    ULONG           nPropSets;  
    ULONG           nProp;  
    IDBProperties*  pIDBProperties  = NULL;  
    IPersistFile*   pIPersistFile   = NULL;  
    DBPROP          aInitProps[nProps];  
    DBPROP*         aSSInitProps    = NULL;  
    DBPROPSET*      aInitPropSets   = NULL;  
    HRESULT         hr;  
  
        nSSProps = 0;  
        nPropSets = 1;  
  
    aInitPropSets = new DBPROPSET[nPropSets];  
  
    // Initialize common property options.  
    for (nProp = 0; nProp < nProps; nProp++)  
        {  
        VariantInit(&aInitProps[nProp].vValue);  
        aInitProps[nProp].dwOptions = DBPROPOPTIONS_REQUIRED;  
        aInitProps[nProp].colid = DB_NULLID;  
        }  
  
    // Level of prompting that will be done to complete the connection  
    // process.  
    aInitProps[0].dwPropertyID = DBPROP_INIT_PROMPT;  
    aInitProps[0].vValue.vt = VT_I2;  
    aInitProps[0].vValue.iVal = DBPROMPT_NOPROMPT;       
  
    // Server name.  
    aInitProps[1].dwPropertyID = DBPROP_INIT_DATASOURCE;      
    aInitProps[1].vValue.vt = VT_BSTR;  
    aInitProps[1].vValue.bstrVal = SysAllocString(pDataSource);  
  
    // Database.  
    aInitProps[2].dwPropertyID = DBPROP_INIT_CATALOG;  
    aInitProps[2].vValue.vt = VT_BSTR;  
    aInitProps[2].vValue.bstrVal = SysAllocString(pCatalog);  
  
    aInitProps[3].dwPropertyID = DBPROP_AUTH_INTEGRATED;  
    if (bUseWinNTAuth == TRUE)  
    {  
        aInitProps[3].vValue.vt = VT_BSTR;  
        aInitProps[3].vValue.bstrVal = SysAllocString(L"SSPI");  
    } //end if  
  
    // Now that properties are set, construct the PropertySet array.  
    aInitPropSets[0].guidPropertySet = DBPROPSET_DBINIT;  
    aInitPropSets[0].cProperties = nProps;  
    aInitPropSets[0].rgProperties = aInitProps;  
  
    // Set initialization properties  
    pIDBInitialize->QueryInterface(IID_IDBProperties,  
        (void**) &pIDBProperties);  
    hr = pIDBProperties->SetProperties(nPropSets, aInitPropSets);  
    if (FAILED(hr))  
        {  
        // Display error from failed SetProperties.  
        }  
    pIDBProperties->Release();  
  
    // Free references on OLE known strings.  
    for (nProp = 0; nProp < nProps; nProp++)  
        {  
        if (aInitProps[nProp].vValue.vt == VT_BSTR)  
            SysFreeString(aInitProps[nProp].vValue.bstrVal);  
        }  
  
    for (nProp = 0; nProp < nSSProps; nProp++)  
        {  
        if (aSSInitProps[nProp].vValue.vt == VT_BSTR)  
            SysFreeString(aSSInitProps[nProp].vValue.bstrVal);  
        }  
  
    // Free dynamically allocated memory.  
    delete [] aInitPropSets;  
    delete [] aSSInitProps;  
  
    // On success, persist the data source.  
    if (SUCCEEDED(hr))  
        {  
        pIDBInitialize->QueryInterface(IID_IPersistFile,  
            (void**) &pIPersistFile);  
  
        hr = pIPersistFile->Save(OLESTR("MyDataSource.txt"), FALSE);  
  
        if (FAILED(hr))  
            {  
            // Display errors from IPersistFile interface.  
            }  
        pIPersistFile->Release();  
        }  
  
    return (hr);  
    }  
```  
  
 **Б. Использование инициализации материализованных источников данных:**  
  
 Данный пример использует материализованный объект источника данных с дополнительными свойствами инициализации, который предоставляет имя входа и пароль [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```  
HRESULT InitFromPersistedDS  
    (  
    IDBInitialize* pIDBInitialize,  
    WCHAR* pPersistedDSN,  
    WCHAR* pUID,  
    WCHAR* pPWD  
    )  
    {  
    //const ULONG   nProps = 3;  
    const ULONG     nProps = 1;  
    const ULONG     nPropSets = 1;  
    ULONG           nProp;  
    IDBProperties*  pIDBProperties  = NULL;  
    IPersistFile*   pIPersistFile   = NULL;  
    DBPROP          aInitProps[nProps];  
    DBPROPSET       aInitPropSets[nPropSets];  
    HRESULT         hr;  
  
    // First load the persisted data source information.  
    pIDBInitialize->QueryInterface(IID_IPersistFile,  
        (void**) &pIPersistFile);  
  
    hr = pIPersistFile->Load(pPersistedDSN, STGM_DIRECT);  
  
    if (FAILED(hr))  
        {  
        // Display errors from IPersistFile interface.  
        }  
    pIPersistFile->Release();  
  
    if (FAILED(hr))  
        {  
        return (hr);  
        }  
  
    // Initialize common property options.  
    for (nProp = 0; nProp < nProps; nProp++)  
        {  
        VariantInit(&aInitProps[nProp].vValue);  
        aInitProps[nProp].dwOptions = DBPROPOPTIONS_REQUIRED;  
        aInitProps[nProp].colid = DB_NULLID;  
        }  
  
    // Level of prompting that will be done to complete the connection  
    // process.  
    aInitProps[0].dwPropertyID = DBPROP_INIT_PROMPT;  
    aInitProps[0].vValue.vt = VT_I2;  
    aInitProps[0].vValue.iVal = DBPROMPT_NOPROMPT;      
  
    // Now that properties are set, construct the PropertySet array.  
    aInitPropSets[0].guidPropertySet = DBPROPSET_DBINIT;  
    aInitPropSets[0].cProperties = nProps;  
    aInitPropSets[0].rgProperties = aInitProps;  
  
    // Set initialization properties  
    pIDBInitialize->QueryInterface(IID_IDBProperties,  
        (void**) &pIDBProperties);  
    hr = pIDBProperties->SetProperties(nPropSets, aInitPropSets);  
    if (SUCCEEDED(hr))  
        {  
        hr = pIDBInitialize->Initialize();  
        if (FAILED(hr))  
            {  
            DumpError(pIDBInitialize, IID_IDBInitialize);  
            }  
        }  
    else  
        {  
        // Display error from failed SetProperties.  
        }  
    pIDBProperties->Release();  
  
    // Free references on OLE known strings.  
    for (nProp = 0; nProp < nProps; nProp++)  
        {  
        if (aInitProps[nProp].vValue.vt == VT_BSTR)  
            SysFreeString(aInitProps[nProp].vValue.bstrVal);  
        }  
  
    return (hr);  
    }  
```  
  
 Метод **IPersistFile::Save** можно вызвать до или после вызова метода **IDBInitialize::Initialize**. Вызов метода после успешного возвращения из метода **IDBInitialize::Initialize** гарантирует сохранение допустимой спецификации источника данных.  
  
## <a name="see-also"></a>См. также:  
 [Объекты источников данных (OLE DB)](../../oledb/ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  

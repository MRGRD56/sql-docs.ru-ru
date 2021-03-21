---
description: Получение FAST_FORWARD курсора (поставщик собственного клиента OLE DB)
title: Получение FAST_FORWARD курсора (поставщик собственного клиента OLE DB)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- fast forward-only cursors
ms.assetid: 931a28c3-8ea1-45d6-9ca1-2b8388c4d8b0
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 988bc4139ad2479b946a25548c757bc71d6e06da
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753844"
---
# <a name="obtain-a-fast_forward-cursor-native-client-ole-db-provider"></a>Получение FAST_FORWARD курсора (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Чтобы получить курсор только для чтения с последовательным доступом, установите свойства набора строк DBPROP_SERVERCURSOR, DBPROP_OTHERINSERT, DBPROP_OTHERUPDATEDELETE, DBPROP_OWNINSERT и DBPROP_OWNUPDATEDELETE в значение VARIANT_TRUE.  
  
 Полный образец показывает, как настроить свойства набора строк для создания курсора FAST_FORWARD. После настройки свойств выполняется инструкция SELECT, получающая и отображающая столбец **Name** таблицы **Purchasing.Vendor** базы данных **AdventureWorks** .  
  
> [!IMPORTANT]  
>  По возможности используйте аутентификацию Windows. Если проверка подлинности Windows недоступна, запросите у пользователя ввод учетных данных во время выполнения. Избегайте хранения учетных данных в файле. Если необходимо сохранить учетные данные, зашифруйте их с помощью [API-интерфейса шифрования Win32](/windows/win32/seccrypto/cryptography-reference).  
  
### <a name="to-obtain-fast_forward-cursor"></a>Получение курсора FAST_FORWARD  
  
1.  Установите соединение с источником данных.  
  
2.  Установите свойства набора строк DBPROP_SERVERCURSOR, DBPROP_OTHERINSERT, DBPROP_OTHERUPDATEDELETE, DBPROP_OWNINSERT и DBPROP_OWNUPDATEDELETE в значение VARIANT_TRUE.  
  
3.  Выполните команду.  

## <a name="example"></a>Пример  
 В приведенном ниже примере показано, как настроить свойства набора строк для получения курсора FAST_FORWARD. После настройки свойств выполняется инструкция SELECT, получающая и отображающая столбец Name таблицы Purchasing.Vendor в базе данных AdventureWorks. Этот образец не поддерживается на архитектуре IA64.  
  
 Образцу требуется образец базы данных AdventureWorks, который можно загрузить с домашней страницы [Образцы кода и проекты сообщества Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkID=85384) (возможно, на английском языке).  
  
 Скомпилируйте с библиотеками ole32.lib и oleaut32.lib и выполните следующий листинг кода (C++). Это приложение соединяется с установленным на компьютер экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию. В некоторых операционных системах Windows придется заменить (localhost) или (local) на имя своего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Чтобы подключиться к именованному экземпляру, измените строку подключения с L"(local)" на L"(local)\\\<имя>", где <имя> — это именованный экземпляр. По умолчанию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express устанавливается на именованный экземпляр. Убедитесь, что переменная среды INCLUDE включает каталог, содержащий файл sqlncli.h.  
  
```  
// compile with: ole32.lib oleaut32.lib  
#define INITGUID  
#define DBINITCONSTANTS  
#define OLEDBVER 0x0250   // to include correct interfaces  
  
#include <windows.h>  
#include <stdio.h>  
#include <oledb.h>  
#include <sqlncli.h>  
#include <oledberr.h>  
  
IDBInitialize* pIDBInitialize = NULL;  
ICommandText* pICommandText = NULL;  
  
// Connect to the server and create a command object.  
int InitializeAndConnect();  
  
// Set the properties to get a FAST_FORWARD cursor.  
int SetRowsetProperties();  
  
// This function executes a command and displays the results.  
int ExecuteAndDisplay();  
  
// Release memory.  
void Cleanup();  
  
int main() {  
   if (InitializeAndConnect() == -1) {  
      // Handle error.  
      printf("Failed to initialize and connect to the server.\n");  
      return -1;  
   }  
  
   // Set the row properties to FAST_FORWARD cursor.  
   if (SetRowsetProperties() == -1) {  
      // Handle error.  
      printf("Failed to set the rowset properties.\n");  
      return -1;  
   }  
  
   // Execute a command and display the results.  
   if (ExecuteAndDisplay() == -1) {  
      // Handle error.  
      printf("Failed to execute a command and display the results.\n");  
      return -1;  
   }  
  
   Cleanup();  
}  
  
int InitializeAndConnect() {  
   HRESULT hr = S_OK;  
  
   IDBProperties* pIDBProperties = NULL;  
   IDBCreateSession* pIDBCreateSession = NULL;  
   IDBCreateCommand* pIDBCreateCommand = NULL;  
  
   DBPROPSET dbPropSet;  
   DBPROP dbProp[4];  
   int iRetVal = 0;  
  
   // Initialize OLE  
   if ( FAILED( hr = OleInitialize( NULL ) ) ) {  
      // Handle errors here.  
      return -1;  
   }  
  
   // Create an instance of Microsoft SQL Server Native Client OLE DB Provider.  
   if ( FAILED( hr =   
      CoCreateInstance(CLSID_SQLNCLI11, NULL, CLSCTX_INPROC_SERVER, IID_IDBProperties, (void **) &pIDBProperties ))) {  
      // Handle errors here.  
      return -1;  
   }  
  
   // Set up the connection properties.  
   dbProp[0].dwPropertyID = DBPROP_INIT_DATASOURCE;  
   dbProp[0].dwOptions = DBPROPOPTIONS_REQUIRED;  
   dbProp[0].colid = DB_NULLID;  
   V_VT(&(dbProp[0].vValue)) = VT_BSTR;  
  
   V_BSTR(&(dbProp[0].vValue)) = SysAllocString( L"(local)" );  
  
   dbProp[1].dwPropertyID = DBPROP_AUTH_INTEGRATED;  
   dbProp[1].dwOptions = DBPROPOPTIONS_REQUIRED;  
   dbProp[1].colid = DB_NULLID;  
   V_VT(&(dbProp[1].vValue)) = VT_BSTR;  
   V_BSTR(&(dbProp[1].vValue)) = SysAllocString( L"SSPI" );  
  
   dbProp[2].dwPropertyID = DBPROP_NULLCOLLATION;  
   dbProp[2].dwOptions = DBPROPOPTIONS_REQUIRED;  
   dbProp[2].colid = DB_NULLID;  
   V_VT(&(dbProp[2].vValue)) = VT_BSTR;  
   V_BSTR(&(dbProp[2].vValue)) = SysAllocString( L"" );  
  
   dbProp[3].dwPropertyID = DBPROP_INIT_CATALOG;  
   dbProp[3].dwOptions = DBPROPOPTIONS_REQUIRED;  
   dbProp[3].colid = DB_NULLID;  
   V_VT(&(dbProp[3].vValue)) = VT_BSTR;  
   V_BSTR(&(dbProp[3].vValue)) = SysAllocString( L"AdventureWorks" );  
  
   dbPropSet.rgProperties = dbProp;  
   dbPropSet.cProperties = 4;  
   dbPropSet.guidPropertySet = DBPROPSET_DBINIT;  
  
   if ( FAILED( hr = pIDBProperties->SetProperties( 1, &dbPropSet ))) {  
      // Handle errors here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   SysFreeString( V_BSTR(&(dbProp[0].vValue)) );  
   SysFreeString( V_BSTR(&(dbProp[1].vValue)) );  
   SysFreeString( V_BSTR(&(dbProp[2].vValue)) );  
   SysFreeString( V_BSTR(&(dbProp[3].vValue)) );  
  
   // Get an IDBInitialize interface.  
   if ( FAILED( hr =   
      pIDBProperties->QueryInterface( IID_IDBInitialize, (void **) &pIDBInitialize ))) {  
      // Handle errors here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   // Call Initialize.  
   if ( FAILED( hr = pIDBInitialize->Initialize())) {  
      // Handle errors here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   // Get a IDBCreateSession interface.  
   if ( FAILED( hr =   
      pIDBInitialize->QueryInterface( IID_IDBCreateSession, (void **) &pIDBCreateSession ))) {  
      // Handle errors here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   // Create a session  
   if ( FAILED( hr =   
      pIDBCreateSession->CreateSession( NULL, IID_IDBCreateCommand, (IUnknown **) &pIDBCreateCommand))) {  
      // Handle errors here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   // Create a command.  
   if ( FAILED( hr =   
      pIDBCreateCommand->CreateCommand( NULL, IID_ICommandText, (IUnknown **) &pICommandText))) {  
      // Handle errors here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
CLEANUP:  
   // Release all the objects not needed anymore.  
   pIDBProperties->Release();  
   if ( pIDBCreateSession )  
      pIDBCreateSession->Release();  
   if ( pIDBCreateCommand )  
      pIDBCreateCommand->Release();  
  
   return iRetVal;  
}  
  
int SetRowsetProperties() {  
   HRESULT hr = S_OK;  
   ICommandProperties* pICommandProperties = NULL;  
   DBPROPSET dbPropSet;  
   DBPROP dbProp[5];  
   int iRetVal = 0;  
  
   // Get an ICommandProperties object.  
   if ( FAILED( hr =   
      pICommandText->QueryInterface( IID_ICommandProperties, (void **) &pICommandProperties ))) {  
      // Handle errors here.  
      return -1;  
   }  
  
   // Set up the properties to get a FAST_FORWARD cursor.  
   dbProp[0].dwPropertyID       = DBPROP_SERVERCURSOR;  
   dbProp[0].dwOptions          = DBPROPOPTIONS_REQUIRED;  
   dbProp[0].colid              = DB_NULLID;  
   V_VT(&(dbProp[0].vValue))    = VT_BOOL;  
   V_BOOL(&(dbProp[0].vValue))  = VARIANT_TRUE;  
  
   dbProp[1].dwPropertyID       = DBPROP_OTHERINSERT;  
   dbProp[1].dwOptions          = DBPROPOPTIONS_REQUIRED;  
   dbProp[1].colid              = DB_NULLID;  
   V_VT(&(dbProp[1].vValue))    = VT_BOOL;  
   V_BOOL(&(dbProp[1].vValue))  = VARIANT_TRUE;  
  
   dbProp[2].dwPropertyID       = DBPROP_OTHERUPDATEDELETE;  
   dbProp[2].dwOptions          = DBPROPOPTIONS_REQUIRED;  
   dbProp[2].colid              = DB_NULLID;  
   V_VT(&(dbProp[2].vValue))    = VT_BOOL;  
   V_BOOL(&(dbProp[2].vValue))  = VARIANT_TRUE;  
  
   dbProp[3].dwPropertyID       = DBPROP_OWNINSERT;  
   dbProp[3].dwOptions          = DBPROPOPTIONS_REQUIRED;  
   dbProp[3].colid              = DB_NULLID;  
   V_VT(&(dbProp[3].vValue))    = VT_BOOL;  
   V_BOOL(&(dbProp[3].vValue))  = VARIANT_TRUE;  
  
   dbProp[4].dwPropertyID       = DBPROP_OWNUPDATEDELETE;  
   dbProp[4].dwOptions          = DBPROPOPTIONS_REQUIRED;  
   dbProp[4].colid              = DB_NULLID;  
   V_VT(&(dbProp[4].vValue))    = VT_BOOL;  
   V_BOOL(&(dbProp[4].vValue))  = VARIANT_TRUE;  
  
   dbPropSet.rgProperties       = dbProp;  
   dbPropSet.cProperties        = 5;  
   dbPropSet.guidPropertySet    = DBPROPSET_ROWSET;  
  
   if ( FAILED( hr = pICommandProperties->SetProperties( 1, &dbPropSet))) {  
      // Handle errors here.  
      iRetVal = -1;  
   }  
  
   // Release the ICommandProperties object.  
   pICommandProperties->Release();  
  
   return iRetVal;  
}  
  
int ExecuteAndDisplay() {  
   HRESULT hr = S_OK;  
   IRowset* pIRowset = NULL;  
   IAccessor* pIAccessor = NULL;  
  
   BYTE* pData = NULL;  
   DBCOUNTITEM cRowsObtained = 0;  
   ULONG cCount = 0;  
  
   HROW* pRows = new HROW[10];  
   HACCESSOR hAccessor = 0;  
   DBBINDING Bind[1];  
   int iRetVal = 0;  
  
   if (!pRows)  
      return -1;  
  
   // Set the command text.  
   if ( FAILED( hr = pICommandText->SetCommandText( DBGUID_SQL, L"select Name from Purchasing.Vendor")))  
      // Handle errors and free the memory here.  
      return -1;  
  
   // Execute the command.  
   if ( FAILED( hr = pICommandText->Execute( NULL, IID_IRowset, NULL, NULL, (IUnknown **) &pIRowset )))  
      // Handle errors and free the memory here.  
      return -1;  
  
    // Set up the binding structure for Name (nvarchar(50)).  
    Bind[0].dwPart      = DBPART_VALUE;  
    Bind[0].eParamIO    = DBPARAMIO_NOTPARAM;  
    Bind[0].iOrdinal    = 1;  
    Bind[0].pTypeInfo   = NULL;  
    Bind[0].pObject     = NULL;  
    Bind[0].pBindExt    = NULL;  
    Bind[0].dwFlags     = 0;  
    Bind[0].dwMemOwner  = DBMEMOWNER_CLIENTOWNED;  
    Bind[0].obLength    = 0;  
    Bind[0].obStatus    = 0;  
    Bind[0].obValue     = 0;  
    Bind[0].cbMaxLen    = 102;  
    Bind[0].wType       = DBTYPE_WSTR;  
    Bind[0].bPrecision  = 0;  
    Bind[0].bScale      = 0;  
  
   // Get an IAccessor interface.  
   if ( FAILED( hr = pIRowset->QueryInterface( IID_IAccessor, (void **) &pIAccessor))) {  
      // Handle errors and free the memory here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   // Create an accessor.  
   if ( FAILED( hr =   
      pIAccessor->CreateAccessor(  DBACCESSOR_ROWDATA, 1, Bind, 0, &hAccessor, NULL))) {  
      // Handle errors and free the memory here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   // Allocate memory for the data.  
   pData = new BYTE[102];  
   if (!(pData /* = new BYTE[102] */ )) {  
      // Handle errors and free the memory here.  
      iRetVal = -1;  
      goto CLEANUP;  
   }  
  
   // Loop through all of the rows.  
   for ( ; ; ) {  
      if (FAILED( hr = pIRowset->GetNextRows( NULL, 0, 10, &cRowsObtained, &pRows))) {  
         // Handle errors and free the memory here.  
         iRetVal = -1;  
         goto CLEANUP;  
      }  
  
      // Make sure some rows were obtained.  
      if (cRowsObtained == 0)  
         break;  
  
      // Get the data for the each of the rows.  
      for ( cCount = 0 ; cCount < cRowsObtained ; cCount++ ) {  
         // Get the row data needed.  
         if ( FAILED( hr = pIRowset->GetData( pRows[cCount], hAccessor, pData ))) {  
            // Handle errors and free the memory here.  
            iRetVal = -1;  
            goto CLEANUP;  
         }  
  
         // Display row data.  
         printf( "%S\n", pData);  
      }  
  
      // Release the rows.  
      if ( FAILED( hr = pIRowset->ReleaseRows(cRowsObtained, pRows, NULL, NULL, NULL ))) {  
         // Handle errors and free the memory here.  
         iRetVal = -1;  
         goto CLEANUP;  
      }  
   }  
  
CLEANUP:  
   // Release memory allocated for the data.  
   delete [] pRows;  
   delete [] pData;  
  
   // Release the HACCESSOR.  
   if (pIAccessor) {  
      pIAccessor->ReleaseAccessor( hAccessor, NULL );  
  
      // Release the IAccessor object.  
      pIAccessor->Release();  
   }  
  
   // Release the rowset.  
   pIRowset->Release();  
  
   return iRetVal;  
}  
  
void Cleanup() {  
   HRESULT hr = S_OK;  
  
   // Release the ICommandText object.  
   pICommandText->Release();  
  
   // Uninitialize the IDBInitialize object.  
   if ( FAILED( hr = pIDBInitialize->Uninitialize())) {  
      // Handle errors here.  
   }  
  
   // Release the IDBInitialize object.  
   pIDBInitialize->Release();  
  
   // Uninitialize OLE.  
   OleUninitialize();  
}  
```  
  

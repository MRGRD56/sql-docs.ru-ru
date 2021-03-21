---
title: ISSCommandWithParameters::GetParameterProperties (OLE DB) | Документация Майкрософт
description: ISSCommandWithParameters::GetParameterProperties возвращает массив структур наборов свойств в OLE DB Driver for SQL Server, по одному для каждого определяемого пользователем типа или параметра XML.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters::GetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- GetParameterProperties method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a982287a57dd033021bd806fc0c9aaba98212439
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753164"
---
# <a name="isscommandwithparametersgetparameterproperties-ole-db"></a>ISSCommandWithParameters::GetParameterProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Возвращает массив структур SSPARAMPROPS, представляющих собой множества свойств, по одному множеству свойств SSPARAMPROPS на каждый параметр определяемого пользователем типа или XML.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
HRESULT GetParameterProperties(  
      DB_UPARAMS *pcParams,  
      SSPARAMPROPS **prgParamProperties);  
```  
  
## <a name="arguments"></a>Аргументы  
 *pcParams*[out][in]  
 Указатель на область памяти, где содержится количество структур SSPARAMPROPS, возвращаемых в параметре *prgParamProperties*.  
  
 *prgParamProperties*[out]  
 Указатель на область памяти, в которую будет возвращен массив структур SSPARAMPROPS. Поставщик выделяет память под структуры и возвращает адрес этой памяти. Когда структуры больше не нужны потребителю, он освобождает память вызовом метода `IMalloc::Free`. До вызова метода `IMalloc::Free` для объекта *prgParamProperties* потребитель должен также вызвать метод `VariantClear` для свойства *vValue* каждой структуры DBPROP, чтобы предотвратить утечку памяти в случае, если вариант содержит ссылочный тип (например, BSTR). Если указатель *pcParams* равен нулю на выходе или происходит любая ошибка, отличная от DB_E_ERRORSOCCURRED, поставщик не выделяет память и обеспечивает равенство указателя *prgParamProperties* NULL на выходе.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Метод `GetParameterProperties` возвращает те же коды ошибок, что и метод `ICommandProperties::GetProperties` основного OLE DB, за исключением того, что DB_S_ERRORSOCCURRED и DB_E_ERRORSOCCURED не могут быть вызваны.  
  
## <a name="remarks"></a>Remarks  
 Метод `ISSCommandWithParameters::GetParameterProperties` действует постоянно с учетом `GetParameterInfo`. Если [`ISSCommandWithParameters::SetParameterProperties`](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) или `SetParameterInfo` не были вызваны или вызваны с cParams, равными нулю, `GetParameterInfo` наследует сведения о параметрах и возвращает их. Если `ISSCommandWithParameters::SetParameterProperties` или `SetParameterInfo` были вызваны по крайней мере для одного параметра, метод `ISSCommandWithParameters::GetParameterProperties` возвращает свойства только для тех параметров, для которых был вызван `ISSCommandWithParameters::SetParameterProperties`. Если `ISSCommandWithParameters::SetParameterProperties` вызывается после `ISSCommandWithParameters::GetParameterProperties` или `GetParameterInfo`, последующие вызовы `ISSCommandWithParameters::GetParameterProperties` возвращают переопределенные значения для этих параметров, для которых был вызван метод `ISSCommandWithParameters::SetParameterProperties`.  
  
 Структура SSPARAMPROPS определена следующим образом.  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
|Участник|Description|  
|------------|-----------------|  
|*iOrdinal*|Порядковый номер переданного параметра.|  
|*cPropertySets*|Количество структур DBPROPSET в *rgPropertySets*.|  
|*rgPropertySets*|Указатель на буфер, в который будет возвращен массив структур DBPROPSET.|  
  
## <a name="see-also"></a>См. также:  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  

---
description: 'ISSCommandWithParameters:: SetParameterProperties в SQL Server Native Client (OLE DB)'
title: ISSCommandWithParameters::SetParameterProperties (OLE DB)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSCommandWithParameters::SetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- SetParameterProperties method
ms.assetid: 4cd0281a-a2a0-43df-8e46-eb478b64cb4b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95bb26f6e528f94b99d3e3bff2c6da0d2d4eb85e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753704"
---
# <a name="isscommandwithparameterssetparameterproperties-in-sql-server-native-client-ole-db"></a>ISSCommandWithParameters:: SetParameterProperties в SQL Server Native Client (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Задает свойства каждого параметра по порядковому номеру или задает массовые свойства параметра, указывая массив структур SSPARAMPROPS.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp
HRESULT SetParameterProperties(  
      DB_UPARAMS cParams,   
      SSPARAMPROPS rgParamProperties[]);  
```  
  
## <a name="arguments"></a>Аргументы  
 *cParams*[in]  
 Количество структур SSPARAMPROPS в массиве *rgParamProperties*. Если это число равно 0, то метод **ISSCommandWithParameters::SetParameterProperties** удалит все свойства, которые могли быть указаны для всех параметров команды.  
  
 *rgParamProperties*[in]  
 Массив задаваемых структур SSPARAMPROPS.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Метод **ISSCommandWithParameters::SetParameterProperties** возвращает те же коды ошибок, что и базовый метод OLE DB **ICommandProperties::SetProperties**.  
  
## <a name="remarks"></a>Remarks  
 С помощью этого метода разрешается задавать свойства для каждого параметра по порядковому номеру параметра или путем одного вызова метода **ISSCommandWithParameters::SetParameterProperties**, если структура SSPARAMPROPS строится из массива свойств.  
  
 Метод **SetParameterInfo** необходимо вызывать до вызова метода **ISSCommandWithParameters::SetParameterProperties**. Вызов метода `SetParameterProperties(0, NULL)` очищает все указанные свойства параметра, тогда как вызов метода `SetParameterInfo(0,NULL,NULL)` очищает все сведения о параметре, в том числе все свойства, которые могут быть связаны с параметром.  
  
 Вызов **ISSCommandWithParameters:: SetParameterProperties** для указания свойств параметра, который не относится к типу DBTYPE_XML или DBTYPE_UDT возвращает DB_E_ERRORSOCCURRED или DB_S_ERRORSOCCURRED и помечает поле *dwstatus устанавливается* всех дбпропс, содержащихся в SSPARAMPROPS, для этого параметра DBPROPSTATUS_NOTSET. Чтобы обнаружить, к какому параметру относится DB_E_ERRORSOCCURRED или DB_S_ERRORSOCCURRED, необходимо пройти по массиву DBPROP каждого набора DBPROPSET, содержащегося в структуре SSPARAMPROPS.  
  
 Если метод **ISSCommandWithParameters::SetParameterProperties** вызывается, чтобы задать свойства параметров, сведения о которых еще не были заданы с помощью метода **SetParameterInfo**, то поставщик возвратит E_UNEXPECTED со следующим сообщением об ошибке:  
  
 Невозможно вызвать метод SetParameterProperties для указанных параметров, не вызывая предварительно метод SetParameterInfo. До задания свойств параметров необходимо задать сведения о параметрах.  
  
 Если вызов метода **ISSCommandWithParameters::SetParameterProperties** содержит некоторые параметры с заданными сведениями и некоторые параметры с незаданными сведениями, то свойства dwStatus в структуре DBPROPSET набора свойств SSPARAMPROPS будут возвращены со значением DBSTATUS_NOTSET.  
  
 Структура SSPARAMPROPS определена следующим образом.  

```cpp
struct SSPARAMPROPS {
    DBORDINAL iOrdinal;
    ULONG cPropertySets;
    DBPROPSET *rgPropertySets;
};
```

 Улучшения ядра СУБД, появившиеся с версии [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], позволяют методу ISSCommandWithParameters::SetParameterProperties получать более точные описания ожидаемых результатов. Эти более точные результаты могут отличаться от значений, которые метод ISSCommandWithParameters::SetParameterProperties возвращает в предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделе [Обнаружение метаданных](../../relational-databases/native-client/features/metadata-discovery.md).  
  
|Участник|Description|  
|------------|-----------------|  
|*iOrdinal*|Порядковый номер переданного параметра.|  
|*cPropertySets*|Количество структур DBPROPSET в *rgPropertySets*.|  
|*rgPropertySets*|Указатель на буфер, в который будет возвращен массив структур DBPROPSET.|  
|||

## <a name="see-also"></a>См. также:  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  

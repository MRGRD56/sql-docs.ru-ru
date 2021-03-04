---
title: ISSDataClassification::GetSensitivityClassification | Документация Майкрософт
description: ISSDataClassification::GetSensitivityClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification::GetSensitivityClassification
apitype: COM
helpviewer_keywords:
- GetSensitivityClassification method
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 232bcf6187967e9dab7b09414626785d7808e18f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837331"
---
# <a name="issdataclassificationgetsensitivityclassification"></a>ISSDataClassification::GetSensitivityClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Отвечает за извлечение данных классификации чувствительности для активного набора строк. Дополнительные сведения и пример кода см. в статье [Использование классификации данных](../features/using-data-classification.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
HRESULT GetSensitivityClassification(
    SENSITIVITYCLASSIFICATION** ppSensitivityClassification);
```  
  
## <a name="arguments"></a>Аргументы  
  *ppSensitivityClassification*[out]  
 Указатель на указатель структуры SENSITIVITYCLASSIFICATION. Если этот метод не дает результатов или отсутствуют сведения о классификации данных, поставщик не выделяет память и гарантирует, что аргумент ppSensitivityClassification при выводе является пустым указателем.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 S_OK  
 Метод выполнен успешно.    
  
 E_INVALIDARG  
 Аргумент *ppSensitivityClassification* имел значение NULL.  
  
 E_OUTOFMEMORY  
 OLE DB Driver for SQL Server не удалось выделить достаточный объем памяти для завершения запроса.  

  
## <a name="remarks"></a>Remarks  
OLE DB Driver for SQL Server выделяет блок памяти для хранения структуры SENSITIVITYCLASSIFICATION и данных, на которые ссылается эта структура. Когда объекту-получателю больше не нужен доступ к данным классификации, он должен освободить эту память, вызвав метод [IMalloc::Free](/windows/win32/api/objidl/nf-objidl-imalloc-free).  
  
 Структура SENSITIVITYCLASSIFICATION определяется следующим образом:
  
```cpp
typedef struct tagSensitivityClassification
{
    USHORT                     cSensitivityLabels;
    SENSITIVITYLABEL          *rgSensitivityLabels;
    USHORT                     cInformationTypes;
    INFORMATIONTYPE           *rgInformationTypes;
    USHORT                     cColumnSensitivityMetadata;
    COLUMNSENSITIVITYMETADATA *rgColumnSensitivityMetadata;
    SENSITIVITYRANKENUM        eQuerySensitivityRank;
} SENSITIVITYCLASSIFICATION;
```  

|Член|Описание|  
|------------|-----------------|  
|*cSensitivityLabels*|Число структур SENSITIVITYLABEL в *rgSensitivityLabels*.|  
|*rgSensitivityLabels*|Массив структур SENSITIVITYLABEL.|  
|*cInformationTypes*|Число структур INFORMATIONTYPE в *rgInformationTypes*.|  
|*rgInformationTypes*|Массив структур INFORMATIONTYPE.|  
|*cColumnSensitivityMetadata*|Число структур COLUMNSENSITIVITYMETADATA в *rgColumnSensitivityMetadata*.|  
|*rgColumnSensitivityMetadata*|Массив структур COLUMNSENSITIVITYMETADATA.|  
|*eQuerySensitivityRank*|Относительный рейтинг чувствительности запроса, который был выполнен для получения набора строк.|  

Структура SENSITIVITYLABEL определяется следующим образом:
```cpp
typedef struct tagSENSITIVITYLABEL
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} SENSITIVITYLABEL;
```

|Член|Описание|  
|------------|-----------------|  
|*pwszName*|Имя метки конфиденциальности.|  
|*pwszId*|Идентификатор метки конфиденциальности.|  

Структура INFORMATIONTYPE определена следующим образом:
```cpp
typedef struct tagINFORMATIONTYPE
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} INFORMATIONTYPE;
```

|Член|Описание|  
|------------|-----------------|  
|*pwszName*|Имя типа сведений.|  
|*pwszId*|Идентификатор типа сведений.|  

Структура COLUMNSENSITIVITYMETADATA определяется следующим образом:
```cpp
typedef struct tagCOLUMNSENSITIVITYMETADATA
{
    SENSITIVITYPROPERTY* rgSensitivityProperties;
    USHORT cSensitivityProperties;
} COLUMNSENSITIVITYMETADATA;
```

|Член|Описание|  
|------------|-----------------|  
|*cSensitivityProperties*|Число структур SENSITIVITYPROPERTY в *rgSensitivityProperties*.|  
|*rgSensitivityProperties*|Массив структур SENSITIVITYPROPERTY.|  

Перечисление SENSITIVITYRANKENUM определяется следующим образом:
```cpp
typedef enum tagSENSITIVITYRANKENUM
{
    SENSITIVITYRANK_NOT_DEFINED = -1,
    SENSITIVITYRANK_NONE = 0,
    SENSITIVITYRANK_LOW = 10,
    SENSITIVITYRANK_MEDIUM = 20,
    SENSITIVITYRANK_HIGH = 30,
    SENSITIVITYRANK_CRITICAL = 40
} SENSITIVITYRANKENUM;
```

Структура SENSITIVITYPROPERTY определяется следующим образом:
```cpp
typedef struct tagSENSITIVITYPROPERTY
{
    SENSITIVITYLABEL* pSensitivityLabel;
    INFORMATIONTYPE* pInformationType;
    SENSITIVITYRANKENUM eSensitivityRank;
} SENSITIVITYPROPERTY;
```

|Член|Описание|  
|------------|-----------------|  
|*pSensitivityLabel*|Указатель на структуру SENSITIVITYLABEL.|  
|*pInformationType*|Указатель на структуру INFORMATIONTYPE.|  
|*eSensitivityRank*|Относительный рейтинг чувствительности столбца в рамках данных о каждом столбце.|  

## <a name="see-also"></a>См. также  
 [ISSDataClassification](../../oledb/ole-db-interfaces/issdataclassification-ole-db.md)  
 [Наборы строк](../ole-db-rowsets/rowsets.md)  

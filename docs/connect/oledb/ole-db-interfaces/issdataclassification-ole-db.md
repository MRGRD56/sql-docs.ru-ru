---
title: ISSDataClassification | Документация Майкрософт
description: Интерфейс ISSDataClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification
apitype: COM
helpviewer_keywords:
- ISSDataClassification interface
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 8ff73fec5480e12c401f44e22325d433f421c69e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753104"
---
# <a name="issdataclassification"></a>ISSDataClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Интерфейс **ISSDataClassification** предоставляет функции, позволяющие получить сведения о классификации конфиденциальных данных для активного набора строк, полученного от SQL Server.
  

## <a name="methods"></a>Методы

|Метод|Описание|  
|------------|-----------------|  
|[ISSDataClassification::GetSensitivityClassification](../../oledb/ole-db-interfaces/issdataclassification-getsensitivityclassification-ole-db.md)|Возвращает указатель на структуру SENSITIVITYCLASSIFICATION, содержащую сведения о классификации конфиденциальных данных.|  

## <a name="see-also"></a>См. также:  
 [Интерфейсы (OLE DB)](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)   
 [Наборы строк](../ole-db-rowsets/rowsets.md)   
 [Использование классификации данных](../features/using-data-classification.md)

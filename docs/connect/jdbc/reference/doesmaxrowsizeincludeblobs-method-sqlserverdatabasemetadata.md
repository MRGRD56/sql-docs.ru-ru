---
description: Метод doesMaxRowSizeIncludeBlobs (SQLServerDatabaseMetaData)
title: Метод doesMaxRowSizeIncludeBlobs (SQLServerDatabaseMetaData) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.doesMaxRowSizeIncludeBlobs
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0c90a7a7-5a59-4858-bb26-3e725d8611d7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b40e199531e6f2f5e5abd210ff558b316221989b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163440"
---
# <a name="doesmaxrowsizeincludeblobs-method-sqlserverdatabasemetadata"></a>Метод doesMaxRowSizeIncludeBlobs (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Извлекает значение, определяющее, включает ли возвращаемое значение метода [getMaxRowSize](../../../connect/jdbc/reference/getmaxrowsize-method-sqlserverdatabasemetadata.md) типы данных SQL LONGVARCHAR и LONGVARBINARY.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public boolean doesMaxRowSizeIncludeBlobs()  
```  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **true**, если возвращаемое значение включает типы данных. В противном случае — **false**.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод doesMoxRowSizeIncludeBlobs задается с помощью метода doesMoxRowSizeIncludeBlobs в интерфейсе java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Элементы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Класс SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  

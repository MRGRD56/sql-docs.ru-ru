---
description: Метод nullsAreSortedAtStart (SQLServerDatabaseMetaData)
title: Метод nullsAreSortedAtStart (SQLServerDatabaseMetaData) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDatabaseMetaData.nullsAreSortedAtStart Method (SQLServerDatabaseMetaData)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 372515da-3b0e-46f6-8c0b-01b1b45c5a2f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 50d26d02f4af38ec181c5c88a987475398a33dff
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433156"
---
# <a name="nullsaresortedatstart-method-sqlserverdatabasemetadata"></a>Метод nullsAreSortedAtStart (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает значение, определяющее, размещаются ли значения NULL в начале независимо от порядка сортировки.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public boolean nullsAreSortedAtStart()  
```  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **true**, если значения NULL при сортировке размещаются в начале. В противном случае — **false**.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод nullsAreSortedAtStart задается с помощью метода nullsAreSortedAtStart в интерфейсе java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Элементы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Класс SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  

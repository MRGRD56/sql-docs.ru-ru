---
description: Метод getMaxColumnsInSelect (SQLServerDatabaseMetaData)
title: Метод getMaxColumnsInSelect (SQLServerDatabaseMetaData) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxColumnsInSelect
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 43c428df-ef91-4f55-81c3-49a4db3379cc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d7a659f7e9b0d2a1968ca8df5f3b330e5169555b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162832"
---
# <a name="getmaxcolumnsinselect-method-sqlserverdatabasemetadata"></a>Метод getMaxColumnsInSelect (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает максимальное число столбцов, допустимое в списке SELECT для этой базы данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public int getMaxColumnsInSelect()  
```  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **int**, указывающее максимально допустимое количество столбцов.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод getMaxColumnsInSelect задается с помощью метода getMaxColumnsInSelect в интерфейсе java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Элементы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Класс SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  

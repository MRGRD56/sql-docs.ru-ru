---
description: Метод getRow (SQLServerResultSet)
title: Метод getRow (SQLServerResultSet) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: a266e3bc-05c2-44e2-9346-125ae6780216
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 191cfad8b1610d3fb379b3893f478a849aa4f540
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175129"
---
# <a name="getrow-method-sqlserverresultset"></a>Метод getRow (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Извлекает номер текущей строки.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public int getRow()  
```  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **int**, указывающее номер текущей строки, или 0, если строка отсутствует.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод getRow определен с помощью метода getRow в интерфейсе java.sql.ResultSet.  
  
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Класс SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  

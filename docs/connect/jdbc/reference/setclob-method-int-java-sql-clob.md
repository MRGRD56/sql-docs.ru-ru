---
description: Метод setClob (int, java.sql.Clob)
title: Метод setClob (int, java.sql.Clob) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.setClob
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 68d49f2c-fd8d-4abb-bfdc-e7b0fbd9a9da
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c273c32f430fcdd67f0b76e1c387ec3ec85790a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179104"
---
# <a name="setclob-method-int-javasqlclob"></a>Метод setClob (int, java.sql.Clob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Задает назначенному параметру значение указанного CLOB-объекта.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public final void setClob(int parameterIndex,  
                          java.sql.Clob clobValue)  
```  
  
#### <a name="parameters"></a>Параметры  
 *parameterIndex*  
  
 Значение **int**, определяющее номер параметра.  
  
 *clobValue*  
  
 Объект Clob.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Метод setClob определен с помощью метода setClob в интерфейсе java.sql.PreparedStatement.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-methods.md)   
 [Класс SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  

---
description: Метод getSubString (SQLServerClob)
title: Метод getSubString (SQLServerClob) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.getSubString
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bf915590-a883-4403-befa-5b5bb42f34d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f08b8515dd827b2202d3e373242bb292c5060f72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162185"
---
# <a name="getsubstring-method-sqlserverclob"></a>Метод getSubString (SQLServerClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает копию указанной подстроки в объекте CLOB по заданной начальной позиции и числу символов для копирования.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.lang.String getSubString(long pos,  
                                     int length)  
```  
  
#### <a name="parameters"></a>Параметры  
 *pos*  
  
 Первый символ извлекаемой подстроки. Первый символ находится в позиции 1.  
  
 *length*  
  
 Количество копируемых последовательных символов.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **String**, указывающее подстроку в объекте CLOB.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод getSubString задается с помощью метода getSubString в интерфейсе java.sql.Clob.  
  
 При попытке получить ноль символов из объекта CLOB со значением NULL или с нулевой длиной возвращается пустая строка. При попытке получить символы любой длины, начиная с позиции, отличной от 1, в объекте CLOB нулевой длины вызывается исключение позиции.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Элементы SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Класс SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  

---
description: Метод setAsciiStream (SQLServerClob)
title: Метод setAsciiStream (SQLServerClob) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerClob.setAsciiStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6e1779df-3b2a-41d1-8dca-99692cc9da14
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 80438309cf2e8ee2368762197b0617b2443bc42d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88432586"
---
# <a name="setasciistream-method-sqlserverclob"></a>Метод setAsciiStream (SQLServerClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает поток для записи символов ASCII в объект CLOB, начиная с указанной позиции.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.io.OutputStream setAsciiStream(long pos)  
```  
  
#### <a name="parameters"></a>Параметры  
 *pos*  
  
 Позиция, с которой начинается запись в объект CLOB.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Поток, в который можно записывать символы в кодировке ASCII.  
  
## <a name="exceptions"></a>Исключения  
 java.sql.SQLException  
  
## <a name="remarks"></a>Remarks  
 Этот метод setAsciiStream задается с помощью метода setAsciiStream в интерфейсе java.sql.Clob.  
  
 Символьные данные в объекте CLOB перезаписываются выходным потоком, начиная с заданной позиции, и могут превышать исходную длину объекта CLOB. Если указать значение позиции+1, будут добавлены символы ASCII. Если указать значение позиции+2 и более (либо нулевое или отрицательное значение), то создается ошибка позиции.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Элементы SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Класс SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  

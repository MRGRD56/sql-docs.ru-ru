---
description: Метод getDateTimeOffset (int)
title: Метод getDateTimeOffset (int) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 8bb00356-4d6e-4625-b924-67646930fdf2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4e1c1132c0fd7d788636cedb8cdf5eb9f2ea55f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163197"
---
# <a name="getdatetimeoffset-method-int"></a>Метод getDateTimeOffset (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Этот метод добавлен в [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 Получает значение заданного параметра в виде объекта [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md) на языке программирования Java по заданному индексу параметра.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public microsoft.sql.DateTimeOffset getDateTimeOffset(int index)  
```  
  
#### <a name="parameters"></a>Параметры  
 *index*  
  
 Начинающийся с единицы порядковый номер параметра.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Объект [класса DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Значение параметра [класса DateTimeOffset](../../../connect/jdbc/reference/datetimeoffset-class.md) можно задать с помощью метода [SQLServerCallableStatement.setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlservercallablestatement.md).  
  
## <a name="see-also"></a>См. также:  
 [Метод getDateTimeOffset (SQLServerCallableStatement)](../../../connect/jdbc/reference/getdatetimeoffset-method-sqlservercallablestatement.md)   
 [Элементы SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Класс SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  

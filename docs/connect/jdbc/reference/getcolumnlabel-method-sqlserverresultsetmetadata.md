---
description: Метод getColumnLabel (SQLServerResultSetMetaData)
title: Метод getColumnLabel (SQLServerResultSetMetaData) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSetMetaData.getColumnLabel
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: cf67692c-24aa-49e6-8e88-a47d4e8c021c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d52ac6077e8887ef92b02acdc324c26bc4a58c73
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176044"
---
# <a name="getcolumnlabel-method-sqlserverresultsetmetadata"></a>Метод getColumnLabel (SQLServerResultSetMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает предлагаемое название указанного столбца для использования при отображении и выводе на печать.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.lang.String getColumnLabel(int column)  
```  
  
#### <a name="parameters"></a>Параметры  
 *column*  
  
 Значение типа **int**, указывающее индекс столбца.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **String**, содержащее заголовок столбца.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод getColumnLabel задается с помощью метода getColumnLabel в интерфейсе java.sql.ResultSetMetaData.  
  
 Этот метод возвращает псевдоним столбца. Если псевдоним недоступен, этот метод возвращает имя столбца.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-methods.md)   
 [Элементы SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-members.md)   
 [Класс SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  

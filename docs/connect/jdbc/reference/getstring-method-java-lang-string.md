---
description: Метод getString (java.lang.String)
title: Метод getString (java.lang.String) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getString (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f67371e0-e879-4188-85fc-ecb85f0be2a9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad5a411e77b52130e9e1e4d43ebd38daff5844ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162222"
---
# <a name="getstring-method-javalangstring"></a>Метод getString (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Получает значение заданного параметра в виде значения типа данных **String** на языке программирования Java по заданному имени параметра.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.lang.String getString(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>Параметры  
 *sCol*  
  
 Значение типа **String**, содержащее имя параметра.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение типа **String**.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод getString определен с помощью метода getString в интерфейсе java.sql.CallableStatement.  
  
 Все столбцы в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] можно возвращать в виде строковых значений. Это значит, что можно возвращать строковое представление всех числовых и символьных типов, а также шестнадцатеричное представление двоичных столбцов, таких как binary, varbinary, varbinary(max), image, timestamp и uniqueidentifier.  
  
 Типы, зависящие от расположения, такие как money, smallmoney, datetime, smalldatetime, float, real, decimal и numeric, возвращают канонический формат toString() для базового значения типа.  
  
 Определяемые пользователем типы возвращаются в виде шестнадцатеричных строковых значений.  
  
## <a name="see-also"></a>См. также:  
 [Метод getString (SQLServerCallableStatement)](../../../connect/jdbc/reference/getstring-method-sqlservercallablestatement.md)   
 [Элементы SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Класс SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  

---
description: Метод setString (long, java.lang.String)
title: Метод setString (long, java.lang.String) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerClob.setString (long, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1b2190e9-5ace-497a-8554-0e913ea9b0cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 147cca169c1106557c36e644f7166da6cad18b3f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88450741"
---
# <a name="setstring-method-long-javalangstring"></a>Метод setString (long, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Записывает заданное значение **String** в CLOB, начиная с заданной позиции.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public int setString(long pos,  
                     java.lang.String s)  
```  
  
#### <a name="parameters"></a>Параметры  
 *pos*  
  
 Позиция, с которой начинается запись в объект CLOB.  
  
 *s*  
  
 Значение **String** для записи в объект CLOB.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Количество записанных символов.  
  
## <a name="exceptions"></a>Исключения  
 java.sql.SQLException  
  
## <a name="remarks"></a>Remarks  
 Этот метод setString определен с помощью метода setString в интерфейсе java.sql.Clob.  
  
 Символьные данные перезаписываются, начиная с указанной позиции, и могут превысить исходную длину объекта CLOB. Если указать значение позиции+1, будет добавлена строка. Если указать значение позиции+2 и более (либо нулевое или отрицательное значение), то создается ошибка позиции.  
  
## <a name="see-also"></a>См. также:  
 [Метод setString (SQLServerClob)](../../../connect/jdbc/reference/setstring-method-sqlserverclob.md)   
 [Методы SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Элементы SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Класс SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  

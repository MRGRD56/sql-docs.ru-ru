---
description: Метод refreshRow (SQLServerResultSet)
title: Метод refreshRow (SQLServerResultSet) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.refreshRow
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 048fe245-157f-4fd8-be75-ce54b83e02b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c99d1bfda98734585d3364d1a81cc45a888a0f2c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176697"
---
# <a name="refreshrow-method-sqlserverresultset"></a>Метод refreshRow (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Обновляет текущую строку, записывая последнее значение из базы данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public void refreshRow()  
```  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод refreshRow задается с помощью метода refreshRow в интерфейсе java.sql.ResultSet.  
  
 Этот метод не может быть вызван при нахождении курсора в строке вставки.  
  
 Этот метод обеспечивает для приложения способ явно указать драйверу JDBC на необходимость повторного извлечения строк из базы данных. Вызов этого метода может потребоваться приложению в том случае, когда [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] выполняет кэширование или предварительное получение, чтобы получить последнее значение строки из базы данных. Если размер выборки больше 1, драйвер JDBC может одновременно обновить несколько строк.  
  
 Все повторно извлеченные значения должны соответствовать уровню изоляции транзакции и чувствительности курсора. Если этот метод вызывается после вызова метода обновления, но до вызова метода [updateRow](../../../connect/jdbc/reference/updaterow-method-sqlserverresultset.md), то обновления строки будут потеряны. Вызов этого метода, вероятнее всего, приведет к снижению производительности.  
  
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Класс SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  

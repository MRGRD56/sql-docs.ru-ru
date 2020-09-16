---
description: Метод rowDeleted (SQLServerResultSet)
title: Метод rowDeleted (SQLServerResultSet) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.rowDeleted
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9c6db315-e614-4604-b020-41af6a214cc1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2e241a0eda4d67295aac47e51723e605fbae144b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88432736"
---
# <a name="rowdeleted-method-sqlserverresultset"></a>Метод rowDeleted (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает значение, определяющее, была ли удалена строка.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public boolean rowDeleted()  
```  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **true**, если строка удалена и обнаружены операции удаления. В противном случае — **false**.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод rowDeleted задается с помощью метода rowDeleted в интерфейсе java.sql.ResultSet.  
  
 Удаленная строка может оставить заметный пробел в результирующем наборе. Этот метод позволяет обнаруживать пробелы в результирующем наборе. Возвращаемое значение зависит от возможности обнаружения операций удаления этим объектом [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] обнаруживает удаленные строки для всех обновляемых типов курсора, хотя обнаружение является временным для однопроходных и динамических курсоров.  
  
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Класс SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  

---
description: Метод getBytes (SQLServerCallableStatement)
title: Метод getBytes (SQLServerCallableStatement) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerCallableStatement.getBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b6e88cea-54b3-4d18-a9af-db54abf19f45
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 98c52ff2ece018e496eb851a24e51e82af4f56bb
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88436936"
---
# <a name="getbytes-method-sqlservercallablestatement"></a>Метод getBytes (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает значение указанного параметра в виде массива байтов.  
  
## <a name="overload-list"></a>Список перегрузок  
  
|name|Описание|  
|----------|-----------------|  
|[getBytes (int)](../../../connect/jdbc/reference/getbytes-method-int.md)|Получает значение указанного параметра в виде массива байтов по заданному индексу параметра.|  
|[getBytes (java.lang.String)](../../../connect/jdbc/reference/getbytes-method-java-lang-string.md)|Получает значение указанного параметра в виде массива байтов по заданному имени параметра.|  
  
## <a name="remarks"></a>Remarks  
 В предыдущей версии [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] метод SQLServerCallableStatement.getBytes мог преобразовывать значения из байтового массива в типы данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**date**, **time**, **datetime2** или **datetimeoffset** и обратно. Теперь при вызове этого метода с такими типами данных возникает исключение, указывающее, что такое преобразование не поддерживается.  
  
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Класс SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  

---
description: Метод position (byte, long)
title: Метод position (byte, long) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerBlob.position (byte[], long)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 787412c2-4342-49c8-9ca2-7a9ddcd3277c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 81248632dfff287d349a627dd4a82499e80d1d64
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433066"
---
# <a name="position-method-byte-long"></a>Метод position (byte, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает положение указанного шаблона в BLOB-объекте по заданному **байту** шаблона массива и начального индекса.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public long position(byte[] bPattern,  
                     long start)  
```  
  
#### <a name="parameters"></a>Параметры  
 *bPattern*  
  
 Шаблон для поиска.  
  
 *start*  
  
 Индекс, с которого начинается поиск.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **long**, указывающее позицию, в которой обнаружен шаблон, или значение "-1", если шаблон не обнаружен.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод position задается с помощью метода position в интерфейсе java.sql.Blob.  
  
## <a name="see-also"></a>См. также:  
 [Метод position (SQLServerBlob)](../../../connect/jdbc/reference/position-method-sqlserverblob.md)   
 [Методы SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Элементы SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Класс SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  

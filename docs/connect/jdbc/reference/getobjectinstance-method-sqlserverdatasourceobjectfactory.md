---
description: Метод getObjectInstance (SQLServerDataSourceObjectFactory)
title: Метод getObjectInstance (SQLServerDataSourceObjectFactory) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSourceObjectFactory.getObjectInstance
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0a1503e2-e991-4d70-a223-087fc63baf73
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 79510f1e28f6f8933b417ad17ecac34713e885e1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88435066"
---
# <a name="getobjectinstance-method-sqlserverdatasourceobjectfactory"></a>Метод getObjectInstance (SQLServerDataSourceObjectFactory)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Получает экземпляр указанного объекта источника данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.lang.Object getObjectInstance(java.lang.Object ref,  
                                          javax.naming.Name name,  
                                          javax.naming.Context c,  
                                          java.util.Hashtable h)  
```  
  
#### <a name="parameters"></a>Параметры  
 *ref*  
  
 Значение **Object**.  
  
 *name*  
  
 Имя объекта.  
  
 *c*  
  
 Контекст, связанный с указанным именем.  
  
 *h*  
  
 Среда, используемая при создании объекта.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **Object**.  
  
## <a name="exceptions"></a>Исключения  
 java.sql.SQLException  
  
## <a name="remarks"></a>Remarks  
 Этот метод getObjectInstance задается с помощью метода getObjectInstance в интерфейсе javax.naming.spi.ObjectFactory.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerDataSourceObjectFactory](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-methods.md)   
 [Элементы SQLServerDataSourceObjectFactory](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-members.md)   
 [Класс SQLServerDataSourceObjectFactory](../../../connect/jdbc/reference/sqlserverdatasourceobjectfactory-class.md)  
  
  

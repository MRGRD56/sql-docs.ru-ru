---
description: Метод getObject (java.lang.String, java.util.Map)
title: Метод getObject (java.lang.String, java.util.Map) | Документы Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerCallableStatement.getObject (java.lang.String, Java.util.Map)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e174eb81-d569-479e-a171-365cd6d44b6a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58b1857781baea98e3dca98c00a69b20284a3235
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88435126"
---
# <a name="getobject-method-javalangstring-javautilmap"></a>Метод getObject (java.lang.String, java.util.Map)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Получает значение указанного параметра в виде объекта на языке программирования Java по имени параметра, используя указанный объект Map.  
  
> [!NOTE]  
>  Сейчас этот метод не поддерживается [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Этот метод всегда возвращает сопоставление по умолчанию.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.lang.Object getObject(java.lang.String sCol,  
                                  java.util.Map m)  
```  
  
#### <a name="parameters"></a>Параметры  
 *sCol*  
  
 Значение типа **String**, содержащее имя параметра.  
  
 *m*  
  
 Объект Map.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение **Object**.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод getObject определен с помощью метода getObject в интерфейсе java.sql.CallableStatement.  
  
 Этот метод вернет значение данного столбца в виде объекта Java. Объект Java будет иметь заданный по умолчанию тип, соответствующий типу SQL Server столбца и сопоставленный встроенным типам, указанным в спецификации JDBC. Если значение SQL NULL, то драйвер вернет значение Java NULL.  
  
 Этот метод также может быть использован для чтения абстрактных типов данных, относящихся к базе данных. В API JDBC 2.0 возможности метода getObject расширены и поддерживают материализацию данных определяемых пользователем типов SQL. Если в столбце содержится структурированное или уникальное значение, то выполнение этого метода будет аналогично вызову `getObject(columnIndex, this.getStatement().getConnection().getTypeMap())`.  
  
 Начиная с версии 3.0 драйвера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC действуют следующие правила:  
  
-   Значение типа **date** будет возвращаться в виде объекта java.sql.Date.  
  
-   Значение типа **time** будет возвращаться в виде объекта java.sql.Time.  
  
-   Значение типа **datetime2** будет возвращаться в виде объекта java.sql.Timestamp.  
  
-   Значение типа **datetimeoffset** будет возвращаться в виде объекта microsoft.sql.DateTimeOffset.  
  
## <a name="see-also"></a>См. также:  
 [Метод getObject (SQLServerCallableStatement)](../../../connect/jdbc/reference/getobject-method-sqlservercallablestatement.md)   
 [Элементы SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Класс SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  

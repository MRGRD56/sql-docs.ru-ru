---
description: Метод setObject (int, java.lang.Object, int, int)
title: Метод setObject (int, java.lang.Object, int, int) | Документы Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerPreparedStatement.setObject (int, java.lang.Object, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d190ee20-d669-4c6f-a081-d5cfec2f72ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f0dfff8bd93509a26cc499fbeeb420c8c3cdea0c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88458586"
---
# <a name="setobject-method-int-javalangobject-int-int"></a>Метод setObject (int, java.lang.Object, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Устанавливает значение указанного параметра по заданному объекту, целевому типу и масштабу.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public final void setObject(int n,  
                            java.lang.Object obj,  
                            int targetSqlType,  
                            int scale)  
```  
  
#### <a name="parameters"></a>Параметры  
 *n*  
  
 Значение **int**, определяющее номер параметра.  
  
 *obj*  
  
 Объект.  
  
 *targetSqlType*  
  
 Значение типа **int**, указывающее целевой тип, определенный в java.sql.Types.  
  
 *масштаб*  
  
 Значение типа **int**, указывающее количество разрядов справа от десятичного разделителя. Этот параметр не учитывается для всех типов, кроме NUMERIC и DECIMAL.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Метод setObject определен с помощью метода setObject в интерфейсе java.sql.PreparedStatement.  
  
 Начиная с версии [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0 на работу этого метода влияет свойство подключения **sendTimeAsDatetime** ([Настройка свойств подключения](../../../connect/jdbc/setting-the-connection-properties.md)) и [SQLServerDataSource.setSendTimeAsDatetime](../../../connect/jdbc/reference/setsendtimeasdatetime-method-sqlserverdatasource.md).  
  
 См. сведения о [настройке способа отправки значений java.sql.Time на сервер](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>См. также:  
 [Метод setObject (SQLServerPreparedStatement)](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md)   
 [Элементы SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Класс SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  

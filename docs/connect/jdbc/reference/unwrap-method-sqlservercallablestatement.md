---
description: Метод unwrap (SQLServerCallableStatement)
title: Метод unwrap (SQLServerCallableStatement) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cbbf2728-b8c8-4c35-875a-6e967c8285dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c2909870e0aaa5bdd7db86a567fb11915ede2428
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462409"
---
# <a name="unwrap-method-sqlservercallablestatement"></a>Метод unwrap (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает объект, реализующий указанный интерфейс для доступа к методам, относящимся к драйверу [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public <T> T unwrap(Class<T> iface)  
```  
  
#### <a name="parameters"></a>Параметры  
 *iface*  
  
 Класс типа **T**, определяющий интерфейс.  
  
## <a name="return-value"></a>Возвращаемое значение  
 Объект, реализующий указанный интерфейс.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Метод [unwrap](../../../connect/jdbc/reference/unwrap-method-sqlservercallablestatement.md) определен интерфейсом java.sql.Wrapper, который появился в спецификации JDBC 4.0.  
  
 Приложению может потребоваться доступ к расширениям интерфейса API JDBC, предоставляемым драйвером [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]. Метод unwrap поддерживает развертывание в открытые классы, предоставляемые этим объектом, если эти классы реализуют расширения поставщиков.  
  
 [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md) реализует интерфейс [ISQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md), расширяющий интерфейс [ISQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md). При вызове этого метода объект развертывается в следующие объекты: [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) и [SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md).  
  
 См. сведения об [интерфейсах и программах-оболочках](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
 В следующем примере кода показано, как использовать методы isWrapperFor и unwrap для проверки расширений драйвера и вызова методов, предоставленных поставщиками, например [setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md) и [getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md).  
  
```  
public static void executeStoredProcedure(Connection con) {  
   try {  
    CallableStatement cstmt =   
       con.prepareCall("{call dbo.stored_proc_name(?, ?)}");  
  
    // The recommended way to access the JDBC   
    // Driver-specific methods is to use the JDBC 4.0 Wrapper   
    // functionality.   
    // The following code statements demonstrates how to use the   
    // isWrapperFor and unwrap methods  
    // to access the driver-specific response buffering methods.  
  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerCallableStatement.class)) {  
     // The CallableStatement object can unwrap to   
     // SQLServerCallableStatement.  
     SQLServerCallableStatement SQLcstmt =   
     cstmt.unwrap(  
        com.microsoft.sqlserver.jdbc.SQLServerCallableStatement.class);  
     SQLcstmt.setResponseBuffering("adaptive");  
     System.out.println("Response buffering mode has been set to " +  
         SQLcstmt.getResponseBuffering());  
     }  
  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement.class)) {  
      // The CallableStatement object can unwrap to   
      // SQLServerPreparedStatement.                    
      SQLServerPreparedStatement SQLpstmt =   
       cstmt.unwrap(  
       com.microsoft.sqlserver.jdbc.SQLServerPreparedStatement.class);  
      SQLpstmt.setResponseBuffering("adaptive");  
      System.out.println("Response buffering mode has been set to " +  
          SQLpstmt.getResponseBuffering());  
    }  
    if (cstmt.isWrapperFor(  
      com.microsoft.sqlserver.jdbc.SQLServerStatement.class)) {  
  
      // The CallableStatement object can unwrap to SQLServerStatement.   
      SQLServerStatement SQLstmt =   
        cstmt.unwrap(  
        com.microsoft.sqlserver.jdbc.SQLServerStatement.class);  
      SQLstmt.setResponseBuffering("adaptive");  
      System.out.println("Response buffering mode has been set to " +  
      SQLstmt.getResponseBuffering());  
    }  
  }  
  catch (Exception e) {  
     e.printStackTrace();  
  }  
}   
```  
  
## <a name="see-also"></a>См. также:  
 [Метод isWrapperFor (SQLServerCallableStatement)](../../../connect/jdbc/reference/iswrapperfor-method-sqlservercallablestatement.md)   
 [Элементы SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Класс SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  

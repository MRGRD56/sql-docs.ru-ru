---
description: Метод getURL (SQLServerDatabaseMetaData)
title: Метод getURL (SQLServerDatabaseMetaData) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getURL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: fcb66851-db5f-4ae8-b728-d129480b6f42
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 095c057127d16f1587177fdfbade321d3043e057
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154755"
---
# <a name="geturl-method-sqlserverdatabasemetadata"></a>Метод getURL (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Возвращает URL-адрес для этой базы данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public java.lang.String getURL()  
```  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение типа **String**, содержащее URL-адрес.  
  
## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Remarks  
 Этот метод getURL указывается методом getURL в интерфейсе java.sql.DatabaseMetaData.  
  
 При использовании [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] с базой данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] этот метод возвращает значение типа **String**, которое содержит следующие сведения.  
  
-   Значение URL-адреса — «jdbc:sqlserver://»  
  
-   Дополнительные свойства подключения, такие как **имя_сервера**, **имя_экземпляра** и **номер_порта**  
  
-   Другие свойства подключения, заданные пользователем, а также все свойства подключения с непустыми или отличными от NULL значениями по умолчанию для драйвера, кроме **userName**, **password** и **integratedSecurity**.  
  
## <a name="see-also"></a>См. также:  
 [Методы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Элементы SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Класс SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  

---
description: Интерфейс ISQLServerPreparedStatement
title: Интерфейс ISQLServerPreparedStatement | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: cf87892e-5c34-4ac6-8258-c2a81e117b26
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6f3d76d2630ea3b369b823ee84b5e68d3c356ad4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177326"
---
# <a name="isqlserverpreparedstatement-interface"></a>Интерфейс ISQLServerPreparedStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Представляет базовую реализацию функциональности подготовленной инструкции JDBC. Этот интерфейс добавлен в версии 3.0 драйвера JDBC [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 **Пакет:** com.microsoft.sqlserver.jdbc  
  
 **Расширяет:** java.sql.PreparedStatement, [ISQLServerStatement](../../../connect/jdbc/reference/isqlserverstatement-interface.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public interface ISQLServerPreparedStatement  
```  
  
## <a name="remarks"></a>Remarks  
 Этот интерфейс реализуется с помощью [класса SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
 Этот интерфейс обеспечивает доступ к следующим методам, определяемым [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]:  
  
|Метод|Дополнительные сведения см. в разделе|  
|------------|-------------------------------|  
|public void setDateTimeOffset(int, microsoft.sql.DateTimeOffset)|[setDateTimeOffset](../../../connect/jdbc/reference/setdatetimeoffset-method-sqlserverpreparedstatement.md)|  
  
## <a name="see-also"></a>См. также:  
 [Справка по API драйвера JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  

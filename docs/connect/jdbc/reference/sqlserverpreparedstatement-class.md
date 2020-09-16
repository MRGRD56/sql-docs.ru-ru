---
description: Класс SQLServerPreparedStatement
title: Класс SQLServerPreparedStatement | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a8481c06-fbba-432b-8c69-4f4619c20ad4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 75a04a0873308cc09c16b54f5d482ed54a20c0b1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88472146"
---
# <a name="sqlserverpreparedstatement-class"></a>Класс SQLServerPreparedStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Представляет базовую реализацию функциональности подготовленной инструкции JDBC.  
  
 **Пакет:** com.microsoft.sqlserver.jdbc  
  
 **Расширяет:** SQLServerStatement  
  
 **Реализует:** [ISQLServerPreparedStatement](../../../connect/jdbc/reference/isqlserverpreparedstatement-interface.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public class SQLServerPreparedStatement  
```  
  
## <a name="remarks"></a>Remarks  
 SQLServerPreparedStatement предоставляет методы, позволяющие указать для параметров любые типы машинного кода Java и различные типы объектов Java. Класс SQLServerPreparedStatement выполняет подготовку инструкции с помощью хранимой процедуры [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **sp_prepare**, а затем использует возвращенный дескриптор инструкции для каждого последующего выполнения инструкции (обычно при этом используются различные параметры, указываемые пользователем).  
  
 SQLServerPreparedStatement поддерживает пакетную организацию, когда набор подготовленных инструкций выполняется за одно обращение к базе данных, чтобы повысить производительность выполнения.  
  
 Этот класс поддерживает распаковку в класс SQLServerPreparedStatement, интерфейсы ISQLServerPreparedStatement и java.sql.PreparedStatement, и в любые другие классы и интерфейсы, для которых SQLServerStatement поддерживает распаковку. См. сведения об [интерфейсах и программах-оболочках](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Справка по API драйвера JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  

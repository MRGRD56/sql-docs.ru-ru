---
description: Метод getEnablePrepareOnFirstPreparedStatementCall (SQLServerConnection)
title: Метод getEnablePrepareOnFirstPreparedStatementCall (SQLServerConnection) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getEnablePrepareOnFirstPreparedStatementCall
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 90feee153d1e020801418cdae679d862fd95da3e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175801"
---
# <a name="getenableprepareonfirstpreparedstatementcall-method-sqlserverconnection"></a>Метод getEnablePrepareOnFirstPreparedStatementCall (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Возвращает значение свойства подключения **enablePrepareOnFirstPreparedStatementCall**. Если это значение равно false, первое выполнение вызывает sp_executesql и не подготавливает инструкцию, а второе выполнение вызывает sp_prepexec и фактически настраивает обработчик подготовленной инструкции. При всех последующих выполнениях вызывается sp_execute. Это избавляет от необходимости аннулировать закрываемую подготовленную инструкцию вызовом sp_unprepare, если эта инструкция выполнялась лишь один раз. Значение по умолчанию для этого параметра можно изменить, вызвав setDefaultEnablePrepareOnFirstPreparedStatementCall().

## <a name="syntax"></a>Синтаксис  
  
```  
  
public boolean getEnablePrepareOnFirstPreparedStatementCall()  
```  

## <a name="return-value"></a>Возвращаемое значение
 Возвращает **логическое** значение свойства подключения **enablePrepareOnFirstPreparedStatementCall**.

## <a name="exceptions"></a>Исключения  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Remarks  
 Этот метод доступен в драйвере JDBC 6.4 и более поздних версий.
 
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Класс SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  

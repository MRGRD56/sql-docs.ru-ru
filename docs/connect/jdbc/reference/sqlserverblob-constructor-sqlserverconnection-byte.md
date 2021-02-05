---
description: Конструктор SQLServerBlob (SQLServerConnection, byte)
title: Конструктор SQLServerBlob (SQLServerConnection, byte) | Документы Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection, byte[].SQLServerBlob
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9fe573e3-30db-4828-abab-e9346493e931
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2ff6f2a7fadaa23df917e07b10b20f2cf3df427
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178405"
---
# <a name="sqlserverblob-constructor-sqlserverconnection-byte"></a>Конструктор SQLServerBlob (SQLServerConnection, byte)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Инициализирует новый экземпляр класса [SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md), если имеется объект [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) и массив **byte**.  
  
> [!NOTE]  
>  Этот метод устарел в версии драйвера JDBC 2.0. Используйте вместо него метод [createBlob](../../../connect/jdbc/reference/createblob-method-sqlserverconnection.md) класса [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public SQLServerBlob(SQLServerConnection connection,  
                     byte[] data)  
```  
  
#### <a name="parameters"></a>Параметры  
 *connection*;  
  
 Объект SQLServerConnection.  
  
 *data*  
  
 Массив **byte**.  
  
## <a name="see-also"></a>См. также:  
 [Конструкторы SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-constructors.md)   
 [Элементы SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Класс SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  

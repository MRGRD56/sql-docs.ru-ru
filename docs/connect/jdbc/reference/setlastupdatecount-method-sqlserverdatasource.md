---
description: Метод setLastUpdateCount (SQLServerDataSource)
title: Метод setLastUpdateCount (SQLServerDataSource) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setLastUpdateCount
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5487631a-1107-4169-84ca-b77fd09bea66
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dc4cc7f66e75ce6c3f69a65ab9ab71e3f4769212
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173406"
---
# <a name="setlastupdatecount-method-sqlserverdatasource"></a>Метод setLastUpdateCount (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Задание значения **Boolean**, определяющее, включено ли свойство lastUpdateCount.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public void setLastUpdateCount(boolean lastUpdateCount)  
```  
  
#### <a name="parameters"></a>Параметры  
 *lastUpdateCount*  
  
 Значение **true**, если включено свойство lastUpdateCount. В противном случае — **false**.  
  
## <a name="remarks"></a>Remarks  
 Если свойство lastUpdateCount содержит значение **true**, то драйвер [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] вернет только последний счетчик обновления из инструкции SQL, переданной серверу. Если свойство lastUpdateCount имеет значение **false**, то драйвер вернет все счетчики обновлений, в том числе и те, которые возвращены всеми сработавшими триггерами. Если свойство lastUpdateCount не задано, то метод [getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md) возвращает значение по умолчанию **true**.  
  
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Класс SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  

---
description: sqlsrv_get_config
title: sqlsrv_get_config | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- sqlsrv_get_config
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_get_config
- sqlsrv_get_config
ms.assetid: ce2befc2-af98-45bb-8d41-60f1674dccfc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d8b78e001b666fbc45f204aee9488e740c95208
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88426296"
---
# <a name="sqlsrv_get_config"></a>sqlsrv_get_config
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Возвращает текущее значение указанного параметра конфигурации.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sqlsrv_get_config( string $setting )  
```  
  
#### <a name="parameters"></a>Параметры  
*$setting*: параметр конфигурации, для которого возвращается значение. Список настраиваемых параметров см. в статье [sqlsrv_configure](../../connect/php/sqlsrv-configure.md).  
  
## <a name="return-value"></a>Возвращаемое значение  
Значение параметра, определяемое параметром *$setting* . Если указано недопустимое значение, возвращается значение **false** , а в коллекцию ошибок добавляется ошибка.  
  
## <a name="remarks"></a>Remarks  
Если **false** config **sqlsrv_get_config**, необходимо вызвать [sqlsrv_errors](../../connect/php/sqlsrv-errors.md) , чтобы определить, произошла ли ошибка или значение **false** параметра определено параметром *$setting* .  
  
## <a name="see-also"></a>См. также:  
[Справочник по API для драйвера SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[sqlsrv_configure](../../connect/php/sqlsrv-configure.md)  

[sqlsrv_errors](../../connect/php/sqlsrv-errors.md)  
  

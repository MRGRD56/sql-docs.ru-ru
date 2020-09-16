---
description: Метод setURL (SQLServerDataSource)
title: Метод setURL (SQLServerDataSource) | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDataSource.setURL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bea70100-ac98-4625-8748-ef7cc0b111ea
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3e27df8b938cee4503a2078d597b12eb880f52c4
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88467126"
---
# <a name="seturl-method-sqlserverdatasource"></a>Метод setURL (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Задает URL-адрес, используемый для соединения с источником данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
public void setURL(java.lang.String url)  
```  
  
#### <a name="parameters"></a>Параметры  
 *url*  
  
 Значение типа **String**, содержащее URL-адрес.  
  
## <a name="remarks"></a>Remarks  
 По соображениям безопасности не следует включать пароль в URL-адрес, передаваемый в метод setURL. Так происходит из-за того, что на серверах приложений Java сторонних разработчиков в пользовательском интерфейсе настройки для источника данных часто выводится значение, заданное для свойства URL-адреса. Вместо этого используйте для задания значения пароля метод [setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md). На серверах приложений Java не отображается пароль, заданный в источнике данных в пользовательском интерфейсе настройки.  
  
> [!NOTE]  
>  Если метод setURL не вызван перед методом [getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md), то getURL возвращает значение по умолчанию — "jdbc:sqlserver://".  
  
## <a name="see-also"></a>См. также:  
 [Элементы SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Класс SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  

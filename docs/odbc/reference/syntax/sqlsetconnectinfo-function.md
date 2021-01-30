---
description: Функция SQLSetConnectInfo
title: Функция Склсетконнектинфо | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectInfo function [ODBC]
ms.assetid: 0782a1c3-c5d1-499b-a8ba-134162db9990
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1930d16605c1b2a02db08560549019afa656ca52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192428"
---
# <a name="sqlsetconnectinfo-function"></a>Функция SQLSetConnectInfo
**Соответствия**  
 Представленная версия: соответствие стандартам ODBC 3,81: ODBC  
  
 **Сводка**  
 **Склсетконнектинфо** используется для задания источника данных, идентификатора пользователя и пароля в токене сведений о соединении для вызова [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) приложения.  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp
  
SQLRETURN  SQLSetConnectInfo(  
                SQLHDBC_INFO_TOKEN   TokenHandle,  
                WCHAR *              ServerName,  
                SQLSMALLINT          NameLength1,  
                WCHAR *              UserName,  
                SQLSMALLINT          NameLength2,  
                WCHAR *              Authentication,  
                SQLSMALLINT          NameLength3 );  
```  
  
## <a name="arguments"></a>Аргументы  
 *токенхандле*  
 Входной Маркер маркера.  
  
 *ServerName*  
 Входной Имя источника данных. Данные могут находиться на том же компьютере, что и программа, или на другом компьютере в сети. Сведения о том, как приложение выбирает источник данных, см. [в разделе Выбор источника данных или драйвера](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 *NameLength1*  
 Входной Длина **ServerName* в символах.  
  
 *UserName*  
 Входной Идентификатор пользователя.  
  
 *NameLength2*  
 Входной Длина *имени пользователя* в символах.  
  
 *Аутентификация*  
 Входной Строка проверки подлинности (обычно пароль).  
  
 *NameLength3*  
 Входной Длина **Проверка подлинности* в символах.  
  
## <a name="returns"></a>Возвращаемое значение  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR или SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Диагностика  
 То же, что и [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) для ошибок проверки ввода, за исключением того, что диспетчер драйверов будет использовать **параметром handletype** SQL_HANDLE_DBC_INFO_TOKEN и **маркер** *хдбЦинфотокен*.  
  
## <a name="remarks"></a>Замечания  
 Всякий раз, когда драйвер возвращает SQL_ERROR или SQL_INVALID_HANDLE, диспетчер драйверов возвращает ошибку приложению (в [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) или [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)).  
  
 Всякий раз, когда драйвер возвращает SQL_SUCCESS_WITH_INFO, диспетчер драйверов получает диагностические сведения от *хдбЦинфотокен* и возвращает SQL_SUCCESS_WITH_INFO приложению в [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) и [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md).  
  
 Приложения не должны вызывать эту функцию напрямую. Драйвер ODBC, поддерживающий пулы соединений с учетом драйверов, должен реализовывать эту функцию.  
  
 Включите склспи. h для разработки драйвера ODBC.  
  
## <a name="see-also"></a>См. также:  
 [Разработка драйвера ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Организация пулов соединений с учетом драйверов](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Разработка драйвера ODBC с поддержкой пула подключений](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)

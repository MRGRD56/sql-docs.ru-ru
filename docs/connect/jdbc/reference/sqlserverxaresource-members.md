---
description: Элементы SQLServerXAResource
title: Элементы SQLServerXAResource | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a069bf2c-1b70-4817-b084-a508445de799
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 81a63c16b08b7c1d418614a1e521cc8bede0b0f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181667"
---
# <a name="sqlserverxaresource-members"></a>Элементы SQLServerXAResource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  В следующих таблицах перечислены доступные члены класса [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md).  
  
## <a name="constructors"></a>Конструкторы  
 Нет.  
  
## <a name="fields"></a>Поля  
  
|name|Описание|  
|----------|-----------------|  
|[SSTRANSTIGHTLYCPLD](../../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md)|Используется для разрешения тесно связанных транзакций XA, имеющих различные идентификаторы ветвей транзакции (XID), но одинаковые глобальные идентификаторы транзакции (GTRID).|  
  
## <a name="inherited-fields"></a>Наследуемые поля  
  
|Класс, из которого наследуется:|Методы|  
|---------------------------|-------------|  
|javax.transaction.xa.XAResource|TMENDRSCAN, TMFAIL, TMJOIN, TMNOFLAGS, TMONEPHASE, TMRESUME, TMSTARTRSCAN, TMSUCCESS, TMSUSPEND, XA_OK, XA_RDONLY|  
  
## <a name="methods"></a>Методы  
  
|Имя|Описание|  
|----------|-----------------|  
|[commit](../../../connect/jdbc/reference/commit-method-sqlserverxaresource.md)|Фиксирует глобальную транзакцию, которая указана в заданном объекте Xid.|  
|[end](../../../connect/jdbc/reference/end-method-sqlserverxaresource.md)|Завершает работу, выполняемую от имени ветви транзакции.|  
|[forget](../../../connect/jdbc/reference/forget-method-sqlserverxaresource.md)|Удаляет в диспетчере ресурсов данные об эвристически выполненной ветви транзакции.|  
|[getTransactionTimeout](../../../connect/jdbc/reference/gettransactiontimeout-method-sqlserverxaresource.md)|Получает значение времени ожидания текущей транзакции, заданное для этого объекта [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md).|  
|[isSameRM](../../../connect/jdbc/reference/issamerm-method-sqlserverxaresource.md)|Определяет, совпадает ли экземпляр диспетчера ресурсов, представленный целевым объектом, с экземпляром диспетчера ресурсов, представленным заданным объектом XAResource.|  
|[prepare](../../../connect/jdbc/reference/prepare-method-sqlserverxaresource.md)|Запрашивает в диспетчере ресурсов подготовку к фиксации транзакции, указанной в заданном объекте Xid.|  
|[recover](../../../connect/jdbc/reference/recover-method-sqlserverxaresource.md)|Получает список ветвей подготовленной транзакции от диспетчера ресурсов.|  
|[rollback](../../../connect/jdbc/reference/rollback-method-sqlserverxaresource.md)|Запрашивает в диспетчере ресурсов откат работы, выполненной от имени ветви транзакции.|  
|[setTransactionTimeout](../../../connect/jdbc/reference/settransactiontimeout-method-sqlserverxaresource.md)|Задает значение времени ожидания текущей транзакции для этого объекта [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md).|  
|[start](../../../connect/jdbc/reference/start-method-sqlserverxaresource.md)|Начинает работу от имени ветви транзакции, указанной в объекте Xid.|  
  
## <a name="inherited-methods"></a>Наследуемые методы  
  
|Класс, из которого наследуется:|Методы|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
  
## <a name="see-also"></a>См. также:  
 [Класс SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  

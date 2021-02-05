---
description: Элементы SQLServerConnectionPoolDataSource
title: Элементы SQLServerConnectionPoolDataSource | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: dac0337e-8088-488c-a25a-801a2190f6ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 614bcf5295b776cdd61fc9bb7b070fe1e26cbb69
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178348"
---
# <a name="sqlserverconnectionpooldatasource-members"></a>Элементы SQLServerConnectionPoolDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  В следующих таблицах перечислены элементы класса [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md).  
  
## <a name="constructors"></a>Конструкторы  
  
|Имя|Описание|  
|----------|-----------------|  
|[SQLServerConnectionPoolDataSource ()](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-constructor.md)|Инициализирует новый экземпляр класса [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md).|  
  
## <a name="fields"></a>Поля  
 Нет.  
  
## <a name="inherited-fields"></a>Наследуемые поля  
 Нет.  
  
## <a name="methods"></a>Методы  
  
|Имя|Описание|  
|----------|-----------------|  
|[getApplicationIntent](../../../connect/jdbc/reference/getapplicationintent-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает значение свойства подключения **applicationIntent**.|  
|[getApplicationName](../../../connect/jdbc/reference/getapplicationname-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает имя приложения.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Устанавливает подключение к источнику данных, который представляется этим объектом DataSource.|  
|[getDatabaseName](../../../connect/jdbc/reference/getdatabasename-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает имя базы данных.|  
|[getDescription](../../../connect/jdbc/reference/getdescription-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает описание источника данных.|  
|[getFailoverPartner](../../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает имя сервера отработки отказа, которое используется в параметрах зеркального отображения баз данных.|  
|[getInstanceName](../../../connect/jdbc/reference/getinstancename-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getLastUpdateCount](../../../connect/jdbc/reference/getlastupdatecount-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает значение типа **boolean**, которое указывает, включено ли свойство lastUpdateCount.|  
|[getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает значение типа **int**, которое указывает продолжительность ожидания (в мс) до того, как база данных сообщит об истечении времени ожидания блокировки.|  
|[getLoginTimeout](../../../connect/jdbc/reference/getlogintimeout-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает продолжительность ожидания (в с) объекта DataSource при попытке установить подключение.|  
|[getLogWriter](../../../connect/jdbc/reference/getlogwriter-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает символьный выходной поток, который используется для всех сообщений ведения журнала и трассировки.|  
|[getMultiSubnetFailover](../../../connect/jdbc/reference/getmultisubnetfailover-method-sqlserverdatasource.md)|(Наследуется от класса [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает значение свойства подключения **multiSubnetFailover**.|  
|[getPooledConnection](../../../connect/jdbc/reference/getpooledconnection-method-sqlserverconnectionpooldatasource.md)|Устанавливает физическое соединение с базой данных, которое будет использоваться в пулах соединений.|  
|[getPortNumber](../../../connect/jdbc/reference/getportnumber-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает номер текущего порта, который используется для взаимодействия с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getReference](../../../connect/jdbc/reference/getreference-method-sqlserverconnectionpooldatasource.md)|Возвращает ссылку на этот объект DataSource.|  
|[getSelectMethod](../../../connect/jdbc/reference/getselectmethod-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает тип курсора по умолчанию, который используется для всех результирующих наборов, создаваемых с помощью этого объекта DataSource.|  
|[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает значение типа **Boolean**, определяющее, включена ли отправка на сервер строковых параметров в формате Юникода.|  
|[getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает имя компьютера, на котором выполняется [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает URL-адрес, который используется для подключения к источнику данных.|  
|[getUser](../../../connect/jdbc/reference/getuser-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает имя пользователя, которое используется для подключения к источнику данных.|  
|[getWorkstationID](../../../connect/jdbc/reference/getworkstationid-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает имя клиентского компьютера, которое используется для подключения к источнику данных.|  
|[getXopenStates](../../../connect/jdbc/reference/getxopenstates-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Возвращает значение типа **Boolean**, которое указывает, включено ли преобразование состояний SQL в состояния, соответствующие стандарту XOPEN.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverconnectionpooldatasource.md)|Указывает, является ли этот объект оболочкой указанного интерфейса.|  
|[setApplicationIntent](../../../connect/jdbc/reference/setapplicationintent-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает значение свойства подключения **applicationIntent**.|  
|[setApplicationName](../../../connect/jdbc/reference/setapplicationname-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает имя приложения.|  
|[setAuthenticationSceme](../../../connect/jdbc/reference/setauthenticationscheme-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Указывает тип встроенной безопасности, который должно использовать приложение.|  
|[setDatabaseName](../../../connect/jdbc/reference/setdatabasename-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает имя базы данных, к которой необходимо установить подключение.|  
|[setDescription](../../../connect/jdbc/reference/setdescription-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает описание источника данных.|  
|[setFailoverPartner](../../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает имя сервера отработки отказа, которое используется в зеркальной конфигурации баз данных.|  
|[setInstanceName](../../../connect/jdbc/reference/setinstancename-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает имя экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setIntegratedSecurity](../../../connect/jdbc/reference/setintegratedsecurity-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает значение типа **Boolean**, которое указывает, включено ли свойство integratedSecurity.|  
|[setLastUpdateCount](../../../connect/jdbc/reference/setlastupdatecount-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает значение типа **Boolean**, которое указывает, включено ли свойство lastUpdateCount.|  
|[setLockTimeout](../../../connect/jdbc/reference/setlocktimeout-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает значение типа **int**, которое указывает продолжительность ожидания (в мс) до того, как база данных сообщит об истечении времени ожидания блокировки.|  
|[setLoginTimeout](../../../connect/jdbc/reference/setlogintimeout-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает продолжительность ожидания (в с) объекта DataSource при попытке установить подключение.|  
|[setLogWriter](../../../connect/jdbc/reference/setlogwriter-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает символьный выходной поток, который используется для всех сообщений ведения журнала и трассировки.|  
|[setMultiSubnetFailover](../../../connect/jdbc/reference/setmultisubnetfailover-method-sqlserverdatasource.md)|(Наследуется от класса [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)). Задает значение свойства подключения **multiSubnetFailover**.|  
|[setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает пароль, который будет использоваться для подключения к [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setPortNumber](../../../connect/jdbc/reference/setportnumber-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает номер порта, который используется для взаимодействия с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setSelectMethod](../../../connect/jdbc/reference/setselectmethod-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает тип курсора по умолчанию, который используется для всех результирующих наборов, создаваемых с помощью этого объекта DataSource.|  
|[setSendStringParametersAsUnicode](../../../connect/jdbc/reference/setsendstringparametersasunicode-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает значение типа **Boolean**, определяющее, включена ли отправка на сервер строковых параметров в формате Юникода.|  
|[setServerName](../../../connect/jdbc/reference/setservername-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает имя компьютера, на котором выполняется [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|[setURL](../../../connect/jdbc/reference/seturl-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает URL-адрес, который используется для подключения к источнику данных.|  
|[setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает имя пользователя, которое используется для подключения к источнику данных.|  
|[setWorkstationID](../../../connect/jdbc/reference/setworkstationid-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает имя клиентского компьютера, которое используется для подключения к источнику данных.|  
|[setXopenStates](../../../connect/jdbc/reference/setxopenstates-method-sqlserverdatasource.md)|(Наследуется от [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)) Задает значение типа **boolean**, которое указывает, включено ли преобразование состояний SQL в состояния, соответствующие стандарту XOPEN.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverconnectionpooldatasource.md)|Возвращает объект, реализующий указанный интерфейс для доступа к методам, относящимся к драйверу [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Наследуемые методы  
  
|Класс, из которого наследуется:|Методы|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerDataSource|getApplicationName, getConnection, getDatabaseName, getDescription, getFailoverPartner, getInstanceName, getLastUpdateCount, getLockTimeout, getLoginTimeout, getLogWriter, getPortNumber, getSelectMethod, getSendStringParametersAsUnicode, getServerName, getURL, getUser, getWorkstationID, getXopenStates, setApplicationName, setDatabaseName, setDescription, setFailoverPartner, setInstanceName, setIntegratedSecurity, setLastUpdateCount, setLockTimeout, setLoginTimeout, setLogWriter, setPassword, setPortNumber, setSelectMethod, setSendStringParametersAsUnicode, setServerName, setURL, setUser, setWorkstationID, setXopenStates|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
|javax.sql.ConnectionPoolDataSource|getLoginTimeout, getLogWriter, setLoginTimeout, setLogWriter, getPooledConnection|  
  
## <a name="see-also"></a>См. также:  
 [Класс SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
  

---
description: SQLGetConnectAttr
title: SQLGetConnectAttr | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetConnectAttr function
ms.assetid: 26e4e69a-44fd-45e3-b47a-ae39184f041b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3991a2b999d993c507703f37d58cd6e397665779
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753314"
---
# <a name="sqlgetconnectattr"></a>SQLGetConnectAttr
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Драйвер ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определяет характерные для драйвера атрибуты соединения. Некоторые из этих атрибутов доступны для функции **SQLGetConnectAttr**, а сама функция используется для определения их текущих значений. Нельзя быть уверенным в правильности значений этих атрибутов, сообщаемых функцией, до тех пор, пока не будет установлено соединение или атрибут не будет задан при помощи функции [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).  
  
 В этом разделе приведены атрибуты режима только для чтения. Сведения о других атрибутах подключения, относящихся к драйверу поставщика ODBC собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , см. в разделе [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).  
  
## <a name="sql_copt_ss_connection_dead"></a>SQL_COPT_SS_CONNECTION_DEAD  
 Атрибут SQL_COPT_SS_CONNECTION_DEAD сообщает серверу данные о состоянии соединения. Для определения текущего состояния соединения драйвер запрашивает сеть.  
  
> [!NOTE]  
>  Стандартный атрибут соединения ODBC SQL_ATTR_CONNECTION_DEAD возвращает последнее по времени состояние соединения. Может оказаться так, что это состояние соединения будет отличаться от текущего.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_CD_TRUE|Соединение с сервером потеряно.|  
|SQL_CD_FALSE|Соединение открыто и доступно для обработки инструкций.|  
  
## <a name="sql_copt_ss_client_connection_id"></a>SQL_COPT_SS_CLIENT_CONNECTION_ID  
 Атрибут SQL_COPT_SS_CLIENT_CONNECTION_ID извлекает идентификатор соединения клиента, по которому затем производится поиск и обнаружение следующих данных.  
  
-   Диагностические сведения в журнале XEvents, если он включен.  
  
-   Сведения об ошибке соединения в кольцевом буфере соединения.  
  
-   Диагностические сведения в журналах отслеживания доступа к данным, если они включены.  
  
 Дополнительные сведения см. [в разделе доступ к диагностическим сведениям в журнале расширенных событий](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md).  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_ERROR|Ошибка соединения|  
|SQL_SUCCESS|Подключение выполнено успешно. Идентификатор соединения клиента будет находиться в выходном буфере.|  
  
## <a name="sql_copt_ss_perf_data"></a>SQL_COPT_SS_PERF_DATA  
 Атрибут SQL_COPT_SS_PERF_DATA возвращает указатель на структуру SQLPERF, содержащую текущую статистику производительности драйвера. Если ведение журнала производительности не включено, функция **SQLGetConnectAttr** возвращает значение NULL. Драйвер не обновляет статистику в структуре SQLPERF динамически. Каждый раз, когда возникает необходимость обновить статистику производительности, вызывайте функцию **SQLGetConnectAttr** .  
  
|Значение|Описание|  
|-----------|-----------------|  
|NULL|Ведение журнала производительности не включено.|  
|Любое другое значение|Указатель на структуру SQLPERF.|  
  
## <a name="sql_copt_ss_perf_query"></a>SQL_COPT_SS_PERF_QUERY  
 Если запись в журнал данных о длительных запросах включена, то атрибут SQL_COPT_SS_PERF_QUERY возвращает значение TRUE. Если запись в журнал данных о запросах неактивна, этот атрибут возвращает значение FALSE.  
  
## <a name="sql_copt_ss_user_data"></a>SQL_COPT_SS_USER_DATA  
 Атрибут SQL_COPT_SS_USER_DATA извлекает указатель на данные пользователя. Пользовательские данные хранятся в принадлежащей клиенту памяти и записываются отдельно для каждого соединения. Если указатель на данные пользователя на задан, что соответствует значению SQL_UD_NOTSET, то возвращается указатель NULL.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_UD_NOTSET|Указатель на данные пользователя не задан.|  
|Любое другое значение|Указатель на данные пользователя.|  
  
## <a name="sqlgetconnectattr-support-for-service-principal-names-spns"></a>Поддержка функции SQLGetConnectAttr для имен участников-служб (SPN)  
 SQLGetConnectAttr можно использовать для запроса значения новых атрибутов соединения SQL_COPT_SS_SERVER_SPN, SQL_COPT_SS_FAILOVER_PARTNER_SPN, SQL_COPT_SS_MUTUALLY_AUTHENTICATED и SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD. (SQLGetConnectOption также можно использовать для запроса этих значений.)  
  
 Атрибут SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD доступен только для открытых соединений, в которых используется проверка подлинности Windows.  
  
 Если атрибут SQL_COPT_SS_SERVER_SPN или SQL_COPT_SS_FAILOVER_PARTNER еще не задан, возвращается значение по умолчанию (пустая строка).  
  
 Дополнительные сведения о SPN см. [в статье имена субъектов-служб &#40;имен участников-служб&#41; в клиентских подключениях &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md).  
  
## <a name="see-also"></a>См. также:  
 [Функция SQLGetConnectAttr](../../odbc/reference/syntax/sqlgetconnectattr-function.md)   
 [Сведения о реализации API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)   
 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md)  
  

---
title: SQLSetConnectAttr | Документация Майкрософт
description: Сведения об атрибутах подключения в SQLSetConnectAttr, включая, когда они заданы и являются возможными значениями в драйвере SQL Server Native Client ODBC.
ms.custom: ''
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLSetConnectAttr function
ms.assetid: d21b5cf1-3724-43f7-bc96-5097df0677b4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 30893a308be9cb7634f3c0ceb755572e0e5b274b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754834"
---
# <a name="sqlsetconnectattr"></a>SQLSetConnectAttr

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В драйвере ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметр SQL_ATTR_CONNECTION_TIMEOUT не учитывается.  
  
 Не учитывается также настройка SQL_ATTR_TRANSLATE_LIB, указание другой библиотеки перевода не поддерживается. Чтобы приложения, использующие драйвер Microsoft ODBC для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], можно было легко переносить, любое значение, заданное с помощью настройки SQL_ATTR_TRANSLATE_LIB, копируется в буфер и из буфера диспетчера драйверов.  
  
 Драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] реализует уровень изоляции транзакции repeatable read с повторяющимся чтением как сериализуемый.  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] обеспечил поддержку нового атрибута изоляции транзакций, SQL_COPT_SS_TXN_ISOLATION. Задание SQL_COPT_SS_TXN_ISOLATION значения SQL_TXN_SS_SNAPSHOT указывает, что транзакция произойдет на уровне изоляции моментального снимка.  
  
> [!NOTE]  
> SQL_ATTR_TXN_ISOLATION можно использовать для установки всех других уровней изоляции за исключением SQL_TXN_SS_SNAPSHOT. При необходимости использовать изоляцию моментальных снимков, следует установить SQL_TXN_SS_SNAPSHOT через SQL_COPT_SS_TXN_ISOLATION. Однако уровень изоляции можно получить с помощью SQL_ATTR_TXN_ISOLATION или SQL_COPT_SS_TXN_ISOLATION.  
  
 Повышение атрибутов инструкций ODBC до атрибутов соединения может привести к непредсказуемым результатам. Атрибуты инструкций, запрашивающие серверные курсоры для обработки результирующего набора, можно повысить до атрибутов соединения. Например, присвоение атрибуту инструкции ODBC SQL_ATTR_CONCURRENCY значения (более ограничивающего, чем SQL_CONCUR_READ_ONLY), принятое по умолчанию, указывает драйверу, что для всех инструкций соединения следует использовать динамические курсоры. Выполнение функции каталога ODBC в инструкции соединения возвращает SQL_SUCCESS_WITH_INFO и диагностическую запись, указывающую, что режим работы курсора изменился на «только чтение». Попытка выполнить инструкцию Transact-SQL SELECT, содержащую предложение COMPUTE, в том же соединении завершится ошибкой.  
  
 Драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает некоторые расширения атрибутов соединения ODBC, зависящие от драйвера и определенные в файле sqlncli.h. Драйверу ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может потребоваться установка атрибутов перед соединением, в противном случае он может пропустить атрибут, если он уже установлен. В следующей таблице приводится список ограничений.  
  
|Атрибут SQL Server|Устанавливается перед или после соединения с сервером|  
|--------------------------|----------------------------------------------|  
|SQL_COPT_SS_ANSI_NPW|До|  
|SQL_COPT_SS_APPLICATION_INTENT|До|  
|SQL_COPT_SS_ATTACHDBFILENAME|До|  
|SQL_COPT_SS_BCP|До|  
|SQL_COPT_SS_BROWSE_CONNECT|До|  
|SQL_COPT_SS_BROWSE_SERVER|До|  
|SQL_COPT_SS_CONCAT_NULL|До|  
|SQL_COPT_SS_CONNECTION_DEAD|После|  
|SQL_COPT_SS_ENCRYPT|До|  
|SQL_COPT_SS_ENLIST_IN_DTC|После|  
|SQL_COPT_SS_ENLIST_IN_XA|После|  
|SQL_COPT_SS_FALLBACK_CONNECT|До|  
|SQL_COPT_SS_FAILOVER_PARTNER|До|  
|SQL_COPT_SS_INTEGRATED_SECURITY|До|  
|SQL_COPT_SS_MARS_ENABLED|До|  
|SQL_COPT_SS_MULTISUBNET_FAILOVER|До|  
|SQL_COPT_SS_OLDPWD|До|  
|SQL_COPT_SS_PERF_DATA|После|  
|SQL_COPT_SS_PERF_DATA_LOG|После|  
|SQL_COPT_SS_PERF_DATA_LOG_NOW|После|  
|SQL_COPT_SS_PERF_QUERY|После|  
|SQL_COPT_SS_PERF_QUERY_INTERVAL|После|  
|SQL_COPT_SS_PERF_QUERY_LOG|После|  
|SQL_COPT_SS_PRESERVE_CURSORS|До|  
|SQL_COPT_SS_QUOTED_IDENT|Можно использовать|  
|SQL_COPT_SS_TRANSLATE|Можно использовать|  
|SQL_COPT_SS_TRUST_SERVER_CERTIFICATE|До|  
|SQL_COPT_SS_TXN_ISOLATION|Можно использовать|  
|SQL_COPT_SS_USE_PROC_FOR_PREP|Можно использовать|  
|SQL_COPT_SS_USER_DATA|Можно использовать|  
|SQL_COPT_SS_WARN_ON_CP_ERROR|До|  
  
 Использование атрибута предварительного соединения и эквивалентной команды [!INCLUDE[tsql](../../includes/tsql-md.md)] для одного и того же сеанса, базы данных или состояния [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может повлечь непредвиденное поведение. Например,  
  
```  
SQLSetConnectAttr(SQL_COPT_SS_QUOTED_IDENT, SQL_QI_ON) // turn ON via attribute  
SQLDriverConnect(...);  
SQLExecDirect("SET QUOTED_IDENTIFIER OFF") // turn OFF via Transact-SQL  
SQLSetConnectAttr(SQL_ATTR_CURRENT_CATALOG, ...) // restores to pre-connect attribute value  
```  

<a name="sqlcoptssansinpw"></a>
## <a name="sql_copt_ss_ansi_npw"></a>SQL_COPT_SS_ANSI_NPW  
 SQL_COPT_SS_ANSI_NPW включает или отключает использование обработки значения NULL по стандарту ISO в сравнениях, объединениях строк, заполнениях символьных типов данных и предупреждениях. Дополнительные сведения см. в описаниях команд SET ANSI_NULLS, SET ANSI_PADDING, SET ANSI_WARNINGS и SET CONCAT_NULL_YIELDS_NULL.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_AD_ON|По умолчанию. Соединение использует настройки по умолчанию ANSI для обработки сравнений NULL, дополнений, предупреждений и объединений NULL.|  
|SQL_AD_OFF|Соединение использует обработку значений NULL, заполнение символьных типов данных и предупреждения, определенные в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
 Если используется пул соединений, SQL_COPT_SS_ANSI_NPW должны быть заданы в строке подключения, а не в SQLSetConnectAttr. После установки соединения любая попытка изменить этот атрибут при использовании пула соединений завершится ошибкой без сообщений.  

<a name="sqlcoptssapplicationintent"></a>
## <a name="sql_copt_ss_application_intent"></a>SQL_COPT_SS_APPLICATION_INTENT  
 Объявляет тип рабочей нагрузки приложения при соединении с сервером. Возможные значения: **ReadOnly** и **ReadWrite**. Пример:  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_APPLICATION_INTENT, TEXT("Readonly"), SQL_NTS)  
```  
  
 Значение по умолчанию — **ReadWrite**. Дополнительные сведения о [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддержке групп доступности в собственном клиенте [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] см. в разделе [Поддержка высокой доступности и аварийного восстановления в SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  

<a name="sqlcoptssattachdbfilename"></a>
## <a name="sql_copt_ss_attachdbfilename"></a>SQL_COPT_SS_ATTACHDBFILENAME  
 SQL_COPT_SS_ATTACHDBFILENAME указывает имя первичного файла для присоединяемой базы данных. Эта база данных присоединяется и становится для соединения базой данных по умолчанию. Для использования SQL_COPT_SS_ATTACHDBFILENAME необходимо указать имя базы данных в качестве значения атрибута соединения SQL_ATTR_CURRENT_CATALOG или в параметре DATABASE = параметра [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md). Если база данных уже присоединена, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не будет повторно присоединять ее.  
  
|Значение|Описание|  
|-----------|-----------------|  
|Указатель SQLPOINTER на символьную строку|Эта строка содержит имя первичного файла для присоединяемой базы данных. Включите полное имя файла.|  

<a name="sqlcoptssbcp"></a>
## <a name="sql_copt_ss_bcp"></a>SQL_COPT_SS_BCP  
 SQL_COPT_SS_BCP включает функции массового копирования в соединение. Дополнительные сведения см. в разделе [функции для операций с массовым копированием](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md).  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_BCP_OFF|По умолчанию. Функции массового копирования недоступны в соединении.|  
|SQL_BCP_ON|Функции массового копирования доступны в соединении.|  

<a name="sqlcoptssbrowseconnect"></a>
## <a name="sql_copt_ss_browse_connect"></a>SQL_COPT_SS_BROWSE_CONNECT  
 Этот атрибут используется для настройки результирующего набора, возвращаемого функцией [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md). SQL_COPT_SS_BROWSE_CONNECT включает или отключает возвращение дополнительных сведений из перечисляемого экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Они могут включать такие сведения, как является ли сервер кластером, имена различных экземпляров и номер версии.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_MORE_INFO_NO|По умолчанию. Возвращает список серверов.|  
|SQL_MORE_INFO_YES|**SQLBrowseConnect** возвращает расширенную строку свойств сервера.|  

<a name="sqlcoptssbrowseserver"></a>
## <a name="sql_copt_ss_browse_server"></a>SQL_COPT_SS_BROWSE_SERVER  
 Этот атрибут используется для настройки результирующего набора, возвращаемого функцией **SQLBrowseConnect**. SQL_COPT_SS_BROWSE_SERVER указывает имя сервера, для которого **SQLBrowseConnect** возвращает сведения.  
  
|Значение|Описание|  
|-----------|-----------------|  
|computername|**SQLBrowseConnect** возвращает список экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на указанном компьютере. В имени сервера не следует использовать двойную косую черту ( \\ \\ ) (например, вместо \\ \мисервер, следует использовать MyServer).|  
|NULL|По умолчанию. **SQLBrowseConnect** возвращает сведения обо всех серверах в домене.|  

<a name="sqlcoptssconcatnull"></a>
## <a name="sql_copt_ss_concat_null"></a>SQL_COPT_SS_CONCAT_NULL  
 SQL_COPT_SS_CONCAT_NULL включает или выключает обработку значений NULL согласно стандарту ISO при объединении строк. Дополнительные сведения см. в разделе SET CONCAT_NULL_YIELDS_NULL.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_CN_ON|По умолчанию. Соединение использует поведение ISO по умолчанию для обработки значений NULL при объединении строк.|  
|SQL_CN_OFF|Соединение использует поведение, определенное в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], для обработки значений NULL при объединении строк.|  

<a name="sqlcoptssencrypt"></a>
## <a name="sql_copt_ss_encrypt"></a>SQL_COPT_SS_ENCRYPT  
 Управляет шифрованием соединения.  
  
 Шифрование использует сертификат, находящийся на сервере. Он должен быть заверен центром сертификации, если атрибут соединения SQL_COPT_SS_TRUST_SERVER_CERTIFICATE не имеет значение SQL_TRUST_SERVER_CERTIFICATE_YES или строка соединения не содержит «TrustServerCertificate=yes». Если соблюдается одно из этих условий, то при отсутствии сертификата на сервере для шифрования соединения может использоваться сертификат, созданный и подписанный сервером.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_EN_ON|Соединение будет зашифрованным.|  
|SQL_EN_OFF|Соединение не будет зашифрованным. Это значение по умолчанию.|  

<a name="sqlcoptssenlistindtc"></a>
## <a name="sql_copt_ss_enlist_in_dtc"></a>SQL_COPT_SS_ENLIST_IN_DTC  
 Клиент вызывает метод Microsoft координатор распределенных транзакций (MS DTC) OLE DB **ITransactionDispenser:: BeginTransaction** , чтобы начать ТРАНЗАКЦИЮ MS DTC и создать объект транзакции MS DTC, представляющий транзакцию. Затем приложение вызывает **SQLSetConnectAttr** с параметром SQL_COPT_SS_ENLIST_IN_DTC, чтобы связать объект транзакции с соединением ODBC. Все связанные действия базы данных будут выполняться под защитой транзакции MS DTC. Приложение вызывает **SQLSetConnectAttr** с SQL_DTC_DONE, чтобы завершить связь DTC подключения.  
  
|Значение|Описание|  
|-----------|-----------------|  
|Объект DTC*|Объект транзакции MS DTC OLE, который задает экспорт транзакции в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SQL_DTC_DONE|Отделяет конец транзакции DTC.|  

<a name="sqlcoptssenlistinxa"></a>
## <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA  
 Чтобы начать транзакцию XA с XA-совместимым обработчиком транзакций (TP), клиент вызывает функцию Open Group **tx_begin** . Затем приложение вызывает **SQLSetConnectAttr** с параметром SQL_COPT_SS_ENLIST_IN_XA, равным true, чтобы связать транзакцию XA с соединением ODBC. Все связанные действия базы данных будут выполняться под защитой XA-транзакции. Чтобы завершить связь XA с подключением ODBC, клиент должен вызвать **SQLSetConnectAttr** с параметром SQL_COPT_SS_ENLIST_IN_XA false. Дополнительные сведения см. в документации по координатору распределенных транзакций (Майкрософт).  
  
## <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT  
 Этот атрибут больше не поддерживается.  

<a name="sqlcoptssfailoverpartner"></a>
## <a name="sql_copt_ss_failover_partner"></a>SQL_COPT_SS_FAILOVER_PARTNER  
 Применяется для указания или получения имени участника отработки отказа, используемого в зеркальном отображении базы данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], и является символьной строкой, заканчивающейся символом NULL, которая должна устанавливаться до первоначального соединения с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 После создания соединения приложение может запросить этот атрибут с помощью [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) , чтобы определить идентификатор партнера по обеспечению отработки отказа. Если сервер-источник не имеет партнера по обеспечению отработки отказа, то это свойство вернет пустую строку. Это позволяет приложению кэшировать последний определенный резервный сервер, но при этом необходимо учитывать, что данные обновляются только при первоначальной установке соединения или его восстановлении (при наличии пула), поэтому они могут устареть, если соединение поддерживается длительное время.  
  
 Дополнительные сведения см. в статье [Зеркальное отображение базы данных](../../relational-databases/native-client/features/using-database-mirroring.md).  

<a name="sqlcoptssintegratedsecurity"></a>
## <a name="sql_copt_ss_integrated_security"></a>SQL_COPT_SS_INTEGRATED_SECURITY  
 SQL_COPT_SS_INTEGRATED_SECURITY задает принудительное использование проверки подлинности Windows для проверки доступа по имени входа сервера. При использовании проверки подлинности Windows драйвер не учитывает значения идентификатора пользователя и пароля, указанные в процессе обработки **SQLConnect**, [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md)или [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) .  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_IS_OFF|По умолчанию. Для проверки идентификатора и пароля для имени входа используется проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|SQL_IS_ON|Для проверки прав доступа пользователя к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используется проверка подлинности Windows.|  

<a name="sqlcoptssmarsenabled"></a>
## <a name="sql_copt_ss_mars_enabled"></a>SQL_COPT_SS_MARS_ENABLED  
 Этот атрибут включает или отключает режим MARS. По умолчанию режим MARS отключен. Атрибут должен быть установлен до соединения с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. После открытия соединения с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] режим MARS останется включенным или отключенным до закрытия соединения.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_MARS_ENABLED_NO|По умолчанию. Режим MARS отключен.|  
|SQL_MARS_ENABLED_YES|Режим MARS включен.|  
  
 Дополнительные сведения о режиме MARS см. [в разделе Использование нескольких активных результирующих наборов &#40;режима mars&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  

<a name="sqlcoptssmultisubnetfailover"></a>
## <a name="sql_copt_ss_multisubnet_failover"></a>SQL_COPT_SS_MULTISUBNET_FAILOVER  
 Если приложение соединяется с группой высокого уровня доступности [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] в разных подсетях, это свойство соединения конфигурирует собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для поддержки более быстрого поиска активного (в настоящее время) сервера и соединения с ним. Пример:  
  
```  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_MULTISUBNET_FAILOVER, SQL_IS_ON, SQL_IS_INTEGER)  
```  
  
 Дополнительные сведения о [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддержке групп доступности в собственном клиенте [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] см. в разделе [Поддержка высокой доступности и аварийного восстановления в SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_IS_ON|Собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обеспечивает более быстрое восстановление в случае отработки отказа.|  
|SQL_IS_OFF|Собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не обеспечивает более быстрое восстановление в случае отработки отказа.|  

<a name="sqlcoptssoldpwd"></a>
## <a name="sql_copt_ss_oldpwd"></a>SQL_COPT_SS_OLDPWD  
 Истечение срока действия пароля для проверки подлинности SQL Server было введено в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Добавлен атрибут SQL_COPT_SS_OLDPWD, чтобы клиент мог предоставлять как старый, так и новый пароль для соединения. Если задано это свойство, поставщик не будет использовать пул соединений для первого и последующих соединений, поскольку строка подключения будет содержать «старый пароль», который уже изменен.  
  
 Дополнительные сведения см. в статье [Смена пароля программным способом](../../relational-databases/native-client/features/changing-passwords-programmatically.md).  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_COPT_SS_OLD_PASSWORD|Указатель SQLPOINTER на символьную строку, содержащую старый пароль. Это значение предназначено только для записи и должно устанавливаться перед соединением с сервером.|  

<a name="sqlcoptssperfdata"></a>
## <a name="sql_copt_ss_perf_data"></a>SQL_COPT_SS_PERF_DATA  
 SQL_COPT_SS_PERF_DATA начинает или останавливает запись данных в журнал. Имя файла журнала данных необходимо установить до начала записи в журнал. См. SQL_COPT_SS_PERF_DATA_LOG ниже.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_PERF_START|Начинает выборку сведений о производительности.|  
|SQL_PERF_STOP|Останавливает счетчики производительности.|  
  
 Дополнительные сведения см. в разделе [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  

<a name="sqlcoptssperfdatalog"></a>
## <a name="sql_copt_ss_perf_data_log"></a>SQL_COPT_SS_PERF_DATA_LOG  
 SQL_COPT_SS_PERF_DATA_LOG присваивает имя файлу журнала, который используется для регистрации сведений о производительности. Имя файла журнала представляет собой строку, оканчивающуюся нулевым байтом в кодировке ANSI или в Юникоде, в зависимости от параметров компиляции приложения. Аргумент *StringLength* должен быть SQL_NTS.  

<a name="sqlcoptssperfdatalognow"></a>
## <a name="sql_copt_ss_perf_data_log_now"></a>SQL_COPT_SS_PERF_DATA_LOG_NOW  
 SQL_COPT_SS_PERF_DATA_LOG_NOW дает указание драйверу записать на диск запись журнала статистики. Аргумент *StringLength* должен быть SQL_NTS.  

<a name="sqlcoptssperfquery"></a>
## <a name="sql_copt_ss_perf_query"></a>SQL_COPT_SS_PERF_QUERY  
 SQL_COPT_SS_PERF_QUERY начинает или останавливает ведение журнала для длительных запросов. Имя файла журнала запроса необходимо задать до начала записи в журнал. Приложение может определить «долгое выполнение», установив интервал ведения журнала.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_PERF_START|Начинает ведение журнала для длительных запросов.|  
|SQL_PERF_STOP|Останавливает ведение журнала для длительных запросов.|  
  
 Дополнительные сведения см. в разделе [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  

<a name="sqlcoptssperfqueryinterval"></a>
## <a name="sql_copt_ss_perf_query_interval"></a>SQL_COPT_SS_PERF_QUERY_INTERVAL  
 SQL_COPT_SS_PERF_QUERY_INTERVAL устанавливает пороговое значение ведения журнала запроса в миллисекундах. Запросы, превышающие это значение, регистрируются в журнале долго выполняющихся запросов. Верхний предел порогового значения отсутствует. Пороговое значение запроса, равное нулю, приводит к ведению журнала для всех запросов.  

<a name="sqlcoptssperfquerylog"></a>
## <a name="sql_copt_ss_perf_query_log"></a>SQL_COPT_SS_PERF_QUERY_LOG  
 SQL_COPT_SS_PERF_QUERY_LOG присваивает имя файлу журнала для записи данных долго выполняющихся запросов. Имя файла журнала представляет собой строку, оканчивающуюся нулевым байтом в кодировке ANSI или в Юникоде, в зависимости от параметров компиляции приложения. Аргумент *StringLength* должен иметь SQL_NTS или длину строки в байтах.  

<a name="sqlcoptsspreservecursors"></a>
## <a name="sql_copt_ss_preserve_cursors"></a>SQL_COPT_SS_PRESERVE_CURSORS  
 Этот атрибут позволяет запрашивать и устанавливать независимо от того, сохраняет ли соединение курсоры при фиксации или откате транзакции. Значение имеет вид SQL_PC_ON или SQL_PC_OFF. По умолчанию имеет значение SQL_PC_OFF. Этот параметр определяет, будет ли драйвер закрывать курсоры при вызове [SQLEndTran](../../relational-databases/native-client-odbc-api/sqlendtran.md) (или SQLTransact).  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_PC_OFF|По умолчанию. Курсоры закрываются при фиксации или откате транзакции с помощью **SQLEndTran**.|  
|SQL_PC_ON|Курсоры не закрываются при фиксации или откате транзакции с помощью **SQLEndTran**, за исключением случаев, когда в асинхронном режиме используется указатель на статический или ключевой объект. Если выполняется откат, когда курсора не заполнен до конца, курсор закрывается.|  

<a name="sqlcoptssquotedident"></a>
## <a name="sql_copt_ss_quoted_ident"></a>SQL_COPT_SS_QUOTED_IDENT  
 SQL_COPT_SS_QUOTED_IDENT позволяет использовать в соединении заключенные в кавычки идентификаторы инструкций ODBC и Transact-SQL. Предоставляя заключенные в кавычки идентификаторы, драйвер ODBC для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] дает возможность использовать имена объектов, содержащих пробел в идентификаторе, например «My Table». Дополнительные сведения см. в разделе SET QUOTED_IDENTIFIER.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_QI_OFF|Соединение [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не допускает идентификаторы в кавычках в инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|SQL_QI_ON|По умолчанию. Соединение допускает идентификаторы в кавычках в инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)].|  

<a name="sqlcoptsstranslate"></a>
## <a name="sql_copt_ss_translate"></a>SQL_COPT_SS_TRANSLATE  
 SQL_COPT_SS_TRANSLATE заставляет драйвер преобразовывать символы между кодовыми страницами клиента и сервера при обмене данными MBCS. Атрибут влияет только на данные, хранящиеся в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбцах **char**, **varchar** и **Text** .  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_XL_OFF|Драйвер не преобразует символы из одной кодовой страницы в другую для символьных данных, передаваемых между клиентом и сервером.|  
|SQL_XL_ON|По умолчанию. Драйвер преобразует символы из одной кодовой страницы в другую для символьных данных, передаваемых между клиентом и сервером. Драйвер определяет, какая кодовая страница установлена на сервере и какая используется клиентом, а затем автоматически настраивает преобразование знаков.|  

<a name="sqlcoptsstrustservercertificate"></a>
## <a name="sql_copt_ss_trust_server_certificate"></a>SQL_COPT_SS_TRUST_SERVER_CERTIFICATE  
 SQL_COPT_SS_TRUST_SERVER_CERTIFICATE заставляет драйвер включать или отключать проверку сертификата при использовании шифрования. Атрибут является значением, предназначенным для чтения и записи, но его установка после открытия соединения не имеет действия.  
  
 Клиентские приложения могут запрашивать это свойство после открытия соединения для получения используемых параметров шифрования и проверки.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_TRUST_SERVER_CERTIFICATE_NO|По умолчанию. Шифрование без сертификата не включено.|  
|SQL_TRUST_SERVER_CERTIFICATE_YES|Шифрование без сертификата включено.|  

<a name="sqlcoptsstxnisolation"></a>
## <a name="sql_copt_ss_txn_isolation"></a>SQL_COPT_SS_TXN_ISOLATION  
 SQL_COPT_SS_TXN_ISOLATION устанавливает специальный атрибут изоляции моментальных снимков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Изоляцию моментальных снимков нельзя установить с помощью SQL_ATTR_TXN_ISOLATION, поскольку его значение поддерживается только [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Однако его можно получить, используя SQL_ATTR_TXN_ISOLATION или SQL_COPT_SS_TXN_ISOLATION.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_TXN_SS_SNAPSHOT|Указывает, что из одной транзакции нельзя увидеть изменения, сделанные в другой транзакции. Изменения нельзя увидеть даже с помощью повторных запросов.|  
  
 Дополнительные сведения об изоляции моментальных снимков см. [в разделе Работа с изоляцией моментальных снимков](../../relational-databases/native-client/features/working-with-snapshot-isolation.md).  
  
## <a name="sql_copt_ss_use_proc_for_prep"></a>SQL_COPT_SS_USE_PROC_FOR_PREP

 Этот атрибут больше не поддерживается.  

<a name="sqlcoptssuserdata"></a>
## <a name="sql_copt_ss_user_data"></a>SQL_COPT_SS_USER_DATA  
 SQL_COPT_SS_USER_DATA устанавливает указатель на пользовательские данные. Пользовательские данные — данные соединения, хранящиеся в памяти клиента.  
  
 Дополнительные сведения см. в разделе [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  

<a name="sqlcoptsswarnoncperror"></a>
## <a name="sql_copt_ss_warn_on_cp_error"></a>SQL_COPT_SS_WARN_ON_CP_ERROR  
 Этот атрибут определяет, будет ли выдано предупреждение при потере данных во время преобразования кодовых страниц. Относится только к данным, поступающим с сервера.  
  
|Значение|Описание|  
|-----------|-----------------|  
|SQL_WARN_YES|Создание предупреждений при потере данных во время преобразования кодовых страниц.|  
|SQL_WARN_NO|(По умолчанию) Не формировать предупреждения при потере данных во время преобразования кодовых страниц.|  
  
## <a name="sqlsetconnectattr-support-for-service-principal-names-spns"></a>Поддержка функции SQLSetConnectAttr применительно к именам участников-служб (SPN)

 SQLSetConnectAttr можно использовать для установки значений новых атрибутов соединения SQL_COPT_SS_SERVER_SPN и SQL_COPT_SS_FAILOVER_PARTNER_SPN. Эти атрибуты нельзя устанавливать при открытом соединении, в противном случае возвращается ошибка HY011 с сообщением «В настоящее время эта операция недопустима». (Для установки этих значений также можно использовать SQLSetConnectOption.)  
  
 Дополнительные сведения о SPN см. [в статье имена субъектов-служб &#40;имен участников-служб&#41; в клиентских подключениях &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md).  

<a name="sqlcoptssconnectiondead"></a>
## <a name="sql_copt_ss_connection_dead"></a>SQL_COPT_SS_CONNECTION_DEAD  
 Этот атрибут доступен только для чтения.  
  
 Дополнительные сведения о SQL_COPT_SS_CONNECTION_DEAD см. в разделе [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) и [Подключение к источнику данных &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-communication/connecting-to-a-data-source-odbc.md).  
  
## <a name="example"></a>Пример

 В этом примере данные производительности записываются в журнал.  
  
```  
SQLPERF*     pSQLPERF;  
SQLINTEGER   nValue;  
  
// See if you are already logging. SQLPERF* will be NULL if not.  
SQLGetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA, &pSQLPERF,  
    sizeof(SQLPERF*), &nValue);  
  
if (pSQLPERF == NULL)  
    {  
    // Set the performance log file name.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG,  
        (SQLPOINTER) "\\My LogDirectory\\MyServerLog.txt", SQL_NTS);  
  
    // Start logging...  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
        (SQLPOINTER) SQL_PERF_START, SQL_IS_INTEGER);  
    }  
else  
    {  
    // Take a snapshot now so that your performance statistics are discernible.  
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
    }  
  
    // ...perform some action...  
  
// ...take a performance data snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
    // ...perform more actions...  
  
// ...take another snapshot...  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA_LOG_NOW, NULL, 0);  
  
// ...and disable logging.  
SQLSetConnectAttr(hDbc, SQL_COPT_SS_PERF_DATA,  
    (SQLPOINTER) SQL_PERF_STOP, SQL_IS_INTEGER);  
  
// Continue on...  
```  
  
## <a name="see-also"></a>См. также:

 [Функция SQLSetConnectAttr](../../odbc/reference/syntax/sqlsetconnectattr-function.md)   
 [Сведения о реализации API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [Функции с массовым копированием](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)   
 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)   
 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)   
 [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md)   
 [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)   
 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)   
 [Функция SQLPrepare](../../odbc/reference/syntax/sqlprepare-function.md)   
 [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md)  
  

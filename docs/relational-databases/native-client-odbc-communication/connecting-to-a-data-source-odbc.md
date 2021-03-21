---
description: Соединение с источником данных (ODBC)
title: Подключение к источнику данных (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- checking connection states
- ODBC data sources, connections
- data sources [SQL Server Native Client]
- SQLBrowseConnect function
- ODBC applications, connections
- ODBC applications, data sources
- connections [SQL Server Native Client]
- SQLConnect function
- SQLDriveConnect function
- verifying connection states
- SQL Server Native Client ODBC driver, data sources
- SQL Server Native Client ODBC driver, connections
ms.assetid: ae30dd1d-06ae-452b-9618-8fd8cd7ba074
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 97bcd6e94d77f6c4fe76426ac25d6d21c60282b3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756364"
---
# <a name="connecting-to-a-data-source-odbc"></a>Соединение с источником данных (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  После определения среды и дескрипторов соединения, а также установки любых атрибутов соединения приложение устанавливает соединение с источником данных или драйвером. Существует три функции, которые можно использовать для установки соединения:  
  
-   **SQLConnect**  
  
-   **SQLDriverConnect**  
  
-   **SQLBrowseConnect**  
  
 Дополнительные сведения о подключении к источнику данных, включая различные доступные параметры строки подключения, см. в разделе [Использование ключевых слов строки подключения с SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
## <a name="sqlconnect"></a>SQLConnect  
 **SQLConnect** — это простейшая функция подключения. Она принимает три параметра: имя источника данных, идентификатор пользователя и пароль. Используйте **SQLConnect** , если эти три параметра содержат всю информацию, необходимую для подключения к базе данных. Для этого создайте список источников данных с помощью **SQLDataSources**; запрашивать у пользователя источник данных, идентификатор пользователя и пароль; затем вызовите **SQLConnect**.  
  
 **SQLConnect** предполагает, что имя источника данных, идентификатор пользователя и пароль достаточно для подключения к источнику данных и что источник данных ODBC содержит все остальные сведения, необходимые драйверу ODBC для подключения. В отличие от [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) и [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md), **SQLConnect** не использует строку подключения.  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
 **SQLDriverConnect** используется, если требуется получить больше информации, чем имя источника данных, идентификатор пользователя и пароль. Одним из параметров **SQLDriverConnect** является строка подключения, содержащая сведения, относящиеся к драйверу. Вы можете использовать **SQLDriverConnect** вместо **SQLConnect** по следующим причинам:  
  
-   для указания специфической для драйвера информации во время подключения;  
  
-   для запроса, который драйвер направляет пользователю для получения информации о соединении;  
  
-   для соединения без использования источника данных ODBC.  
  
 Строка подключения **SQLDriverConnect** содержит ряд пар "ключевое слово-значение", которые указывают все сведения о соединении, поддерживаемые драйвером ODBC. Каждый драйвер поддерживает стандартные ключевые слова ODBC (DSN, FILEDSN, DRIVER, UID, PWD и SAVEFILE) в дополнение к специальным ключевым словам драйвера для указания всей информации о соединении, поддерживаемой драйвером. **SQLDriverConnect** можно использовать для подключения без источника данных. Например, приложение, которое предназначено для создания подключения "без DSN" к экземпляру, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может вызывать **SQLDriverConnect** со строкой подключения, определяющей идентификатор входа, пароль, сетевую библиотеку, имя сервера для подключения и используемую базой данных по умолчанию.  
  
 При использовании **SQLDriverConnect** существует два варианта запроса сведений о подключении для пользователя.  
  
-   Диалоговое окно приложения  
  
     Можно создать диалоговое окно приложения, в котором запрашиваются сведения о соединении, а затем вызывает **SQLDriverConnect** с нулевым маркером окна, а *DriverCompletion* — значением SQL_DRIVER_NOPROMPT. Эти параметры предотвращают открытие драйвером ODBC собственного диалогового окна. Этот метод используется, когда важно управлять пользовательским интерфейсом приложения.  
  
-   Диалоговое окно драйвера  
  
     Можно создать код приложения, чтобы передать допустимый обработчик окна в **SQLDriverConnect** и установить параметр *DriverCompletion* в значение SQL_DRIVER_COMPLETE, SQL_DRIVER_PROMPT или SQL_DRIVER_COMPLETE_REQUIRED. Затем драйвер формирует диалоговое окно для получения от пользователя информации о соединении. Этот метод упрощает код приложения.  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
 **SQLBrowseConnect**, например **SQLDriverConnect**, использует строку подключения. Однако с помощью **SQLBrowseConnect** приложение может итеративно создавать полную строку соединения с источником данных во время выполнения. Это позволяет приложению:  
  
-   строить собственные диалоговые окна для запроса этой информации, сохраняя таким образом управление своим пользовательским интерфейсом;  
  
-   просматривать систему в поисках источников данных, которые может использовать конкретный драйвер, возможно, за несколько шагов.  
  
     Например, пользователь может сначала просмотреть серверы в сети, а после выбора сервера с помощью драйвера просмотреть доступные базы данных этого сервера.  
  
 Когда **SQLBrowseConnect** завершает успешное подключение, он возвращает строку подключения, которую можно использовать при последующих вызовах **SQLDriverConnect**.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Драйвер ODBC для собственного клиента всегда возвращает SQL_SUCCESS_WITH_INFO для успешных **SQLConnect**, **SQLDriverConnect** или **SQLBrowseConnect**. Когда приложение ODBC вызывает **SQLGetDiagRec** после получения SQL_SUCCESS_WITH_INFO, оно может получать следующие сообщения:  
  
 5701  
 Показывает, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] помещает пользовательский контекст в базу данных по умолчанию, которая определена в источнике данных, или в базу данных, определенную для идентификатора входа, который использовался в соединении, если источник данных не имеет базы данных по умолчанию.  
  
 5703  
 Обозначает язык, используемый на сервере.  
  
 В следующих примерах показано сообщение, которое возвращается системным администратором при успешном соединении:  
  
```  
szSqlState = "01000", *pfNativeError = 5701,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
       Changed database context to 'pubs'."  
szSqlState = "01000", *pfNativeError = 5703,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
       Changed language setting to 'us_english'."  
```  
  
 Можно не обрабатывать сообщения 5701 и 5703; они всего лишь информационные. Однако не следует пропускать код возврата SQL_SUCCESS_WITH_INFO, так как сообщения, отличные от 5701 и 5703, могут быть возвращены. Например, если драйвер подключается к серверу, на котором выполняется экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с устаревшими хранимыми процедурами каталога, одна из ошибок, возвращаемых через **SQLGetDiagRec** после SQL_SUCCESS_WITH_INFO:  
  
```  
SqlState:   01000  
pfNative:   0  
szErrorMsg: "[Microsoft][SQL Server Native Client]The ODBC  
            catalog stored procedures installed on server  
            my65server are version 06.50.0193; version 07.00.0205  
            or later is required to ensure proper operation.  
            Please contact your system administrator."  
```  
  
 Функция обработки ошибок приложения для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подключений должна вызывать **SQLGetDiagRec** до тех пор, пока не вернет SQL_NO_DATA. Затем он должен работать с любыми сообщениями, кроме тех, с *pfNative* кодом 5701 или 5703.  
  
## <a name="see-also"></a>См. также:  
 [Взаимодействие с SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  

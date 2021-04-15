---
title: Поддержка высокой доступности и аварийного восстановления в драйвере ODBC для Linux и macOS
description: Узнайте, как Microsoft ODBC Driver для Linux и macOS поддерживает группы доступности Always On.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: fa656c5b-a935-40bf-bc20-e517ca5cd0ba
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8183d7d99bd3ec9ffa344f3b813d17eca0dac85b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "107490078"
---
# <a name="odbc-driver-on-linux-and-macos-support-for-high-availability-and-disaster-recovery"></a>Поддержка высокой доступности и аварийного восстановления в драйвере ODBC для Linux и macOS
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Драйверы ODBC для Linux и macOS поддерживают [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]. Дополнительные сведения о [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)] см. в следующих статьях.  
  
-   [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)  
  
-   [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)  
  
-   [Отказоустойчивая кластеризация и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)  
  
-   [Активные вторичные реплики: доступ только для чтения к вторичным репликам (группы доступности Always On)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)  
  
Прослушиватель для заданной группы доступности можно задать в строке подключения. Если приложение ODBC в Linux или macOS подключено к базе данных в группе доступности, которая выполняет переход на другой ресурс, то исходное соединение разрывается, а приложение должно установить новое соединение, чтобы продолжить работу после отработки отказа.

Если вы не подключаетесь к прослушивателю группы доступности, а с именем узла связано множество IP-адресов, то драйвер ODBC для Linux или macOS последовательно перебирает все IP-адреса, связанные с именем узла DNS.

Это может занять много времени, если первый IP-адрес, возвращенный DNS-сервером, недоступен. При подключении к прослушивателю группы доступности драйвер пытается установить соединения ко всем IP-адресам в параллельном режиме. Если попытка соединения завершается успешно, драйвер отменяет все ожидающие попытки подключения.

> [!NOTE]  
> В связи с возможностью неудачного подключения при отработке отказа группы доступности следует реализовать логику повторного соединения, обеспечивающую неограниченное число попыток соединения до достижения успеха. Увеличение времени ожидания соединения и реализация логики повторного соединения позволяют повысить вероятность соединения с группой доступности.

## <a name="connecting-with-multisubnetfailover"></a>Соединение с помощью MultiSubnetFailover

Всегда указывайте **MultiSubnetFailover=Yes** при соединении с прослушивателем группы доступности [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] или экземпляром отказоустойчивого кластера [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. **MultiSubnetFailover** обеспечивает ускоренную отработку отказа для всех групп доступности и экземпляра отказоустойчивого кластера в [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. **MultiSubnetFailover** также значительно сокращает время отработки отказа для топологий AlwaysOn с одной подсетью или несколькими. При отработке отказа в сети с несколькими подсетями клиент пытается установить соединения параллельно. При отработке отказа драйвер агрессивно повторяет попытки соединения по протоколу TCP.

Свойство соединения **MultiSubnetFailover** указывает, что приложение развертывается в группе доступности или экземпляре отказоустойчивого кластера. Драйвер пытается подключиться к базе данных на основном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], пытаясь подключиться ко всем IP-адресам. Если соединение осуществляется с параметром **MultiSubnetFailover=Yes**, клиент повторяет попытку соединения по протоколу TCP быстрее, чем истекают интервалы ожидания до пересылки по TCP операционной системы по умолчанию. **MultiSubnetFailover = Yes** позволяет ускорить восстановление соединения после отработки отказа группы доступности AlwaysOn или экземпляра отказоустойчивого кластера AlwaysOn. **MultiSubnetFailover=Yes** применяется к группам доступности и экземплярам отказоустойчивого кластера как с одной подсетью, так и с несколькими.  

Используйте **MultiSubnetFailover=Yes** при соединении с прослушивателем группы доступности или экземпляром отказоустойчивого кластера. В противном случае производительность приложения может снизиться.

При подключении к серверу в группе доступности или экземпляру отказоустойчивого кластера следуйте приведенным ниже рекомендациям.
  
-   Укажите **MultiSubnetFailover=Yes**, чтобы повысить производительность при подключении к группе доступности в одной или нескольких подсетях.

-   Укажите прослушиватель группы доступности в строке подключения вместо сервера.
  
-   Нельзя подключиться к экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], для которого настроено более 64 IP-адресов.

-   С параметром **MultiSubnetFailover=Yes** можно использовать как проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], так и проверку подлинности Kerberos — на работу приложения это не повлияет.

-   Значение **loginTimeout** можно увеличить с учетом времени отработки отказа, это уменьшит количество попыток повторного соединения в приложении.

-   Распределенные транзакции не поддерживаются.  
  
Если маршрутизация только для чтения неактивна, то подключение к расположению вторичной реплики в группе доступности завершается ошибкой в следующих случаях:  
  
1.  Если местоположение вторичных реплик не настроено для приема подключений.  
  
2.  Если приложение использует **ApplicationIntent=ReadWrite** и расположение вторичных реплик настроено для доступа только для чтения.  
  
При соединении происходит ошибка, если первичная реплика настроена на отклонение рабочих нагрузок только для чтения, а строка подключения содержит **ApplicationIntent=ReadOnly**.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="odbc-syntax"></a>Синтаксис ODBC

Два ключевых слова строки подключения ODBC поддерживают [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)].  
  
-   **ApplicationIntent**  
  
-   **MultiSubnetFailover**  
  
Дополнительные сведения о ключевых словах строки подключения ODBC см. в статье [Использование ключевых слов строки подключения с SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
Эквивалентными атрибутами подключения являются следующие:
  
-   **SQL_COPT_SS_APPLICATION_INTENT**  
  
-   **SQL_COPT_SS_MULTISUBNET_FAILOVER**  
  
Дополнительные сведения об атрибутах соединений ODBC см. в статье [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md).  
  
Приложение ODBC, которое использует [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)], может использовать одну из двух указанных далее функций для установки соединения.  
  
|Компонент|Описание|  
|------------|---------------|  
|[Функция SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|**SQLConnect** поддерживает как **ApplicationIntent**, так и **MultiSubnetFailover**, через имя источника данных (DSN) или атрибут соединения.|  
|[Функция SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|**SQLDriverConnect** поддерживает **ApplicationIntent** и **MultiSubnetFailover** через ключевое слово строки подключения, атрибут соединения или имя DSN.|
  
## <a name="see-also"></a>См. также:  

[Ключевые слова строки подключения и имена источников данных (DSN)](../../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)

[Указания по программированию](../../../connect/odbc/linux-mac/programming-guidelines.md)

[Заметки о выпуске](../../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)  

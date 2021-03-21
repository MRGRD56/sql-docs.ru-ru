---
title: Поддержка высокого уровня доступности и аварийного восстановления в драйвере OLE DB для SQL Server
description: Сведения о поддержке высокой доступности и аварийного восстановления драйвером OLE DB Driver for SQL Server при подключении к базам данных с соответствующими настроенными компонентами.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 819e81cd1d1b09d866901b54cc02341ef3e200cf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742574"
---
# <a name="ole-db-driver-for-sql-server-support-for-high-availability-disaster-recovery"></a>Поддержка высокого уровня доступности и аварийного восстановления в драйвере OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В статье осуждается поддержка *OLE DB Driver for SQL Server* для [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Дополнительные сведения о [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] см. в разделах [Прослушиватели групп доступности, возможность подключения клиентов и отработка отказа приложений (SQL Server)](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Создание и настройка групп доступности (SQL Server)](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Отказоустойчивая кластеризация и группы доступности AlwaysOn (SQL Server)](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) и [Активные вторичные реплики. Доступ только для чтения ко вторичным репликам (группы доступности AlwaysOn)](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 Прослушиватель для заданной группы доступности можно задать в строке подключения. Если приложение драйвера OLE DB для SQL Server подключено к базе данных в группе доступности, которая выполняет переход на другой ресурс, то исходное соединение разрывается, а приложение должно установить новое соединение, чтобы продолжить работу после отработки отказа.  
  
 Если вы не подключаетесь к прослушивателю группы доступности, а с именем узла связано множество IP-адресов, то драйвер OLE DB для SQL Server последовательно переберет все IP-адреса, связанные с записью DNS. Это может занять много времени, если первый IP-адрес, возвращенный DNS-сервером, не привязан ни к одной из сетевых интерфейсных плат. При подключении к прослушивателю группы доступности драйвер OLE DB для SQL Server пытается установить подключение ко всем IP-адресам параллельно, и если одна из попыток окажется успешной, драйвер отменит остальные ожидающие попытки подключения.  
  
> [!NOTE]  
> Увеличение времени ожидания соединения и реализация логики повторного соединения позволяют повысить вероятность соединения приложения с группой доступности. Кроме того, в связи с возможностью неудачного подключения при отработке отказа группы доступности следует реализовать логику повторного соединения, обеспечивающую неограниченное число попыток соединения до достижения успеха.  
  
## <a name="connecting-with-multisubnetfailover"></a>Соединение с помощью MultiSubnetFailover  
 При установлении соединения с прослушивателем группы доступности AlwaysOn Microsoft SQL Server или экземпляром отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] всегда необходимо указывать **MultiSubnetFailover=Yes**. **MultiSubnetFailover** позволяет группам доступности AlwaysOn и экземплярам отказоустойчивого кластера в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] быстрее выполнить отработку отказа, а также значительно сократить время перехода на другой ресурс для топологий AlwaysOn с одной подсетью или несколькими. При отработке отказа с в нескольких подсетях клиент будет выполнять попытки соединения параллельно. Во время отработки отказа подсети OLE DB Driver for SQL Server повторит попытку подключения по протоколу TCP.  
  
 Свойство подключения **MultiSubnetFailover** указывает, что приложение развертывается в группе доступности или экземпляре отказоустойчивого кластера, а также что драйвер OLE DB для SQL Server попытается подключиться ко всем IP-адресам базы данных в первичном экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Когда для соединения установлено свойство **MultiSubnetFailover=Yes**, то клиент производит повторные попытки установить TCP-соединение быстрее интервалов повторной отправки TCP-пакетов по умолчанию для операционной системы. Это позволяет ускорить восстановление соединения после отработки отказа в группе доступности AlwaysOn или в экземпляре отказоустойчивого кластера; метод может применяться к группам доступности и экземплярам отказоустойчивых кластеров как с одной подсетью, так и с несколькими.  
  
 Дополнительные сведения о ключевых словах строки подключения см. в статье [Использование ключевых слов строки подключения с драйвером OLE DB для SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 Указание параметра **MultiSubnetFailover=Yes** при соединении с объектом, отличным от прослушивателя группы доступности или экземпляра отказоустойчивого кластера, может привести к снижению производительности и не поддерживается.  
  
 Следуйте приведенным ниже рекомендациям для подключения к серверу в группе доступности или экземпляру отказоустойчивого кластера.  
  
-   Пользуйтесь свойством соединения **MultiSubnetFailover** при установлении соединения с одной или несколькими подсетями, это благоприятно скажется на производительности в любом случае.  
  
-   Чтобы установить соединение с группой доступности, указывайте ее прослушиватель в строке подключения вместо сервера.  
  
-   При установлении соединения с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], настроенным на работу с более чем 64 IP-адресами, будет возникать ошибка соединения.  
  
-   Режим работы приложения, использующего свойство соединения **MultiSubnetFailover**, не зависит от типа проверки подлинности — [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], Kerberos или Windows.  
  
-   Значение **loginTimeout** можно увеличить с учетом времени отработки отказа, это уменьшит количество попыток повторного соединения в приложении.  
  
-   Распределенные транзакции не поддерживаются.  
  
Если маршрутизация только для чтения неактивна, то подключение к местоположению вторичной реплики в группе доступности завершится ошибкой в следующих случаях:  
  
1.  Если местоположение вторичных реплик не настроено для приема подключений.  
  
2.  Если приложение использует **ApplicationIntent=ReadWrite** (описано ниже) и расположение вторичных реплик настроено для доступа только для чтения.  
  
При соединении произойдет ошибка, если первичная реплика настроена для отклонения рабочих нагрузок только для чтения, а строка подключения содержит **ApplicationIntent=ReadOnly**.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Переход с зеркального отображения базы данных на использование кластеров с несколькими подсетями  
Если в строке подключения содержатся ключевые слова **MultiSubnetFailover** и **Failover_Partner**, то при соединении произойдет ошибка. Ошибка возникает также в том случае, если используется **MultiSubnetFailover** и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] возвращает ответ партнера по обеспечению отработки отказа, указывающий на то, что он является частью пары зеркальных баз данных.  
  
При переводе приложения драйвера OLE DB для SQL Server, которое сейчас использует зеркальное отображение базы данных, на сценарий с использованием множества подсетей необходимо удалить свойство подключения **Failover_Partner**, заменив его свойством **MultiSubnetFailover** со значением **Yes**, а имя сервера в строке подключения — прослушивателем группы доступности. Если в строке подключения используются **Failover_Partner** и **MultiSubnetFailover=Yes**, то драйвер выдаст ошибку. Но если в строке подключения используются параметры **Failover_Partner** и **MultiSubnetFailover=No** (или **ApplicationIntent=ReadWrite**), то приложение будет использовать зеркальное отображение базы данных.  
  
Если в базе данных-источнике группы доступности используется зеркальное отображение баз данных, то драйвер вернет ошибку. Это также произойдет в том случае, если используется параметр **MultiSubnetFailover=Yes** в строке подключения, устанавливающей соединение с базой данных-источником, а не с прослушивателем группы доступности.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="ole-db"></a>OLE DB  
OLE DB Driver for SQL Server поддерживает ключевые слова **ApplicationIntent** и **MultiSubnetFailover**.   
  
Для поддержки [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] в OLE DB Driver for SQL Server были добавлены два ключевые слова строки подключения OLE DB:  
  
-   **ApplicationIntent** 
-   **MultiSubnetFailover**  
  
 Дополнительные сведения о ключевых словах строки подключения см. в статье [Использование ключевых слов строки подключения с драйвером OLE DB для SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

### <a name="application-intent"></a>Назначение приложения 

Эквивалентными свойствами соединения являются следующие:  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
В OLE DB Driver for SQL Server для указания намерения приложения может использоваться один из следующих методов.  
  
 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** использует ранее настроенный набор свойств для инициализации источника данных и создания объекта источника данных. Укажите назначение приложения в качестве свойства поставщика или в виде расширенной строки свойств.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** принимает строку подключения, которая может содержать ключевое слово **Application Intent**.  
  
 -   **IDBProperties::SetProperties**  
 Чтобы задать значение свойства **ApplicationIntent**, вызовите **IDBProperties::SetProperties**, передав свойство **SSPROP_INIT_APPLICATIONINTENT** со значением "**ReadWrite**" или "**ReadOnly**" или свойство **DBPROP_INIT_PROVIDERSTRING** со значением, содержащим "**ApplicationIntent=ReadOnly**" или "**ApplicationIntent=ReadWrite**".  
  
Назначение приложения можно указать в поле "Свойства назначения приложения" на вкладке "Все" в диалоговом окне **Свойства канала передачи данных**.  
  
После установки неявных соединений эти подключения будут использовать настройку назначения приложения для родительского подключения. Аналогичным образом несколько сеансов, созданных с использованием одного источника данных, будут наследовать настройки назначения приложения от источника данных.  
  
### <a name="multisubnetfailover"></a>MultiSubnetFailover

Эквивалентными свойствами соединения являются следующие:  
  
-   **SSPROP_INIT_MULTISUBNETFAILOVER**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  

Для установки параметра MultiSubnetFailover приложение OLE DB Driver for SQL Server может использовать один из следующих методов:  

 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** использует ранее настроенный набор свойств для инициализации источника данных и создания объекта источника данных. Укажите назначение приложения в качестве свойства поставщика или в виде расширенной строки свойств.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** принимает строку подключения, которая может содержать ключевое слово **MultiSubnetFailover**.  

-   **IDBProperties::SetProperties**  
Чтобы задать значение свойства **MultiSubnetFailover**, вызовите **IDBProperties::SetProperties**, передавая свойство **SSPROP_INIT_MULTISUBNETFAILOVER** со значением **VARIANT_TRUE**, **VARIANT_FALSE** или свойство **DBPROP_INIT_PROVIDERSTRING** со значением, содержащим "**MultiSubnetFailover=Yes**" или "**MultiSubnetFailover=No**".

#### <a name="example"></a>Пример

```
DBPROP rgPropMultisubnet;

rgPropMultisubnet.dwPropertyID = SSPROP_INIT_MULTISUBNETFAILOVER;
rgPropMultisubnet.dwOptions = DBPROPOPTIONS_REQUIRED;
rgPropMultisubnet.dwStatus = DBPROPSTATUS_OK;
rgPropMultisubnet.colid = DB_NULLID;
V_VT(&(rgPropMultisubnet.vValue)) = VT_BOOL;
V_BOOL(&(rgPropMultisubnet.vValue)) = VARIANT_TRUE;

DBPROPSET PropSet;

PropSet.rgProperties = &rgPropMultisubnet;
PropSet.cProperties = 1;
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;
IDBProperties* pIDBProperties = NULL;
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);
pIDBProperties->SetProperties(1, &PropSet);
```

## <a name="see-also"></a>См. также:  
 [Возможности драйвера OLE DB для SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Использование ключевых слов строки подключения с драйвером OLE DB для SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)  
  
  

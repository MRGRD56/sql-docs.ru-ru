---
title: Имена субъекта-службы в клиентских соединениях (OLE DB) | Документация Майкрософт
description: Сведения о свойствах OLE DB Driver for SQL Server и функциях-членах, поддерживающих имена участника-службы в клиентских приложениях.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 259ecea7a9890ae44a9a32477f4712137175f04d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754174"
---
# <a name="service-principal-names-spns-in-client-connections-ole-db-in-sql-server-native-client"></a>Имена субъектов-служб (SPN) в клиентских соединениях (OLE DB) в SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]


  В этом разделе описываются свойства OLE DB и функции элементов, поддерживающие имена участника-службы (SPN) в клиентских приложениях. Дополнительные сведения об именах субъектов-служб в клиентских приложениях см. в статье [Поддержка имени участника-службы в клиентских соединениях](../../oledb/features/service-principal-name-spn-support-in-client-connections.md). Пример можно найти в статье [Интегрированная проверка подлинности Kerberos &#40;OLE DB&#41;](../../oledb/ole-db-how-to/integrated-kerberos-authentication-ole-db.md).  
  
## <a name="provider-initialization-string-keywords"></a>Ключевые слова строки инициализации поставщика  
 Следующие ключевые слова строки инициализации поставщика поддерживают имена участников-служб в приложениях OLE DB. В следующей таблице значения в столбце ключевых слов используются строкой поставщика в методе IDBInitialize::Initialize. Значения в столбце описания используются в строках инициализации при соединении с помощью объектов данных ActiveX или метода IDataInitialize::GetDataSource.  
  
|Ключевое слово|Описание|Значение|  
|-------------|-----------------|-----------|  
|ServerSPN|Имя участника-службы сервера|Имя участника-службы для сервера. Значением по умолчанию является пустая строка, и в этом случае OLE DB Driver for SQL Server использует имя участника-службы по умолчанию, создаваемое поставщиком.|  
|FailoverPartnerSPN|Имя участника-службы партнера по обеспечению отработки отказа|Имя участника-службы для партнера по обеспечению отработки отказа. Значением по умолчанию является пустая строка, и в этом случае OLE DB Driver for SQL Server использует имя участника-службы по умолчанию, создаваемое поставщиком.|  
  
## <a name="data-source-initialization-properties"></a>Свойства инициализации источника данных  
 Следующие свойства набора свойств **DBPROPSET_SQLSERVERDBINIT** позволяют приложениям указывать имена участников-служб.  
  
|Имя|Тип|Использование|  
|----------|----------|-----------|  
|SSPROP_INIT_SERVERSPN|VT_BSTR, чтение и запись|Задает имя участника-службы для сервера. Значением по умолчанию является пустая строка, и в этом случае OLE DB Driver for SQL Server использует имя участника-службы по умолчанию, создаваемое поставщиком.|  
|SSPROP_INIT_FAILOVERPARTNERSPN|VT_BSTR, чтение и запись|Указывает имя участника-службы для партнера по обеспечению отработки отказа. Значением по умолчанию является пустая строка, и в этом случае OLE DB Driver for SQL Server использует имя участника-службы по умолчанию, создаваемое поставщиком.|  
  
## <a name="data-source-properties"></a>Свойства источника данных  
 Следующие свойства набора свойств **DBPROPSET_SQLSERVERDATASOURCEINFO** позволяют приложениям распознавать метод проверки подлинности.  
  
|Имя|Тип|Использование|  
|----------|----------|-----------|  
|SSPROP_INTEGRATEDAUTHENTICATIONMETHOD|VT_BSTR, только для чтения|Возвращает метод проверки подлинности, используемый для соединения. Приложению возвращается значение, которое OLE DB Driver for SQL Server получает от Windows. Возможные следующие значения. <br />Значение «NTLM», которое возвращается в том случае, если соединение установлено с использованием проверки подлинности NTLM.<br />Значение «Kerberos», которое возвращается в том случае, если соединение установлено с использованием проверки подлинности Kerberos.<br /><br /> Если при открытом соединении нельзя определить метод проверки подлинности, то возвращается значение VT_EMPTY.<br /><br /> Это свойство доступно для чтения только в том случае, если инициализирован источник данных. При попытке считывания свойства до инициализации источника данных, IDBProperties::GetProperies соответственно вернет ошибку DB_S_ERRORSOCCURRED или DB_E_ERRORSOCCURRED и для атрибута DBPROPSET_PROPERTIESINERROR данного свойства будет установлено значение DBPROPSTATUS_NOTSUPPORTED. Это поведение соответствует основной спецификации OLE DB.|  
|SSPROP_MUTUALLYAUTHENTICATED|VT_BOOL, только для чтения|Если при соединении серверов была выполнена взаимная проверка подлинности, возвращает значение VARIANT_TRUE. В противном случае возвращает VARIANT_FALSE.<br /><br /> Это свойство доступно для чтения только в том случае, если инициализирован источник данных. При попытке считывания свойства до инициализации источника данных, IDBProperties::GetProperies соответственно вернет ошибку DB_S_ERRORSOCCURRED или DB_E_ERRORSOCCURRED и для атрибута DBPROPSET_PROPERTIESINERROR данного свойства будет установлено значение DBPROPSTATUS_NOTSUPPORTED. Это поведение соответствует основной спецификации OLE DB.<br /><br /> Если этот атрибут запрашивается соединением, не использующим проверку подлинности Windows, возвращается значение VARIANT_FALSE.|  
  
## <a name="ole-db-api-support-for-spns"></a>Поддержка OLE DB API для имен участников-служб  
 В следующей таблице описываются функции-члены OLE DB, поддерживающие имена участников-служб в клиентских соединениях.  
  
|Функция-член|Description|  
|---------------------|-----------------|  
|IDataInitialize::GetDataSource|Параметр *pwszInitializationString* может содержать новые ключевые слова **ServerSPN** и **FailoverPartnerSPN**.|  
|IDataInitialize::GetInitializationString|Если свойства SSPROP_INIT_SERVERSPN и SSPROP_INIT_FAILOVERPARTNERSPN не имеют значений по умолчанию, они будут включены в строку инициализации с помощью параметра *ppwszInitString* в виде ключевых слов для **ServerSPN** и **FailoverPartnerSPN**. В противном случае эти ключевые слова не будут включены в строку инициализации.|  
|IDBInitialize::Initialize|Если запрос включается путем установки свойства DBPROP_INIT_PROMPT в свойствах инициализации источника данных, будет отображаться диалоговое окно входа OLE DB. Это позволяет ввести имена участников-служб как для основного сервера, так и для его партнера по обеспечению отработки отказа.<br /><br /> Строка поставщика в свойстве DPPROP_INIT_PROVIDERSTRING, если установлена, будет распознавать новые ключевые слова **ServerSPN** и **FailoverPartnerSPN** , и использовать их значения (при их наличии) для инициализации свойств SSPROP_INIT_SERVER_SPN и SSPROP_INIT_FAILOVER_PARTNER_SPN.<br /><br /> Можно вызвать метод IDBProperties::SetProperties, чтобы установить свойства SSPROP_INIT_SERVER_SPN и SSPROP_INIT_FAILOVER_PARTNER_SPN перед вызовом IDBInitialize::Initialize. Это является альтернативой использованию строки поставщика.<br /><br /> Если свойство устанавливается в нескольких местах, заданное программно значение имеет приоритет над значением, указанным в строке поставщика. Значение, заданное в строке инициализации, имеет приоритет над значением, заданным в диалоговом окне входа.<br /><br /> Если одно ключевое слово появляется в строке поставщика несколько раз, приоритет имеет первое значение.|  
|IDBProperties::GetProperties|Чтобы получить значения новых свойств инициализации источника данных SSPROP_INIT_SERVERSPN и SSPROP_INIT_FAILOVERPARTNERSPN, а также новых свойств источника данных SSPROP_AUTHENTICATIONMETHOD и SSPROP_MUTUALLYAUTHENTICATED, можно вызвать IDBProperties::GetProperties.|  
|IdbProperties::GetPropertyInfo|IdbProperties::GetPropertyInfo будет включать новые свойства инициализации источника данных SSPROP_INIT_SERVERSPN и SSPROP_INIT_FAILOVERPARTNERSPN или новые свойства источника данных SSPROP_AUTHENTICATION_METHOD и SSPROP_MUTUALLYAUTHENTICATED.|  
|IDBProperties::SetProperties|IDBProperties::SetProperties можно вызвать, чтобы установить значения новых свойств инициализации источника данных SSPROP_INITSERVERSPN и SSPROP_INIT_FAILOVERPARTNERSPN.<br /><br /> Эти свойства можно задать в любое время, но если источник данных уже открыт, будет возвращена следующая ошибка: DB_E_ERRORSOCCURRED, «Многошаговая операция OLE DB сформировала ошибки. Проверьте каждое значение состояния OLE DB (если возможно). Работа не была выполнена».|  
  
## <a name="see-also"></a>См. также:  
 [Программирование драйвера OLE DB для SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  

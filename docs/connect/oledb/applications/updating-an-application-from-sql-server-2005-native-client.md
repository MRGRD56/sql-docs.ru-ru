---
title: Обновление приложения с переходом от SQL Server 2005 Native Client | Документы Майкрософт
description: Узнайте о критических изменениях в OLE DB Driver for SQL Server в сравнении с SQL Server Native Client в SQL Server 2005 (9.x).
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, updating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8f53c33d04e76c362737b7c7bdc3ad803846cf9
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793133"
---
# <a name="updating-applications-from-sql-server-2005-native-client"></a>Обновление приложений с переходом от SQL Server 2005 Native Client

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

В этой статье рассматриваются критические изменения в OLE DB Driver for SQL Server в сравнении с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client в [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].

При обновлении с компонентов доступа к данным MDAC до драйвера OLE DB для SQL Server могут возникнуть определенные изменения в работе. Дополнительные сведения см. в статье [Updating an Application to OLE DB Driver for SQL Server from MDAC](updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md) (Обновление приложения с переходом с MDAC на драйвер OLE DB для SQL Server).

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 поставляется в составе [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 поставляется в составе [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  Собственный клиент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 10.5 поставляется в составе [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]. Клиент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 поставляется в составе [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] и [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)].

|Измененное поведение в Microsoft OLE DB Driver for SQL Server по сравнению с [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] Native Client|Description|
|------------------------------------------------------------------------------------|-----------------|
|OLE DB дополняет данные только до заданного масштаба.|Что касается преобразований, при которых преобразованные данные передаются на сервер, Microsoft OLE DB Driver for SQL Server дополняет данные завершающими нулями только до максимальной длины значений **datetime**. Собственный клиент SQL Server версии 9.0 дополнял данные до 9 разрядов.|
|Проверьте DBTYPE_DBTIMESTAMP для ICommandWithParameter::SetParameterInfo.|Microsoft OLE DB Driver for SQL Server реализует требование OLE DB для *bScale* в ICommandWithParameter::SetParameterInfo на задание точности в долях секунды для DBTYPE_DBTIMESTAMP.|
|Хранимая процедура **sp_columns** теперь возвращает значение **"NO"** вместо значения **"NO"** для столбца IS_NULLABLE.|В OLE DB Driver for SQL Server хранимая процедура **sp_columns** теперь возвращает значение **"NO"** вместо значения **"NO"** для столбца IS_NULLABLE.|
|При выходе значения даты за пределы диапазона теперь происходит возврат другого значения ошибки.|Что касается типа **datetime**, то при выходе даты за пределы диапазона допустимых значений Microsoft OLE DB Driver for SQL Server возвращает номер ошибки, отличный от возвращаемого в предыдущих версиях.<br /><br /> В частности, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 возвращал ошибку 22007 для всех выходящих за пределы диапазона значений года, при преобразовании строки в тип **datetime**, а OLE DB Driver for SQL Server возвращает ошибку 22008, если дата находится в пределах допустимого диапазона для типа **datetime 2**, но за пределами диапазона, поддерживаемого типами **datetime** и **smalldatetime**.|
|В значении **datetime** усекаются доли секунды, а округление не происходит, если при округлении изменится значение дня.|До появления [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 клиент, передавая данные типа **datetime** на сервер, округлял их до ближайшей 1/300-й доли секунды. В OLE DB Driver for SQL Server, в этой ситуации происходит усечение долей секунды, если округление приводит к изменению значения дня.|
|Возможно усечение секунд в значении **datetime**.|В приложении, созданном с помощью драйвера OLE DB для SQL Server, при подключении к серверу [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2005 будут усекаться секунды и доли секунды в части отправленных на сервер данных, относящейся ко времени, если выполняется привязка к столбцу типа datetime с идентификатором типа DBTYPE_DBTIMESTAMP (OLE DB) или SQL_TIMESTAMP (ODBC) и масштабом 0.<br /><br /> Пример:<br /><br /> Входные данные: 1994-08-21 21:21:36.000<br /><br /> Вставляемые данные: 1994-08-21 21:21:00.000|
|Преобразование данных OLE DB из типа DBTYPE_DBTIME в DBTYPE_DATE больше не вызывает изменения значения дня.|До появления собственного клиента версии 10.0 для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], если часть времени для значения DBTYPE_DATE отстояла от полуночи не более чем на полсекунды, то применение кода преобразования OLE DB приводило к изменению дня. В OLE DB Driver for SQL Server день не изменяется (доли секунды усекаются, а не округляются).|
|Изменения преобразования IBCPSession::BCColFmt.|В OLE DB Driver for SQL Server при использовании IBCPSession::BCOColFmt для преобразования SQLDATETIME или SQLDATETIME в строковый тип экспортируется дробное значение. Например, при преобразовании типа SQLDATETIME в тип SQLNVARCHARMAX версии до [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 10.0 возвращали следующее:<br /> 1989-02-01 00:00:00.<br />Драйвер OLE DB для SQL Server возвращает следующее: <br />1989-02-01 00:00:00.0000000.|
|В пользовательских приложениях, в которых используется API BCP, теперь могут обнаруживаться предупреждающие сообщения.|API BCP выдает предупреждающее сообщение, если длина данных превышает заданную длину для полей всех типов. Раньше это предупреждение выдавалось только для символьных типов, но не для всех типов.|
|Вставка пустой строки в объект типа **sql_variant**, привязанный как тип даты и времени, вызывает ошибку.|В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 9.0 вставка пустой строки в объект типа **sql_variant**, привязанный как тип даты и времени, не вызывала ошибки. OLE DB Driver for SQL Server в этом случае совершенно обоснованно возвращает ошибку.|
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] может возвращать различные результаты при выполнении триггера.|Изменения, внесенные в [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], могут привести к тому, что приложение получит другие результаты при выполнении инструкции, вызвавшей выполнение триггера при действующем параметре **NOCOUNT OFF**. В такой ситуации в приложении может возникнуть ошибка. Чтобы устранить эту ошибку, задайте для триггера параметр **NOCOUNT ON**.|

## <a name="see-also"></a>См. также раздел

[Драйвер OLE DB для SQL Server](../oledb-driver-for-sql-server.md)

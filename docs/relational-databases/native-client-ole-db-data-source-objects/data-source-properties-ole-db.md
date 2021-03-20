---
description: SQL Server Native Client свойств источника данных (OLE DB)
title: Свойства источника данных (поставщик собственного клиента OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, data source properties
- properties [OLE DB]
- data source properties [OLE DB]
- OLE DB data source properties [SQL Server Native Client]
ms.assetid: 6e14fefc-4e0b-4847-a833-4cf0abe65d50
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9a87b7f4a43b61e5c4883b46168a5d6103bfced
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756034"
---
#  <a name="sql-server-native-client-data-source-properties-ole-db"></a>SQL Server Native Client свойств источника данных (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента реализует свойства источника данных следующим образом.  
  
|Идентификатор свойства|Описание|  
|-----------------|-----------------|  
|DBPROP_CURRENTCATALOG|Ч/З чтение и запись; По умолчанию: None<br /><br /> Описание: значение DBPROP_CURRENTCATALOG сообщает о текущей базе данных для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] сеанса поставщика OLE DB Native Client. Установка значения этого свойства равноценна установке текущей базы данных с помощью инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] USE *база_данных*.<br /><br /> Начиная с версии [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] при вызове хранимой процедуры [sp_defaultdb](../../relational-databases/system-stored-procedures/sp-defaultdb-transact-sql.md) и указании имени базы данных в нижнем регистре, даже если база данных первоначально была создана с именем в смешанном регистре, свойство DBPROP_CURRENTCATALOG возвратит имя в нижнем регистре. В предыдущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] свойство DBPROP_CURRENTCATALOG возвращало имя в ожидаемом смешанном регистре.|  
|DBPROP_MULTIPLECONNECTIONS|Ч/З чтение и запись; По умолчанию: VARIANT_FALSE<br /><br /> Описание. Если в соединении выполняется команда, не создающая набора строк или создающая набор строк, который не является серверным курсором, то для выполнения другой команды, запущенной в это же время, создается новое соединение, если свойство DBPROP_MULTIPLECONNECTIONS имеет значение VARIANT_TRUE.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента не будет создавать другое соединение, если DBPROP_MULTIPLECONNECTION VARIANT_FALSE или если транзакция активна в соединении. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента возвращает DB_E_OBJECTOPEN, если DBPROP_MULTIPLECONNECTIONS VARIANT_FALSE, и возвращает E_FAIL при наличии активной транзакции. Управление транзакциями и блокировками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] производит отдельно для каждого соединения. Если создано второе соединение, команды в отдельных соединениях не используют общие блокировки. Чтобы убедиться, что одна команда не блокирует другую, удерживайте блокировки строк, запрошенных другой командой. Это верно и при создании нескольких сеансов.<br /><br /> Каждый сеанс имеет отдельное соединение.|  
  
 В DBPROPSET_SQLSERVERDATASOURCE свойств, зависящих от поставщика, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик OLE DB собственного клиента определяет следующие дополнительные свойства источника данных.  
  
|Идентификатор свойства|Описание|  
|-----------------|-----------------|  
|SSPROP_ENABLEFASTLOAD|Ч/З чтение и запись; По умолчанию: VARIANT_FALSE<br /><br /> Описание. Чтобы включить массовое копирование из памяти, свойству SSPROP_ENABLEFASTLOAD необходимо присвоить значение VARIANT_TRUE. Если это свойство установлено в источнике данных, вновь созданный сеанс позволяет потребителю получить доступ к интерфейсу [IRowsetFastLoad](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md).<br /><br /> Если это свойство имеет значение VARIANT_TRUE, доступ к интерфейсу **IRowsetFastLoad** можно получить через метод **IOpenRowset::OpenRowset**, запросив интерфейс **IID_IRowsetFastLoad**, или с помощью присвоения свойству **SSPROP_IRowsetFastLoad** значения VARIANT_TRUE.|  
|SSPROP_ENABLEBULKCOPY|Ч/З чтение и запись; По умолчанию: VARIANT_FALSE<br /><br /> Описание. Чтобы включить массовое копирование из файлов, свойству SSPROP_ENABLEBULKCOPY необходимо присвоить значение VARIANT_TRUE. Если это свойство установлено в источнике данных, потребитель получает доступ к интерфейсу IBCPSession с тем же уровнем, что и сеанс.<br /><br /> Свойство SSPROP_IRowsetFastLoad также должно быть установлено в значение VARIANT_TRUE.|  
  
## <a name="see-also"></a>См. также:  
 [Объекты источников данных (OLE DB)](../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  

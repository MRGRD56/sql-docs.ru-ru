---
description: Большие двоичные объекты и OLE (поставщик собственного клиента OLE DB)
title: Большие двоичные объекты и OLE (поставщик собственного клиента OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- BLOBs, OLE objects
- BLOBs
- storage object [OLE DB]
- SQL Server Native Client OLE DB provider, BLOBs
- large data, OLE objects
ms.assetid: 767fa2f6-9cd2-436f-add5-e760bed29a58
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ffe0e69afe53d58c820667f8b9ddd83e5b4c4b6e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749524"
---
# <a name="blobs-and-ole-objects-in-sql-server-native-client"></a>Большие двоичные объекты и OLE в SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента предоставляет интерфейс **ISequentialStream** для поддержки доступа потребителя к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типам данных **ntext**, **Text**, **Image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)** и XML в виде больших двоичных объектов (BLOB). Метод **Read** интерфейса **ISequentialStream** позволяет потребителю получать большой объем данных в виде фрагментов данных, с которыми удобно работать.  
  
 Образец приложения, демонстрирующий эту возможность, см. в статье [Задание данных больших объектов (OLE DB)](../../relational-databases/native-client-ole-db-how-to/set-large-data-ole-db.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента может использовать реализуемый потребителем интерфейс **IStorage** , когда потребитель предоставляет указатель интерфейса в методе доступа, связанном с изменением данных.  
  
 Для типов данных больших значений [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик собственного клиента OLE DB проверяет предположения о размере типов в интерфейсах **IROWSET** и DDL. Столбцы с типами данных **varchar**, **nvarchar** и **varbinary** с максимальным размером, установленным в значение unlimited, будут представлены как длинные через наборы строк схемы и интерфейсы, возвращающие типы данных столбцов.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента предоставляет типы **varchar (max)**, **varbinary (max)** и **nvarchar (max)** как DBTYPE_STR, DBTYPE_BYTES и DBTYPE_WSTR соответственно.  
  
 Для работы с этими типами приложение имеет следующие возможности.  
  
-   Выполните привязку, указав тип (DBTYPE_STR, DBTYPE_BYTES, DBTYPE_WSTR). Если буфер недостаточно большой, будет выполнено усечение — точно так же, как в предыдущих версиях (хотя сейчас доступны большие значения).  
  
-   Выполните привязку, указав тип и задав DBTYPE_BYREF.  
  
-   Выполните привязку, указав тип DBTYPE_IUNKNOWN, и используйте потоковую передачу.  
  
 При привязке к DBTYPE_IUNKNOWN используется потоковая возможность ISequentialStream. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента поддерживает привязку выходных параметров как DBTYPE_IUNKNOWN для типов данных больших значений, чтобы упростить сценарии, в которых хранимая процедура возвращает эти типы данных в виде возвращаемых значений, которые будут предоставлены клиенту как DBTYPE_IUNKNOWN.  
  
## <a name="storage-object-limitations"></a>Ограничения объекта хранилища  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента может поддерживать только один открытый объект хранилища. При попытке открыть несколько объектов хранилища (получить ссылку на несколько указателей интерфейса **ISequentialStream**) возвращается DBSTATUS_E_CANTCREATE.  
  
-   В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщике OLE DB собственного клиента значением свойства DBPROP_BLOCKINGSTORAGEOBJECTS только для чтения является VARIANT_TRUE. Оно указывает, что если имеется активный объект хранилища, некоторые методы (не относящиеся к объектам хранилища) завершатся ошибкой E_UNEXPECTED.  
  
-   Длина данных, представленных объектом хранилища, реализованным потребителем, должна быть известна [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщику OLE DB собственного клиента при создании метода доступа к строке, ссылающегося на объект хранилища. Потребитель должен выполнить привязку признака длины в структуре DBBINDING, которая используется для создания метода доступа.  
  
-   Если строка содержит больше одного большого значения данных, а DBPROP_ACCESSORDER не DBPROPVAL_AO_RANDOM, потребитель должен либо использовать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент OLE DBный набор строк с поддержкой курсора поставщика, чтобы получить данные строк или обработать все большие значения данных перед получением других значений строк. Если DBPROP_ACCESSORDER DBPROPVAL_AO_RANDOM, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик OLE DB собственного клиента кэширует все типы данных XML как большие двоичные объекты (BLOB), чтобы к ним можно было обращаться в любом порядке.  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Возврат больших данных](../../relational-databases/native-client-ole-db-blobs/getting-large-data.md)  
  
-   [Присваивание больших данных](../../relational-databases/native-client-ole-db-blobs/setting-large-data.md)  
  
-   [Поддержка потоков для выходных параметров BLOB-объектов](../../relational-databases/native-client-ole-db-blobs/streaming-support-for-blob-output-parameters.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)   
 [Использование типов больших значений](../../relational-databases/native-client/features/using-large-value-types.md)  
  
  

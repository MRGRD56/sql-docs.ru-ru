---
title: Привязки и преобразования (OLE DB) | Документация Майкрософт
description: Узнайте, как OLE DB Driver for SQL Server преобразует значения из формата datetime в datetimeoffset и наоборот. Существует несколько общих правил преобразования.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB]
- bindings [OLE DB]
- OLE DB, bindings and conversions
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2e87bcae399abd2b563f9c347f6752f49568a523
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755344"
---
# <a name="conversions-ole-db"></a>Преобразования (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В этой статье описано, как выполнять преобразование между значениями типа **datetime** и **datetimeoffset**. Преобразования, описанные в этом разделе, либо уже предоставлены OLE DB, либо являются согласованным расширением OLE DB.  
  
 Формат литералов и строк для дат и времени в OLE DB обычно соответствует ISO и не зависит от локали, установленной на клиенте. Единственное исключение — DBTYPE_DATE, для которого стандартом является OLE-автоматизация. Однако, поскольку OLE DB Driver for SQL Server проводит преобразования типов только при передаче данных с клиента или на клиент, приложение никак не может обязать OLE DB Driver for SQL Server преобразовывать тип DBTYPE_DATE в строковые форматы или обратно. Во всех остальных случаях строки используют следующие форматы (скобками отмечены необязательные элементы).  
  
-   Строки **DateTime** и **DateTimeOffset** имеют следующий формат:  
  
     *ГГГГ*-*ММ*-*ДД*[ *чч*:*мм*:*сс*[.*9999999*][ ± *чч*:*мм*]]  
  
-   Формат строк типа **time**:  
  
     *чч*:*мм*:*сс*[.*9999999*]  
  
-   Строка **date** имеет такой формат:  
  
     *гггг*-*мм*-*дд*  
  
> [!NOTE]  
>  Предыдущие версии собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и SQLOLEDB реализовали преобразования OLE в случаях, когда стандартные преобразования возвращали ошибку. OLE DB Driver for SQL Server демонстрирует такое же поведение, как и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client. Это означает, что некоторые преобразования в OLE DB Driver for SQL Server отличаются от спецификации OLE DB.  
  
 Преобразования из строк обеспечивают гибкость в отношении пробелов и ширины полей. Дополнительные сведения см. в разделе "Форматы данных: строки и литералы" статьи [Улучшения поддержки типов данных даты и времени OLE DB](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md).  
  
 Далее приведены общие правила преобразования.  
  
-   При преобразовании строки в тип данных даты-времени строка сначала проходит синтаксический анализ как литерал ISO. Если анализ завершился неудачей, строка проходит синтаксический анализ как литерал даты OLE, содержащий компонент времени.  
  
-   Если время отсутствует, но получатель способен его хранить, оно устанавливается в нулевое значение. Если дата отсутствует, но принимающий объект может хранить дату, этой дате присваивается значение сегодняшней даты при использовании преобразования ISO и 30 декабря 1899 года при использовании преобразования OLE.  
  
-   Если клиент использует тип даты, в котором не указан часовой пояс, но сервер может хранить информацию о часовом поясе, предполагается, что данные клиента используют часовой пояс клиента.  
  
-   Если клиент содержит информацию о часовом поясе, но на сервере данных часового пояса нет, предполагается, что часовой пояс задан в формате UTC. Это отличается от поведения сервера.  
  
-   Если время присутствует, но получатель не способен его хранить, компонент времени пропускается.  
  
-   Если дата присутствует, но получатель не способен ее хранить, компонент даты пропускается.  
  
-   Если при преобразовании с клиента на сервер происходит усечение секунд или долей секунд, возвращается значение DB_E_ERRORSOCCURRED и устанавливается состояние DBSTATUS_E_DATAOVERFLOW.  
  
-   Если усечение секунд или долей секунд происходит при преобразовании с клиента на сервер, устанавливается состояние DBSTATUS_S_TRUNCATED.  
  
## <a name="in-this-section"></a>в этом разделе  
 [Преобразования, выполняемые при передаче от клиента к серверу](../../oledb/ole-db-date-time/conversions-performed-from-client-to-server.md)  
 Описывает преобразования даты и времени, проводимые между клиентским приложением, написанным с помощью драйвера OLE DB для SQL Server, и [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] (или более поздней версией).  
  
 [Преобразования, выполняемые при передаче от сервера к клиенту](../../oledb/ole-db-date-time/conversions-performed-from-server-to-client.md)  
 Описывает преобразования даты и времени, проводимые между [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] (или более поздней версией) и клиентским приложением, написанным с помощью драйвера OLE DB для SQL Server.  
  
## <a name="see-also"></a>См. также:  
 [Улучшения функций даты и времени &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  

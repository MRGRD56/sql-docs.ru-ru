---
description: Обработка результатов (поставщик собственного клиента OLE DB)
title: Обработка результатов (поставщик собственного клиента OLE DB)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, results processing
- OLE DB, processing results
- rowsets [SQL Server], results processing
- results [SQL Server Native Client]
ms.assetid: 20887ac4-f649-4e7f-92e6-f929e2e70952
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f479b8015248276e87c226c974be1344d06e27f9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753644"
---
# <a name="processing-results-native-client-ole-db-provider"></a>Обработка результатов (поставщик собственного клиента OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Если объект набора строк создан в результате выполнения команды или его формирования непосредственно из поставщика, то пользователю необходимо иметь возможность доступа к данным набора строк и их получения.  
  
 Наборы строк являются важными объектами, позволяющими поставщику OLE DB для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обеспечивать доступ к данным в табличной форме. С концептуальной точки зрения объект набора строк — это набор строк, в котором каждая строка содержит данные столбцов. Объект набора строк предоставляет доступ к интерфейсам, таким как **IRowset** (содержит методы последовательного получения строк из набора), **IAccessor** (обеспечивает определение группы привязок столбцов, описывающих привязку табличных данных к переменным пользовательской программы), **IColumnsInfo** (предоставляет информацию о столбцах набора строк) и **IRowsetInfo** (предоставляет информацию о наборе строк).  
  
 Пользователь может вызывать метод **IRowset::GetData** для получения строки данных из набора строк в буфер. Перед вызовом функции **GetData** пользователь описывает буфер с помощью набора структур DBBINDING. Каждая привязка описывает способ сохранения столбца набора строк в буфере пользователя и содержит следующее.  
  
-   Порядковый номер столбца (или параметра), к которому относится привязка.  
  
-   Информация о том объекте, для которого выполняется привязка (например, значение данных, длина данных, состояние привязки).  
  
-   Информация о смещении в буфере для каждой из этих частей.  
  
-   Длина и тип значений данных, которые имеются в буфере пользователя.  
  
 При получении данных поставщик использует информацию каждой привязки, чтобы определить, куда и как получить данные из буфера пользователя. При задании значений данных в буфере пользователя поставщик использует информацию каждой привязки, чтобы определить, куда и как вернуть данные в буфер пользователя.  
  
 После задания структур DBBINDING создается метод доступа (**IAccessor::CreateAccessor**). Метод доступа представляет собой коллекцию привязок и используется для получения данных из буфера пользователя или задания значений данных в буфере.  
  
## <a name="see-also"></a>См. также:  
 [Создание приложения поставщика SQL Server Native Client OLE DB](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)   
 [Инструкции по OLE DB](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
  

---
description: Хранимые процедуры — выполняются в SQL Server Native Client
title: Выполнение хранимых процедур (OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [OLE DB], executing
- OLE DB, stored procedures
- SQL Server Native Client OLE DB provider, stored procedures
ms.assetid: c77d9be9-2176-4438-8c7a-04b63ebece08
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3c37f35094020a3e587f41acc464e8c1a9da66a0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745014"
---
# <a name="stored-procedures---running-in-sql-server-native-client"></a>Хранимые процедуры — выполняются в SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  При выполнении инструкций вызов хранимой процедуры в источнике данных (вместо выполнения или подготовки инструкции непосредственно в клиентском приложении) может обеспечить следующее:  
  
-   высокую производительность;  
  
-   низкие издержки сети;  
  
-   лучшую согласованность;  
  
-   большую точность;  
  
-   дополнительные возможности.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента поддерживает три механизма, с помощью которых [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] хранимые процедуры возвращают данные:  
  
-   Каждая инструкция SELECT в хранимой процедуре формирует результирующий набор.  
  
-   Процедура может возвращать данные через выходные параметры.  
  
-   Процедура может иметь целочисленный код возврата.  
  
 Приложение должно быть способно обработать все эти данные, возвращаемые хранимыми процедурами.  
  
 Разные поставщики OLE DB возвращают выходные параметры и значения на разных этапах во время обработки результатов. В случае с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поставщиком собственного клиента OLE DB выходные параметры и коды возврата не предоставляются до тех пор, пока потребитель не извлек или не отменил результирующие наборы, возвращенные хранимой процедурой. Коды возврата и выходные параметры возвращаются сервером в последнем пакете потока табличных данных.  
  
 Поставщики используют свойство DBPROP_OUTPUTPARAMETERAVAILABILITY для сообщения о возвращении выходных параметров и возвращаемых значений. Это свойство доступно в наборе свойств DBPROPSET_DATASOURCEINFO.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента задает для свойства DBPROP_OUTPUTPARAMETERAVAILABILITY значение DBPROPVAL_OA_ATROWRELEASE, чтобы указать, что коды возврата и выходные параметры не возвращаются до тех пор, пока результирующий набор не будет обработан или освобожден.  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
  

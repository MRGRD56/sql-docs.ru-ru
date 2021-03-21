---
description: Управление столбцами text и image
title: Управление столбцами Text и Image | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- text columns [ODBC]
- SQL Server Native Client ODBC driver, image columns
- SQL Server Native Client ODBC driver, text columns
- data types [ODBC], text
- columns [ODBC]
- ODBC data types, image columns
- data types [ODBC], mapping
- ODBC data types, text columns
- image columns [ODBC]
ms.assetid: 7b543556-ff36-4d35-ac08-de96223d92cd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5990ec13333c34fd5d737ea60658960d5530552a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737434"
---
# <a name="managing-text-and-image-columns"></a>Управление столбцами text и image
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]данные типа **Text**, **ntext** и **Image** (также называемые длинными данными) представляют собой символьные или двоичные строковые типы данных, которые могут содержать слишком большие значения данных для размещения в столбцах **char**, **varchar**, **binary** или **varbinary** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Тип данных **Text** сопоставляется с типом данных ODBC SQL_LONGVARCHAR. **ntext** сопоставляется с SQL_WLONGVARCHAR; **изображения** и сопоставляются с SQL_LONGVARBINARY. Некоторые объекты данных (например, длинные документы или большие битовые карты) слишком велики для их размещения в памяти. Для получения длинных данных из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] последовательных частей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента позволяет приложению вызывать [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md). Для отправки длинных данных в последовательных частях приложение может вызвать [SQLPutData](../../relational-databases/native-client-odbc-api/sqlputdata.md). Параметры, для которых данные посылаются во время выполнения, называются параметрами c данными времени выполнения.  
  
 Приложение может на самом деле записывать или извлекать данные любого типа (не только длинные данные) с **SQLPutData** или **SQLGetData**, хотя в частях могут быть отправлены и получены только **символьные** и **двоичные** данные. Однако если данные достаточно малы для размещения в одном буфере, обычно нет причин использовать **SQLPutData** или **SQLGetData**. Гораздо проще привязать единичный буфер к параметру или столбцу.  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Привязанные и непривязанные столбцы text и image](../../relational-databases/native-client-odbc-text-image-columns/bound-vs-unbound-text-and-image-columns.md)  
  
-   [Изменения с ведением журнала и без ведения журнала](../../relational-databases/native-client-odbc-text-image-columns/logged-vs-unlogged-modifications.md)  
  
-   [Данные времени выполнения и столбцы text, ntext или image](../../relational-databases/native-client-odbc-text-image-columns/data-at-execution-and-text-ntext-or-image-columns.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (ODBC)](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  

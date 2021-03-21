---
description: Данные времени выполнения и столбцы text, ntext или image
title: Данные на этапе выполнения и Text, ntext, Image
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- text columns [ODBC]
- SQL Server Native Client ODBC driver, image columns
- SQL Server Native Client ODBC driver, text columns
- data types [ODBC], image
- data types [ODBC], text
- columns [ODBC]
- ODBC data types, image columns
- ODBC data types, text columns
- data-at-execution
- ODBC data-at-execution
- image columns [ODBC]
ms.assetid: 67ffb1a6-f38d-4712-ba64-96bdd41ec2b2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d1cd9d264fe6c21c4b15b65b5250ea4f148c78f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755014"
---
# <a name="data-at-execution-and-text-ntext-or-image-columns"></a>Данные времени выполнения и столбцы text, ntext или image
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Данные времени выполнения ODBC позволяют приложениям работать с очень большими объемами данных в связанных столбцах или параметрах. При получении очень больших столбцов **Text**, **ntext** или **Image** приложение может не позволить просто выделить огромный буфер, привязать столбец к буферу и получить строку. При обновлении очень больших столбцов **Text**, **ntext** или **Image** приложение может не позволить просто выделить огромный буфер, привязать его к маркеру параметра в инструкции SQL, а затем выполнить инструкцию. В таких случаях приложение должно использовать [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) или [SQLPutData](../../relational-databases/native-client-odbc-api/sqlputdata.md) с параметрами, выполняемыми при выполнении.  
  
## <a name="see-also"></a>См. также:  
 [Управление столбцами text и image](../../relational-databases/native-client-odbc-text-image-columns/managing-text-and-image-columns.md)  
  
  

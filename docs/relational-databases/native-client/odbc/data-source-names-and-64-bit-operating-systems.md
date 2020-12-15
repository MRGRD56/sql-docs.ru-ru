---
title: Имена источников данных и 64-разрядные операционные системы | Документация Майкрософт
description: Узнайте, как создать и запустить приложение в виде 32-разрядного приложения в 64-разрядной операционной системе, создав источник данных ODBC с помощью администратора ODBC.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: c2f86810-2775-4ddd-8df7-e8373785a7fc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 416cd9e812cabccacc710f2b23c506702fae1778
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475975"
---
# <a name="data-source-names-and-64-bit-operating-systems"></a>Имена источников данных и 64-разрядные операционные системы
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Для построения и запуска 32-разрядного приложения в 64-разрядной операционной системе необходимо создать источник данных ODBC с помощью программы администрирования ODBC (исполняемый файл %windir%\SysWOW64\odbcad32.exe).  
  
## <a name="remarks"></a>Комментарии  
 В 64-разрядной операционной системе Windows есть два файла odbcad32.exe.  
  
-   %SystemRoot%\system32\odbcad32.exe используется для создания и поддержки имен источников данных для 64-разрядных приложений.  
  
-   %SystemRoot%\SysWOW64\odbcad32.exe используется для создания и поддержки имен источников данных для 32-разрядных приложений, в том числе и 32-разрядных приложений, работающих на 64-разрядной операционной системе.  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
  
  

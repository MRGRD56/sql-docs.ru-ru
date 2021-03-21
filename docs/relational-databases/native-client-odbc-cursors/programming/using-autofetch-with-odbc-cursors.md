---
description: Использование автоматической выборки с помощью курсоров ODBC
title: Использование автоматической выборки с курсорами ODBC | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC cursors, autofetch
- autofetch option
- cursors [ODBC], autofetch
ms.assetid: 57bd55f4-8945-4d8d-9f58-d30c81d2a514
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8667b00c908a0355fc628dc056ede93480035c59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756224"
---
# <a name="using-autofetch-with-odbc-cursors"></a>Использование автоматической выборки с помощью курсоров ODBC
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  При подключении к экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента поддерживает параметр автоматической выборки при использовании любого типа серверного курсора. При автоматической выборке функция **SQLExecute** или **SQLExecDirect** , открывающая курсор, также имеет неявную функцию [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)(SQL_FIRST). Строки, составляющие первый набор строк, возвращаются в привязанные переменные приложения в ходе выполнения инструкции; это позволяет сэкономить одно обращение через сеть к серверу [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) не поддерживается, если включен параметр автоматической выборки. столбцы результирующего набора должны быть привязаны к переменным программы.  
  
 Приложения запрашивают автоматическую выборку, задавая для зависящего от драйвера атрибута инструкции SQL_SOPT_SS_CURSOR_OPTIONS значение SQL_CO_AF.  
  
## <a name="see-also"></a>См. также:  
 [Сведения о программировании курсора &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-cursors/programming/cursor-programming-details-odbc.md)  
  
  

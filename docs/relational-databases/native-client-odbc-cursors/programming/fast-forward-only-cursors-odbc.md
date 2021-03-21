---
description: Быстрые курсоры последовательного доступа (ODBC)
title: Быстрые Forward-Only курсоры (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- fast forward-only cursors
- forward-only cursors
- cursors [ODBC], fast forward-only
- ODBC cursors, fast forward-only
ms.assetid: 0707d07e-fc95-42ed-9280-b7e508ac8c62
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b0d9030c4e2bd86612e41c2fad97905a29f11de
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756234"
---
# <a name="fast-forward-only-cursors-odbc"></a>Быстрые курсоры последовательного доступа (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  При подключении к экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] драйвер ODBC собственного клиента поддерживает оптимизацию производительности для однопроходных курсоров только для чтения. Быстрые однопроходные курсоры реализуются драйвером и сервером внутренне, при этом способ реализации очень похож на результирующие наборы по умолчанию. Кроме того, имея высокую производительность, быстрые однопроходные курсоры обладают следующими характеристиками.  
  
-   [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) не поддерживается. Столбцы результирующего набора должны быть привязаны к переменным программы.  
  
-   Когда сервер достигает конца курсора, он автоматически закрывает курсор. Приложение по-прежнему должно вызывать [SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md) или [SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md)(SQL_CLOSE), но драйверу не нужно отсылать запрос на закрытие на сервер. Это экономит обращение через сеть к серверу.  
  
 Приложение запрашивает быстрые однопроходные курсоры при помощи атрибута инструкции SQL_SOPT_SS_CURSOR_OPTIONS, зависящего от драйвера. При установке значения SQL_CO_FFO быстрые однопроходные курсоры разрешаются без автоматической выборки. При установке значения SQL_CO_FFO_AF включен параметр автоматической выборки. Дополнительные сведения о автоматической выборке см. в разделе [Использование автоматической выборки с курсорами ODBC](../../../relational-databases/native-client-odbc-cursors/programming/using-autofetch-with-odbc-cursors.md).  
  
 Быстрые однопроходные курсоры с автоматической выборкой можно использовать для получения небольшого результирующего набора с однократным обращением к серверу. В этих шагах *n* — число возвращаемых строк:  
  
1.  Установите для атрибута SQL_SOPT_SS_CURSOR_OPTIONS значение SQL_CO_FFO_AF.  
  
2.  Задайте для параметра SQL_ATTR_ROW_ARRAY_SIZE значение *n* + 1.  
  
3.  Привязка результирующих столбцов к массивам элементов *n* + 1 (для защиты, если фактически выбирается *n* + 1 строк).  
  
4.  Откройте курсор с помощью **SQLExecDirect** или **SQLExecute**.  
  
5.  Если возвращаемое состояние — SQL_SUCCESS, вызовите **SQLFreeStmt** или **SQLCloseCursor** , чтобы закрыть курсор. Все данные для строк будут находиться в связанных программных переменных.  
  
 С помощью этих шагов **SQLExecDirect** или **SQLExecute** отправляет запрос на открытие курсора с включенным параметром автоматической выборки. В ответ на этот отдельный запрос от клиента сервер:  
  
-   Открывает курсор.  
  
-   Строит результирующий набор и отправляет строки клиенту.  
  
-   Так как размер набора строк был установлен в значение на 1 большее, чем число строк результирующего набора, сервер определяет конец курсора и закрывает его.  
  
## <a name="see-also"></a>См. также:  
 [Сведения о программировании курсора &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-cursors/programming/cursor-programming-details-odbc.md)  
  
  

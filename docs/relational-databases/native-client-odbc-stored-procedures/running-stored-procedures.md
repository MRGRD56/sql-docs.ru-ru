---
title: Выполнение хранимых процедур | Документация Майкрософт
description: Хранимая процедура представляет собой исполняемый объект, хранящийся в базе данных. SQL Server поддерживает хранимые процедуры и расширенные хранимые процедуры.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, stored procedures
- stored procedures [ODBC], running
- SQL Server Native Client ODBC driver, stored procedures
- stored procedures [ODBC], executing
ms.assetid: 866b6dd3-2acd-4dfb-aeca-a0352b2d4c6a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 82a7f61fb23fafdc8c573cc28d3d11add163f12b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104743984"
---
# <a name="running-stored-procedures"></a>Выполнение хранимых процедур
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Хранимая процедура представляет собой исполняемый объект, хранящийся в базе данных. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает:  
  
-   Хранимые процедуры  
  
     Одна или несколько инструкций SQL предварительно откомпилированы в одну исполняемую процедуру.  
  
-   Расширенные хранимые процедуры  
  
     Библиотеки динамических ссылок (DLL) C или C++, написанные с использованием API-интерфейса служб SQL Server Open Data Services для расширенных хранимых процедур. API-интерфейс служб Open Data Services расширяет возможности хранимых процедур, позволяя им использовать код на C или C++.  
  
 При выполнении инструкций путем вызова хранимой процедуры для источника данных (в противоположность непосредственному выполнению или подготовке инструкций в клиентском приложении) можно получить следующие преимущества.  
  
-   Более высокая производительность  
  
     Инструкции SQL анализируются и компилируются во время формирования процедур. Затем эта дополнительная нагрузка компенсируется при выполнении процедур.  
  
-   Снижение сетевых издержек  
  
     Когда вместо отправки по сети сложных запросов выполняется процедура, это способствует сокращению объема сетевого трафика. Если при выполнении хранимой процедуры приложение ODBC использует синтаксическую конструкцию ODBC {CALL}, драйвер ODBC принимает дополнительные меры по оптимизации, которые снимают необходимость преобразования данных параметров.  
  
-   Повышение уровня согласованности  
  
     Если правила организации реализованы в центральном ресурсе, таком, как хранимая процедура, их можно кодировать, тестировать и отлаживать в один прием. Затем индивидуальные программисты могут использовать эти протестированные хранимые процедуры, не разрабатывая собственных реализаций.  
  
-   Более высокая точность  
  
     Поскольку хранимые процедуры, как правило, создаются опытными программистами, они обычно бывают более эффективными и содержат меньше ошибок, нежели код, создаваемый многократно программистами различных уровней компетентности.  
  
-   Дополнительные функциональные возможности  
  
     В расширенных хранимых процедурах можно использовать средства на языках C и C++, недоступные в инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
     Пример вызова хранимой процедуры см. в разделе [обработка кодов возврата и выходных параметров &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md).  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Вызов хранимой процедуры](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)  
  
-   [Создание пакетной обработки вызовов хранимых процедур](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)  
  
-   [Обработка результатов хранимой процедуры](../../relational-databases/native-client-odbc-stored-procedures/processing-stored-procedure-results.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Разделы руководства по выполнению хранимых процедур &#40;ODBC&#41;](../native-client-odbc-how-to/running-stored-procedures-call-stored-procedures.md)  
  

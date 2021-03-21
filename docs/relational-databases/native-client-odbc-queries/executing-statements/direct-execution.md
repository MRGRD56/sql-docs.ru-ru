---
description: Прямое выполнение
title: Прямое выполнение | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC applications, statements
- direct execution [ODBC]
- SQLExecDirect function
- statements [ODBC], direct execution
ms.assetid: fa36e1af-ed98-4abc-97c1-c4cc5d227b29
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a043fd35876566c724b946daeca0e95b0b021cd3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750724"
---
# <a name="direct-execution"></a>Прямое выполнение
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Прямое выполнение — это наиболее распространенный способ выполнения инструкции. Приложение создает строку символов, содержащую [!INCLUDE[tsql](../../../includes/tsql-md.md)] инструкцию, и отправляет ее для выполнения с помощью функции **SQLExecDirect** . Когда инструкция достигает сервера, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] компилирует ее в план выполнения и немедленно запускает план.  
  
 Прямое выполнение обычно используется приложениями, которые создают и выполняют инструкции во время выполнения, и является наиболее эффективным методом для инструкций, которые будут выполняться одновременно. Недостатком многих баз данных является то, что SQL-инструкция должна анализироваться и компилироваться при каждом выполнении, что увеличивает нагрузку при неоднократном использовании данной процедуры.  
  
 В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] значительно повышена производительность прямого выполнения часто выполняемых инструкций в многопользовательских средах, а использование SQLExecDirect с маркерами параметров для часто используемых инструкций SQL может приблизить их эффективность к эффективности выполнения после подготовки.  
  
 При подключении к экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] драйвер ODBC для собственного клиента использует [sp_executesql](../../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md) для передачи инструкции SQL или пакета, указанного в **SQLExecDirect**. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] имеет логику, позволяющую быстро определить, совпадает ли инструкция SQL или пакет, выполненный с **sp_executesql** , с инструкцией или пакетом, создавшим план выполнения, который уже существует в памяти. В случае совпадения [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] повторно использует существующий план, а не компилирует новый. Это означает, что часто выполняемые инструкции SQL, выполняемые с помощью **SQLExecDirect** в системе с большим количеством пользователей, получат преимущество от многих преимуществ повторного использования планов, которые были доступны только для хранимых процедур в предыдущих версиях [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Данное преимущество работает только в случае одновременного выполнения одной инструкции или пакета SQL несколькими пользователями. Следуйте этим соглашениям по написанию кода для повышения вероятности того, что инструкции SQL, выполняемые различными клиентами, будут достаточно похожи, чтобы повторно использовать планы выполнения.  
  
-   Не включайте константы данных в инструкции SQL. Вместо этого используйте маркеры параметров, привязанные к переменным программы. Дополнительные сведения см. [в разделе Использование параметров инструкции](../../../relational-databases/native-client-odbc-queries/using-statement-parameters.md).  
  
-   Используйте полные имена объектов. Планы выполнения не будут использоваться повторно при неполных именах объектов.  
  
-   Соединения приложения по возможности должны использовать стандартный набор параметров соединений и инструкций. Планы выполнения, созданные для соединения с одним набором параметров (например ANSI_NULLS), не используются для соединения, имеющего другой набор параметров. Драйвер ODBC собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и поставщик OLE DB собственного клиента [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] имеют одинаковые установки по умолчанию для этих параметров.  
  
 Если все инструкции, выполняемые с **SQLExecDirect** , кодируются с использованием этих соглашений, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] может повторно использовать планы выполнения при возникновении возможной сделки.  
  
## <a name="see-also"></a>См. также:  
 [Исполнение инструкций &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
  

---
title: Вопросы воспроизведения трассировки
titleSuffix: SQL Server Profiler
description: Узнайте, какие операции, хранимые процедуры, шаблоны и действия журнала не позволяют SQL Server Profiler воспроизводить трассировку.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 73fa339f-b71a-4be4-97ca-d4ae84c8b90b
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.openlocfilehash: d06b9769a76d7e6555e0dc671782fcd062b4574f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349419"
---
# <a name="considerations-for-replaying-traces-sql-server-profiler"></a>Вопросы воспроизведения трассировки (приложение SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] не может воспроизводить следующие виды трассировки.  
  
-   Трассировки, содержащие репликацию транзакций и другие виды деятельности, связанной с журналами транзакций. Эти события пропускаются. Другие типы репликации не затрагивают журнала транзакций, поэтому это к ним не относится.  
  
-   Трассировки, содержащие операции, в которых используются идентификаторы GUID. Эти события пропускаются.  
  
-   Трассировки, содержащие операции в столбцах типов данных **text**, **ntext** и **image** и использующие программу **bcp** , инструкции BULK INSERT, READTEXT, WRITETEXT и UPDATETEXT, а также полнотекстовые операции. Эти события пропускаются.  
  
-   Трассировки, включающие в себя привязку сеансов: системные хранимые процедуры **sp_getbindtoken** и **sp_bindsession** . Эти события пропускаются.  
  
> [!NOTE]  
>  Если не используется предварительно настроенный шаблон воспроизведения (**TSQL_Replay**) и не выполняется сбор всех необходимых данных, приложение [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] не воспроизводит трассировку. Дополнительные сведения см. в разделе [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
 Сведения о разрешениях, необходимых для воспроизведения трассировки, см. в разделе [Permissions Required to Run SQL Server Profiler](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md).  
  
## <a name="see-also"></a>См. также:  
 [bcp Utility](../../tools/bcp-utility.md)   
 [Справочник по классам событий SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md)   
 [sp_getbindtoken (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-getbindtoken-transact-sql.md)   
 [sp_bindsession (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindsession-transact-sql.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [READTEXT (Transact-SQL)](../../t-sql/queries/readtext-transact-sql.md)   
 [WRITETEXT (Transact-SQL)](../../t-sql/queries/writetext-transact-sql.md)   
 [UPDATETEXT (Transact-SQL)](../../t-sql/queries/updatetext-transact-sql.md)  
  
  

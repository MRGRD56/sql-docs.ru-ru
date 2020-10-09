---
description: Трассировка и воспроизведение событий
title: Трассировка и воспроизведение событий | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- replaying events
- traces [SMO]
- events [SMO], replaying
- events [SMO], tracing
ms.assetid: f41b3f85-2f6c-4c3e-9776-8c73d2cc7a53
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a483cbdf9fdeb7e60992ddfb044239a6d495f432
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868602"
---
# <a name="tracing-and-replaying-events"></a>Трассировка и воспроизведение событий
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  В объектах SMO объекты **трассировки** и **воспроизведения** в <xref:Microsoft.SqlServer.Management.Trace> пространстве имен обеспечивают программный доступ к [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)] функциональным возможностям, которые используются для наблюдения за экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] . Приложение позволяет собирать и сохранять данные о каждом событии в файле или в таблице для последующего анализа. Например, с помощью приложения можно наблюдать за рабочей средой, чтобы определить, какие процедуры сказываются на производительности из-за того, что выполняются слишком медленно.  
  
 Объекты **Trace** и **Replay** предоставляют набор объектов, которые можно использовать для создания трассировок экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Эти объекты можно использовать из пользовательских приложений для создания вручную трассировок для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или служб [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]. Кроме того, объекты **трассировки** SMO можно использовать для чтения файлов и ТАБЛИЦ трассировки SQL, созданных мониторингом [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] или ведением журнала служб DTS.  
  
 Объекты **Trace** SMO позволяют выполнять следующие функции:  
  
-   создать трассировку;  
  
-   назначить фильтры для трассировки;  
  
-   назначить отслеживаемые события;  
  
-   остановить и запустить трассировку;  
  
-   прочитать файлы трассировки и таблицы трассировки;  
  
-   получить сведения о событиях трассировки;  
  
-   получить сведения о фильтрах трассировки;  
  
-   программно управлять данными трассировки;  
  
-   записать файлы и таблицы трассировки;  
  
-   воспроизвести файлы и таблицы трассировки.  
  
 Данные трассировки из объектов **Trace** и **Replay** можно использовать в SMO-приложении или исследовать их вручную с помощью [SQL Server Profiler](../../../tools/sql-server-profiler/sql-server-profiler.md). Данные трассировки также совместимы с хранимыми процедурами [SQL Trace](../../../relational-databases/sql-trace/sql-trace.md) , которые также обеспечивают возможности трассировки.  
  
 Объекты трассировки SMO находятся в пространстве имен <xref:Microsoft.SqlServer.Management.Trace>, для использования которого необходима ссылка на файл Microsoft.SQLServer.ConnectionInfo.dll.  
  
 Объектам **трассировки** и **воспроизведения** требуется объект [ServerConnection](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120)) <xref:Microsoft.SqlServer.Management.Smo.Server.%23ctor%2A> для установления соединения с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Объект [ServerConnection](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120)) находится в пространстве имен [Microsoft. SqlServer. Management. Common](/previous-versions/sql/sql-server-2014/ms212673(v=sql.120)) , для которого требуется ссылка на файл Microsoft.SQLServer.ConnectionInfo.dll.  
  
> [!NOTE]  
>  Объекты **Trace** и **Replay** не поддерживаются в 64-разрядной версии платформы.  
  

---
description: Узнайте, как работать с инструкциями и результирующими наборами в JDBC, а также как использовать объект, подходящий для задачи.
title: Работа с инструкциями и результирующими наборами
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cc917534-f5f8-4844-87c8-597c48b4e06d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 52a99005c733ab436c1ea30f02a60ac491d77c0d
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673958"
---
# <a name="working-with-statements-and-result-sets"></a>Работа с инструкциями и результирующими наборами

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

При работе с драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] и его объектами Statement и ResultSet существует ряд способов повысить производительность и надежность ваших приложений.

## <a name="use-the-appropriate-statement-object"></a>Использование подходящего объекта инструкции

При использовании объектов Statement драйвера JDBC, например [SQLServerStatement](reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](reference/sqlserverpreparedstatement-class.md) или [SQLServerCallableStatement](reference/sqlservercallablestatement-class.md), обязательно используйте объект, подходящий для вашей задачи.

- Если параметры OUT отсутствуют, то использовать объект SQLServerCallableStatement не нужно. Вместо него используйте объект SQLServerStatement или SQLServerPreparedStatement.  
- Если не планируется исполнять инструкции несколько раз или параметры IN либо OUT отсутствуют, то использовать объект SQLServerCallableStatement или SQLServerPreparedStatement не нужно. Вместо них используйте объект SQLServerStatement.

## <a name="use-the-appropriate-concurrency-for-resultset-objects"></a>Использование подходящего параллелизма для объектов ResultSet

Обновляемый параллелизм при создании инструкций, выдающих результирующие наборы, требуется, только если планируется непременно обновлять результаты. Маленькие результирующие наборы быстрее всего считывает стандартная однопроходная неизменяемая модель курсора.

## <a name="limit-the-size-of-your-result-sets"></a>Ограничение размера результирующих наборов

Попробуйте использовать метод [setMaxRows](reference/setmaxrows-method-sqlserverstatement.md) (либо SQL-синтаксис SET ROWCOUNT или SELECT TOP N), чтобы ограничить число строк, возвращаемых из потенциально больших результирующих наборов. Если приходится работать с большими результирующими наборами, то, возможно, следует использовать адаптивную буферизацию ответов. Для этого нужно установить свойству строки соединения responseBuffering значение «adaptive». Это значение используется по умолчанию. Такая методика позволяет приложению обрабатывать большие результирующие наборы, не прибегая к курсорам на стороне сервера, и свести к минимуму занимаемую приложением память. Дополнительные сведения об использовании адаптивной буферизации см. в [этой статье](using-adaptive-buffering.md).

## <a name="use-the-appropriate-fetch-size"></a>Использование выборки подходящего размера

Для серверных курсоров только для чтения целесообразность определяется сравнением затрат на обращение к серверу и затрат памяти драйвером. Для обновляемых серверных курсоров размер выборки также влияет на чувствительность результирующего набора к изменениям и параллелизму на сервере. Вы не увидите изменений в строках в текущем буфере выборки, пока не воспользуетесь в явном виде методом [refreshRow](reference/refreshrow-method-sqlserverresultset.md) или пока из буфера не выйдет курсор. У больших буферов выборки будет лучшая производительность (меньше обращений к серверу), но у них меньше чувствительность к изменениям и уменьшению параллелизма на сервере при использовании CONCUR_SS_SCROLL_LOCKS (1009). Для максимальной чувствительности к изменениям размер выборки должен равняться 1. Однако при этом будет совершаться одно обращение к серверу на каждую выбранную строку.

## <a name="use-streams-for-large-in-parameters"></a>Использование потоков для больших параметров IN

Используйте потоки объектов BLOB и CLOB, постепенно материализуемые для управления обновляющимися большими значениями столбцов или отправления больших параметров IN. Драйвер JDBC дробит эти типы и отправляет на сервер за несколько обращений, тем самым позволяя задавать и обновлять значения большие, чем помещаются в памяти.

## <a name="see-also"></a>См. также раздел

[Повышение производительности и надежности с помощью JDBC Driver](improving-performance-and-reliability-with-the-jdbc-driver.md)

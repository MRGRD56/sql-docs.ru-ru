---
title: Воспроизведение до курсора
titleSuffix: SQL Server Profiler
description: Узнайте, как использовать курсор в SQL Server Profiler для приостановки воспроизведения трассировки в определенной точке. Упростите отладку благодаря воспроизведению до курсора.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 89eadc41-4424-4a1c-ba61-0b52c851cdb1
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: a903e7176b4715dcf54db8a55b28a280a825b3bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353350"
---
# <a name="replay-to-a-cursor-sql-server-profiler"></a>воспроизвести до курсора (приложение SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В этом подразделе описывается воспроизведение файлов или таблиц трассировки, которые были приостановлены по достижении курсора при помощи приложения [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Приостановка трассировок курсоров поддерживает отладку, так как существует возможность разбиения воспроизведения длинных скриптов трассировки на короткие сегменты, которые могут анализироваться по шагам.  
  
### <a name="to-replay-to-the-cursor"></a>Воспроизведение до курсора  
  
1.  Откройте файл трассировки или таблицу трассировки, которые необходимо воспроизвести. Дополнительные сведения см. в статье [Открыть файл трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) или в помощник по настройке ядра СУБД [Открыть таблицу трассировки (приложение SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md).  
  
     Убедитесь, что открытый файл трассировки или открытая таблица содержат классы событий, которые необходимо воспроизвести. Дополнительные сведения см. в разделе [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
2.  В окне трассировки выберите событие.  
  
3.  В меню **Воспроизведение** выберите пункт **Выполнить до курсора** и подключитесь к серверу, на котором хотите воспроизвести трассировку.  
  
4.  В диалоговом окне **Конфигурация воспроизведения** проверьте настройки и нажмите кнопку **ОК**.  
  
     Воспроизведение начинается, останавливаясь по достижении первого курсора.  
  
5.  Для продолжения трассировки нажмите клавишу F5.  
  
6.  Повторяйте шаг 5 до завершения трассировки.  
  
## <a name="see-also"></a>См. также:  
 [Воспроизведение нагрузки до точки останова (приложение SQL Server Profiler)](../../tools/sql-server-profiler/replay-to-a-breakpoint-sql-server-profiler.md)   
 [Воспроизведение трассировок](../../tools/sql-server-profiler/replay-traces.md)   
 [Приложение SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  

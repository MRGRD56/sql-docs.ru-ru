---
title: Сравнение средств переноса данных SQL
titleSuffix: SQL Server
description: 'Сравните средства переноса данных SQL, чтобы решить, какой из них лучше всего соответствует вашим бизнес-потребностям. В число этих средств входят Помощник по миграции данных (DMA), Миграция Azure, Azure Database Migration Service, Помощник по миграции SQL Server (SSMA), Database Experimentation Assistant (DEA). '
ms.date: 03/15/2021
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajpo
ms.author: rajpo
ms.openlocfilehash: fc406a39dc0b5d1a80e4f357a28b6945da0bf453
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603314"
---
# <a name="compare-sql-data-migration-tools"></a>Сравнение средств переноса данных SQL

Корпорация Майкрософт предоставляет набор средств и служб, призванных помочь пользователям в переносе баз данных-источников различных типов в целевые среды. 

В этой статье приводится краткий обзор средств, доступных для миграции в SQL Server и Azure SQL. 

## <a name="azure-migrate"></a>Служба "Миграция Azure"

Миграция Azure — это центральный узел для обнаружения и оценки локальных серверов, инфраструктуры, приложений и данных, которые планируется перенести в Azure.  Миграция Azure обеспечивает единообразный перенос всех серверов, баз данных и приложений. 

С помощью службы "Миграция Azure" можно обнаружить все серверы SQL Server в центре обработки данных, оценить зависимости приложений, понять готовность серверов SQL Server к миграции в Azure SQL, а также получить рекомендации Майкрософт, оптимальный вариант развертывания Azure SQL и номер SKU, соответствующий требованиям ваших рабочих нагрузок к производительности.  Вы также можете примерно оценить стоимость использования этих баз данных в Azure SQL с учетом преимуществ лицензирования. 

Миграция Azure применяется в следующих сценариях: 
- определение и оценка среды данных SQL Server; 
- получение рекомендаций по развертыванию Azure SQL, целевых размеров и примерных месячных затрат;
- перенос всей среды данных в SQL Server на виртуальных машинах Azure методом lift-and-shift. 

Подробные сведения см. в [документации по Миграции Azure](/azure/migrate/). 

## <a name="data-migration-assistant-dma"></a>Помощник по миграции данных (DMA)

Помощник по миграции данных (DMA) помогает выполнить обновление до современной платформы данных путем обнаружения проблем совместимости, которые могут повлиять на функциональность базы данных в новой версии SQL Server или в Базе данных SQL Azure. DMA предлагает рекомендации по повышению производительности и надежности целевой среды, а также позволяет переместить схему, данные и неавтономные объекты с исходного сервера на целевой сервер.

Используйте DMA в следующих сценариях: 
- обновление SQL Server 2005 и более поздних версий до SQL Server 2012, SQL Server 2014, SQL Server 2016, SQL Server 2017 и SQL Server 2019 на базе Windows и Linux или до SQL Server в виртуальной машине Azure; 
- обнаружение проблем совместимости, которые могут повлиять на работу баз данных в более новой целевой версии SQL Server или в Azure SQL, а также получение рекомендаций по их устранению; 
- перенос схемы, данных и неавтономных объектов с исходного сервера в SQL Server или Azure SQL. 

Дополнительные сведения см. в [документации по Помощнику по миграции данных](../../dma/dma-overview.md). 

## <a name="sql-server-migration-assistant-ssma"></a>Помощник по миграции SQL Server (SSMA)

Помощник по миграции SQL Server (SSMA) — это средство для автоматизации переноса баз данных в SQL Server и Azure SQL из других ядер СУБД. 

Используйте SSMA в следующих сценариях:
- перенос баз данных Microsoft Access, DB2, MySQL, Oracle и SAP ASE в SQL Server;
- перенос баз данных Microsoft Access, DB2, MySQL, Oracle и SAP ASE в Azure SQL.

Дополнительные сведения см. в [документации по Помощнику по миграции SQL Server](../../ssma/sql-server-migration-assistant.md).

## <a name="azure-database-migration-service-dms"></a>Azure Database Migration Service (DMS)

Azure Database Migration Service обеспечивает бесперебойную миграцию из нескольких источников баз данных на платформы данных Azure с минимальным временем простоя.  Database Migration Service предоставляет устойчивый, надежный конвейер миграции, требующий минимального участия пользователя на протяжении всего процесса миграции. 

Используйте Database Migration Service в следующих сценариях:
- перенос баз данных в Azure SQL, особенно большого масштаба (с точки зрения количества и размера баз данных); 
- перенос баз данных в Базу данных Azure.

Дополнительные сведения см. в [документации по Azure Database Migration Service](/azure/dms/). 

## <a name="database-experimentation-assistant-dea"></a>Database Experimentation Assistant (DEA)

Database Experimentation Assistant (DEA) — это экспериментальное решение для обновления SQL Server. Оно может помочь оценить целевую версию SQL Server в плане возможности поддержки определенной рабочей нагрузки. Клиенты, выполняющие обновление с более ранних версий SQL Server (начиная с версии 2005) до более поздних могут использовать метрики анализа, предоставляемые этим средством.

Используйте Database Experimentation Assistant в следующих сценариях:
- сбор данных по рабочей нагрузке в исходной среде SQL Server и ее оценка на исходном сервере SQL Server для подготовки к миграции; 
- выявление ошибок совместимости и возможном снижении производительности запросов в сценарии миграции SQL Server. 

Дополнительные сведения см. в [документации по Database Experimentation Assistant](../../dea/database-experimentation-assistant-overview.md).


## <a name="quick-comparison"></a>Краткое сравнение

Для сравнения возможностей средств миграции SQL обратитесь к приведенной ниже таблице.


| Возможности |Служба "Миграция Azure"  |Помощник по миграции данных (DMA)  |Помощник по миграции SQL Server (SSMA)  | Azure Database Migration Service (DMS) | Database Experimentation Assistant (DEA)|
|---------|---------|---------|---------|---|---|
|Обнаружение и оценка среды данных SQL| В большом масштабе | Да |Нет |Нет|Нет|
|Перенос объектов SQL Server в Базу данных SQL или Управляемый экземпляр SQL| Нет| Да | Нет  | Да|Нет|
|Перенос баз данных SQL Server в SQL Server на виртуальной машине Azure методом lift-and-shift | Да | Нет | Нет | Нет| Нет |
|Перенос (и обновление) баз данных SQL Server в SQL Server на виртуальной машине Azure | Нет | Да| Нет | Нет | Нет| 
|Перенос объектов, отличных от SQL </br> (Oracle, Access, DB2 и т. д.) | Нет |Нет|Да|Нет|Нет|
|Перенос баз данных с открытым кодом </br> (MySQL, PostgreSQL, MariaDB и т. д.)| Нет | Нет | Нет | Да | Нет|
|Сравнение рабочих нагрузок на исходном и целевом серверах SQL Server |Нет|Нет|Нет|Нет|Да|
|||||||

## <a name="next-steps"></a>Дальнейшие действия

Приступите к переходу на [SQL Server](../../ssma/sql-server-migration-assistant.md) из другого ядра СУБД, выполните миграцию в [Azure SQL](/azure/azure-sql/migration-guides/) или оцените свою среду данных SQL с помощью службы [Миграция Azure](/azure/migrate/how-to-create-azure-sql-assessment). 


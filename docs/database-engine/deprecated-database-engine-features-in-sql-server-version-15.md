---
description: Нерекомендуемые функции ядра СУБД в [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Нерекомендуемые функции ядра СУБД в SQL Server 2019 | Документация Майкрософт
titleSuffix: SQL Server 2019
ms.custom: seo-lt-2019
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- deprecated changes 2019 [SQL Server]
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 4e322ff478e9ccdc031882e7c3b5b18fd8506e42
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186476"
---
# <a name="deprecated-database-engine-features-in-sql-server-2019-15x"></a>Нерекомендуемые функции ядра СУБД в SQL Server 2019 (15.x)

[!INCLUDE[sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE [sssql19-md](../includes/sssql19-md.md)] не объявляет нерекомендуемыми все функции, кроме устаревших в предыдущих выпусках:

- [[!INCLUDE [sssql17-md](../includes/sssql17-md.md)]](deprecated-database-engine-features-in-sql-server-2017.md)
- [[!INCLUDE [sssql16-md](../includes/sssql16-md.md)]](deprecated-database-engine-features-in-sql-server-2016.md)

## <a name="deprecation-guidelines"></a>Рекомендации по устареванию

Если функция помечена как нерекомендуемая, это означает следующее:

- Функция находится в режиме обслуживания. Новые изменения, в том числе касающиеся совместимости с новыми функциями, вноситься не будут.
- Мы стараемся не удалять нерекомендуемые функции из новых выпусков, чтобы упростить обновление. Однако иногда мы можем окончательно удалять функции из [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], если они препятствуют дальнейшим инновациям.
- Для новых задач разработки не используйте нерекомендуемые функции. Для существующих приложений, которые в настоящее время используют эти функции, запланируйте изменение как можно скорее.     

Наблюдать за использованием устаревших функций можно с помощью счетчика производительности объекта устаревших функций [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] или расширенных событий `deprecation_announcement` и `deprecation_final_support`. Дополнительные сведения см. в разделе [Использование объектов SQL Server](../relational-databases/performance-monitor/use-sql-server-objects.md).  

## <a name="query-deprecated-features"></a>Устаревшие возможности запроса

Значение этих счетчиков также можно получить, выполнив следующую инструкцию:  

```sql
SELECT * FROM sys.dm_os_performance_counters
WHERE object_name = 'SQLServer:Deprecated Features';
```

### <a name="see-also"></a>См. также

- [Критические изменения в функциях ядра СУБД в SQL Server 2019](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Нерекомендуемые функции ядра СУБД в SQL Server](../database-engine/discontinued-database-engine-functionality-in-sql-server.md)
- [Обратная совместимость компонента ядра СУБД SQL Server](./discontinued-database-engine-functionality-in-sql-server.md)

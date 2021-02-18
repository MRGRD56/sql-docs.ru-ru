---
description: Обозреватель решений
title: Обозреватель решений
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], solutions
- projects [SQL Server Management Studio], about projects
- SQL Server Management Studio [SQL Server], projects
- solutions [SQL Server Management Studio], about solutions
- SQL Server Management Studio [SQL Server], items
- items [SQL Server]
ms.assetid: 0df09843-0d4f-4925-bc6c-99265035a0c1
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 1ebf2ca3be5b92474c1f62052d1845cb3fb5b277
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349016"
---
# <a name="solution-explorer"></a>Обозреватель решений

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Панель обозревателя решений в среде [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] содержит контейнеры (проекты) для управления такими элементами, как скрипты базы данных, запросы, подключения к данным и файлы. Один или несколько связанных между собой проектов могут быть объединены в контейнер, который называется решением.  
  
Решение содержит один или несколько проектов, а также файлы и метаданные, которые позволяют определить решение как единое целое. Проект — это набор файлов и относящихся к ним метаданных, например сведений о соединении. Решения и проекты содержат элементы, которые представляют собой скрипты, запросы, сведения о соединениях и файлы, необходимые для создания решения базы данных.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
## <a name="benefits-of-using-solutions"></a>Преимущества использования решений  
Эти контейнеры позволяют:  
  
-   реализовать систему управление версиями для запросов и скриптов;  
  
-   управлять параметрами как решения в целом, так и конкретных проектов;  
  
-   облегчить работу с файлами и позволить сосредоточиться на работе с элементами, которые составляют решение базы данных;  
  
-   добавлять элементы одновременно к нескольким проектам решения либо ко всему решению, не указывая элемент в каждом из проектов;  
  
-   работать с различными файлами, формат которых не зависит от решений и проектов.  
  
Элементы, содержащиеся в проектах, зависят от типа проекта и от того, используется ли среда [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="related-tasks"></a>Связанные задачи  
Используйте следующие разделы для начала работы с решениями SQL Server.  
  
|Описание|Раздел|  
|-|-|    
|Описывает, как добавить один или несколько проектов в решение.|[Решения (среда SQL Server Management Studio)](../../ssms/solution/solutions-sql-server-management-studio.md)|  
|Описывает, как создать проект и добавить элементы, например скрипты и соединения.|[Проекты (SQL Server Management Studio)](../../ssms/solution/projects-sql-server-management-studio.md)|  
|Сведения о файлах, используемых средой [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] для управления решениями и файлами.|[Файлы для управления решениями и проектами](../../ssms/solution/files-that-manage-solutions-and-projects.md)|  
  

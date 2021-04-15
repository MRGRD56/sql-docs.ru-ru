---
title: Использование диаграмм обновления для изменения данных в SQLXML 4.0
description: Просмотрите сведения и примеры диаграмм обновления и их использование для изменения данных в SQLXML 4,0.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, updategrams
- data insertions [SQLXML]
- data deletions [SQLXML]
- updating data [SQLXML]
- modifying data [SQLXML]
- OPENXML function
- updategrams [SQLXML]
- database modifications [SQLXML]
- data updates [SQLXML]
- modifying databases
- data modifications [SQLXML]
- deleting data
- inserting data
ms.assetid: b8b3b892-cb73-41d0-b945-bce148d81d9b
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1396a2ac530e9052c3c2b276afa8daf9fe6b98a5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491469"
---
# <a name="using-updategrams-to-modify-data-in-sqlxml-40"></a>Использование диаграмм обновления для изменения данных в SQLXML 4.0
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Можно изменить (вставить, обновить или удалить) базу данных [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] из существующего XML-документа с помощью функции диаграмма обновления или OPENXML [!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
 В этом разделе содержатся сведения о диаграммах обновления и примеры их использования.  
  
## <a name="in-this-section"></a>в этом разделе  
 [Введение в диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/introduction-to-updategrams-sqlxml-4-0.md)  
 Содержит основные сведения и примеры диаграмм обновления.  
  
 [Указание схемы сопоставления с заметками в диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)  
 Содержит объяснение и примеры аннотированных схем сопоставления в диаграммах обновления.  
  
 [Обработка значений NULL &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/null-handling-sqlxml-4-0.md)  
 Содержит описание задания значений NULL для значений элементов и атрибутов.  
  
 [Вставка данных с помощью XML диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md)  
 Содержит описание и примеры использования диаграмм обновления для добавления данных.  
  
 [Удаление данных с помощью XML диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/deleting-data-using-xml-updategrams-sqlxml-4-0.md)  
 Содержит описание и примеры использования диаграмм обновления для удаления данных.  
  
 [Обновление данных с помощью XML диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/updating-data-using-xml-updategrams-sqlxml-4-0.md)  
 Содержит описание и примеры использования диаграмм обновления для изменения существующих данных.  
  
 [Передача параметров в диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/passing-parameters-to-updategrams-sqlxml-4-0.md)  
 Содержит описание и примеры передачи параметров в диаграммы обновления.  
  
 [Обработка проблем параллелизма базы данных в диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/handling-database-concurrency-issues-in-updategrams-sqlxml-4-0.md)  
 Содержит описание и примеры возможных уровней защиты для решения проблем параллелизма в диаграммах обновления.  
  
 [Примеры приложений диаграмма обновления &#40;SQLXML 4,0&#41;]()  
 Содержит образцы приложений, использующих диаграммы обновления.  
  
 [Рекомендации и ограничения для XML-диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/guidelines-and-limitations-of-xml-updategrams-sqlxml-4-0.md)  
 Содержит перечень фактов, о которых необходимо помнить при работе с диаграммами обновления.  
  

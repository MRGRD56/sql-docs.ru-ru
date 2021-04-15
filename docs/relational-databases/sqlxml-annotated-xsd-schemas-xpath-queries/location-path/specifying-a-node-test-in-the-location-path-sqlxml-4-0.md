---
title: Указание проверки узла в пути расположения (SQLXML)
description: Узнайте, как указать проверку узла в пути к расположению запроса XPath для SQLXML 4,0.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], location paths
- principal node types [SQLXML]
- node tests [SQLXML]
- location path for XPath query
ms.assetid: f46c30bf-1e24-4435-9ac2-f8ba43a8ff94
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 365441f7ebd67bc1083149de9bb994910fdef180
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491829"
---
# <a name="specifying-a-node-test-in-the-location-path-sqlxml-40"></a>Задание проверки узла в пути доступа (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Проверка узла задает тип узла, выбранного на шаге доступа. Каждая ось (**дочерний**, **родительский**, **атрибут** или **Self**) имеет тип основного узла. Для оси **атрибута** типом основного узла является **\<attribute>** . Для **родительских**, **дочерних** и **собственных** осей тип основного узла — **\<element>** .  
  
> [!NOTE]  
>  Шаблон проверки узла * (например `child::*`) не поддерживается.  
  
## <a name="node-test-example-1"></a>Проверка узла: Пример 1  
 Путь к расположению `child::Customer` выбирает **\<Customer>** дочерние элементы узла контекста.  
  
 В следующем примере элемент `child` является осью, а `Customer` является проверкой узла. Тип основного узла для **дочерней** оси — **\<element>** . Поэтому проверка узла имеет значение TRUE, если **\<Customer>** узел является **\<element>** узлом. Если узел контекста не имеет **\<Customer>** дочерних элементов, возвращается пустой набор узлов.  
  
## <a name="node-test-example-2"></a>Проверка узла: пример 2  
 Путь к расположению `attribute::CustomerID` выбирает атрибут **CustomerID** узла контекста.  
  
 В этом примере элемент `attribute` является осью, а `CustomerID` является проверкой узла. Тип основного узла оси **атрибута** — **\<attribute>** . Поэтому проверка узла имеет значение TRUE, если **CustomerID** является **\<attribute>** узлом. Если узел контекста не имеет **CustomerID**, возвращается пустой набор узлов.  
  
> [!NOTE]  
>  В этой реализации XPath, если шаг расположения ссылается на **\<element>** или **\<attribute>** тип, который не объявлен в схеме, создается ошибка. Это отличается от реализации XPath в MSXML, где возвращается пустое множество узлов.  
  
## <a name="abbreviated-syntax-for-the-axes"></a>Сокращенный синтаксис осей  
 Поддерживается следующий сокращенный синтаксис пути доступа.  
  
-   `attribute::` можно сократить до `@`.  
  
     Путь доступа `Customer[@CustomerID="ALFKI"]` аналогичен выражению `child::Customer[attribute::CustomerID="ALFKI"]`.  
  
-   `child::` можно исключить из шага определения расположения данных.  
  
     Таким же значением является **дочерняя** ось по умолчанию. Путь доступа `Customer/Order` аналогичен выражению `child::Customer/child::Order`.  
  
-   `self::node()` можно сократить до одной точки (.), а `parent::node()` можно сократить до двух точек (..).  
  
  

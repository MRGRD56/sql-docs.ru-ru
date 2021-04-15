---
title: Указание оси (SQLXML)
description: Узнайте, как указать ось в запросе на языке SQLXML 4,0 XPath, указывающий связь дерева между узлами, выбранными на шаге расположение, и узлом контекста.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], axes
- XPath queries [SQLXML], location paths
- self axis
- attribute axis [SQLXML]
- axis [SQLXML]
- child axis
- parent axis
- location path for XPath query
- axes [SQLXML]
ms.assetid: 65631795-3389-40cf-90ea-85e9438956c5
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb31ec22517e7f43aea4631e7ec3fce22ea6041a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490487"
---
# <a name="specifying-an-axis-sqlxml-40"></a>Определение оси (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
    
-   Ось определяет древовидную связь между узлами, которые выбираются шагом доступа, и контекстными узлами. Поддерживаются следующие оси:  **дочерний элемент**  
  
     Содержит дочерний элемент узла контекста.  
  
     Следующее выражение XPath (путь расположения) выбирает из текущего контекстного узла все **\<Customer>** дочерние элементы.  
  
    ```  
    child::Customer  
    ```  
  
     В следующем запросе XPath `child` является осью. `Customer` является проверкой узла.  
  
-   **parent**  
  
     Содержит родительский элемент контекстного узла.  
  
     Следующее выражение XPath выбирает все **\<Customer>** родительские **\<Order>** элементы дочерних элементов:  
  
    ```  
    child::Customer/child::Order[parent::Customer/@customerID="ALFKI"]  
    ```  
  
     Это аналогично указанию `child::Customer`. В данном запросе XPath `child` и `parent` являются осями. `Customer` и `Order` являются проверками узла.  
  
-   **версию**  
  
     Содержит атрибут узла контекста.  
  
     Следующее выражение XPath выбирает атрибут **CustomerID** узла контекста:  
  
    ```  
    attribute::CustomerID  
    ```  
  
-   **самообслуживания**  
  
     Содержит сам узел контекста.  
  
     Следующее выражение XPath выбирает текущий узел, если он является **\<Order>** узлом:  
  
    ```  
    self::Order  
    ```  
  
     В следующем запросе XPath `self` является осью, а `Order` — проверкой узла.  
  
  

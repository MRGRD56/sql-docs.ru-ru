---
title: Обработка значений NULL (SQLXML)
description: 'Узнайте, как атрибуты или элементы NULL могут быть указаны в SQLXML 4,0 диаграмма обновления с помощью атрибута атрибута updg: NullValue.'
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- updg:nullvalue attribute
- updategrams [SQLXML], null values
- nullvalue attribute
- null values [SQLXML]
ms.assetid: 5e11eebb-d94e-4ce6-a6d0-870225706bc1
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3fac9c5de10981dbb8afff517a106135ccca82f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484826"
---
# <a name="null-handling-sqlxml-40"></a>Обработка значений NULL (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Синтаксис XML определяет значение NULL как отсутствие. (Например, если атрибут или элемент имеет значение NULL, этот атрибут или элемент отсутствует в XML-документе.) В [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML атрибут **атрибута updg: NullValue** позволяет указать значение NULL для элемента или значения атрибута.  
  
 Например, следующий диаграмма обновления гарантирует, что значение **заголовка** контакта со значением **ContactID** , равным 64, будет равно null, а затем обновите значение **заголовка** на «Mr». для этого контакта.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync updg:nullvalue="IsNULL"  >  
    <updg:before>  
       <Person.Contact ContactID="64" Title="IsNULL" />  
    </updg:before>  
    <updg:after>  
       <Person.Contact ContactID="64" Title="Mr." />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Когда параметры передаются диаграмме обновления, значение NULL может передаваться как значение параметра. Это делается путем указания атрибута **NullValue** в **\<updg:header>** блоке. Пример см. в разделе [Передача параметров в диаграмм обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/passing-parameters-to-updategrams-sqlxml-4-0.md).  
  
## <a name="see-also"></a>См. также:  
 [Вопросы безопасности диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  

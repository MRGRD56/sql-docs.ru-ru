---
title: 'Элемент &lt;xsd: redefine&gt; | Документация Майкрософт'
description: Сведения о поддержке элемента W3C XSD redefine и обновлении схемы XML или ее компонентов.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- xsd:redefine element
ms.assetid: 5f3e9b65-f10e-4db2-a62c-b270ac11d04e
author: rothja
ms.author: jroth
ms.openlocfilehash: 6137e814b85ee3670bd10711962ba3cec47dd43e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487420"
---
# <a name="the-ltxsdredefinegt-element"></a>Элемент &lt;xsd:redefine&gt;
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Элемент W3C XSD **redefine** обеспечивает поддержку переопределения компонентов схемы. Однако поддержка этой директивы является потенциально затратной в смысле производительности, а также требует, чтобы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] повторно проверялись все экземпляры типа данных **xml** , связанные с переопределенной схемой. Поэтому [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает этот элемент. XML-схемы, которые включают элемент **\<xsd:redefine>** , будут отклонены сервером.  
  
 Чтобы обновить схему или ее компоненты, вместо этого можно сделать следующее.  
  
1.  Создайте новую коллекцию XML-схем с измененными компонентами схемы.  
  
2.  Повторно введите все типы данных **xml** (XML DT), в которых применяется переопределяемая коллекция схем XML, таким образом, чтобы в них использовалась новая коллекция. Для повторного ввода столбцов воспользуйтесь параметром ALTER COLUMN команды ALTER TABLE или измените ограничения коллекции XML-схемы по переменным или параметрам.  
  
3.  Удалите старую версию коллекции XML-схем.  

## <a name="see-also"></a>См. также:  
 [Требования и ограничения для коллекций схем XML на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  

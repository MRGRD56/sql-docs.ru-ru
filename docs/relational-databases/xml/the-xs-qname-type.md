---
title: Тип xs:QName | Документация Майкрософт
description: Узнайте, как использовать тип xs:QName в качестве элемента ограничения схемы XML или как тип элемента объединения.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- xs:QName type
ms.assetid: 72c5bfde-b0b2-4372-bf36-97ba930dea06
author: rothja
ms.author: jroth
ms.openlocfilehash: c620a1134795cfede3d706ae5e624d2e6ddbb67e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487434"
---
# <a name="the-xsqname-type"></a>Тип xs:QName
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает типы, унаследованные из **xs:QName** и использующие элемент ограничения XML-схемы. Кроме того, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в настоящее время не поддерживает типы объединений с **QName** в качестве типа элемента.  
  
## <a name="example"></a>Пример  
 Следующая инструкция `CREATE XML SCHEMA COLLECTION` не сможет загрузить XML-схему, так как указан тип `xs:QName` в качестве типа объединения:  
  
```  
CREATE XML SCHEMA COLLECTION QNameLimitation1 AS N'  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  
    <xs:simpleType name="myUnion">  
        <xs:union memberTypes="xs:int xs:QName"/>  
    </xs:simpleType>  
</xs:schema>'  
GO  
  
CREATE XML SCHEMA COLLECTION QNameLimitation2 AS N'  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">  
    <xs:simpleType name="myUnion">  
        <xs:union memberTypes="xs:integer">  
   <xs:simpleType>  
    <xs:list itemType="xs:QName"/>  
   </xs:simpleType>  
  </xs:union>  
    </xs:simpleType>  
</xs:schema>'  
GO  
```  
  
 Обе инструкции потерпят неудачу с сообщением об ошибке.  
  
## <a name="see-also"></a>См. также:  
 [Требования и ограничения для коллекций схем XML на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  

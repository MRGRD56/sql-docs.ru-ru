---
title: Ограничение однозначного соответствия примитивов | Документация Майкрософт
description: Узнайте, как правило ограничения однозначного соответствия примитивов (UPA) отклоняет схему XSD, если она содержит тип с потенциально неоднозначной моделью содержимого.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
f1_keywords:
- unique particle attribution
helpviewer_keywords:
- schema collections [SQL Server], unique particle attribution
- XML schema collections [SQL Server], unique particle attribution
- UPA constraint rule
- unique particle attribution constraint rule
ms.assetid: 6bb879e9-a5ee-402e-94e4-fe8cec5966b0
author: rothja
ms.author: jroth
ms.openlocfilehash: 8bc31e58dc78ba54f7e899dc2e27523104c9805a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487400"
---
# <a name="unique-particle-attribution-constraint"></a>Ограничение однозначного соответствия примитивов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В XSD сложные модели содержимого ограничены правилом ограничения однозначного соответствия примитивов. Это правило требует, чтобы каждый элемент в экземпляре документа однозначно соответствовал единственному примитиву `<xsd:element>` или `<xsd:any>` в родительской модели содержимого. Любая схема, которая содержит тип с потенциально неоднозначной моделью содержимого, отклоняется.  
  
 Чаще всего неоднозначность возникает при использовании символов-шаблонов `<xsd:any>` и примитивов, имеющих переменные диапазоны, например minOccurs < maxOccurs. Приведенная ниже модель содержимого является неоднозначной, так как элемент <`e1`> может соответствовать элементам `<xsd:element>` и `<xsd:any>`.  
  
```  
<xsd:element name="root">  
    <xsd:complexType>  
        <xsd:choice>  
            <xsd:element name="e1"/>  
            <xsd:any namespace="##any"/>  
        </xsd:choice>  
    </xsd:complexType>  
</xsd:element>  
```  
  
 Следующая модель содержимого также является неоднозначной:  
  
```  
<xsd:element name="root">  
    <xsd:complexType>  
        <xsd:sequence>  
            <xsd:element name="e1" maxOccurs="2"/>  
            <xsd:element name="e2" minOccurs="0"/>  
            <xsd:element name="e1"/>  
        </xsd:sequence>  
    </xsd:complexType>  
</xsd:element>  
```  
  
 В отличие от документа `<root><e1/><e2/><e1/></root>` , документ `<root><e1/><e1/></root>` является неоднозначным, поскольку неясно, к какому элементу `<xsd:element>` относится второй элемент `<e1/>` . Даже если документы можно проверить однозначно, схема будет отклонена, поскольку возможность неоднозначности существует.  
  
 Чтобы модель содержимого была допустимой, должна существовать возможность однозначно проверить любой экземпляр, не заглядывая вперед. Например, рассмотрим следующую модель содержимого:  
  
```  
<xsd:element name="root">  
    <xsd:complexType>  
        <xsd:choice>  
           <xsd:sequence>  
               <xsd:element name="e1"/>  
               <xsd:element name="e2"/>  
           </xsd:sequence>  
           <xsd:sequence>  
               <xsd:element name="e1"/>  
               <xsd:element name="e3"/>  
           </xsd:sequence>  
       </xsd:choice>  
    </xsd:complexType>  
</xsd:element>  
```  
  
 Для такого документа, как `<root><e1/><e3/></root>`, последовательность `<e1/><e3/>` однозначно совпадает со вторым элементом `<xsd:sequence>`. Тем не менее, поскольку элемент `<xsd:element>` , к которому относится `<e1/>` , нельзя определить, не заглядывая вперед в `<e3/>`, модель содержимого нарушает правило ограничения однозначного соответствия примитивов.  
  
## <a name="finding-more-information"></a>Дополнительные сведения  
 Следующий документ опубликован консорциумом World Wide Web (W3C) и содержит техническое описание ограничения однозначного соответствия примитивов:  
  
 "Схема XML. Часть 1: структуры, второе издание, W3C Proposed Edited Recommendation":  
  
-   раздел 3.8.6. Ограничения компонентов схемы группы моделей  
  
-   Приложение H. Анализ ограничения однозначного соответствия примитивов (ненормативный)  
  
 Чтобы просмотреть документ, перейдите по адресу [http://www.w3.org/TR/xmlschema-1](https://go.microsoft.com/fwlink/?linkid=48881).  
  
## <a name="see-also"></a>См. также:  
 [Коллекции XML-схем (SQL Server)](../../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  

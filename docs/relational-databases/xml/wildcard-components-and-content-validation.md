---
title: Компоненты-шаблоны и проверка достоверности содержимого | Документация Майкрософт
description: Узнайте, как используются компоненты-шаблоны XML-элемента и атрибута для увеличения гибкости с точки зрения того, что может присутствовать в модели содержимого.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- wildcard components [XML]
- content validation [XML]
ms.assetid: ffa7d974-3645-446c-8425-f0b22b6b060a
author: rothja
ms.author: jroth
ms.openlocfilehash: d89150453d767dcdaff998e65ee8efa452e14820
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107486669"
---
# <a name="wildcard-components-and-content-validation"></a>Компоненты-шаблоны и проверка достоверности содержимого
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Компоненты-шаблоны используются для увеличения гибкости в том, в чем это разрешено для модели содержимого. Эти компоненты поддерживаются в языке XSD следующими способами.  
  
-   Компоненты-шаблоны элемента. Они представлены элементом **\<xsd:any>** .  
  
-   Компоненты-шаблоны атрибута. Они представлены элементом **\<xsd:anyAttribute>** .  
  
 Оба элемента подстановочного знака, **\<xsd:any>** и **\<xsd:anyAttribute>** , поддерживают использование атрибута **processContents**. Это позволяет задать значение, указывающее, как приложения XML обрабатывают проверку правильности содержимого документа, связанную с этими элементами символа-шаблона. Они являются различными значениями, и их действие заключается в следующем.  
  
-   Значение **strict** указывает, что содержимое полностью подтверждено.  
  
-   Значение **skip** указывает, что содержимое не подтверждено.  
  
-   Значение **lax** указывает, что только элементы и атрибуты, для которых определения схемы являются доступными, будут подтверждены.  
  
## <a name="lax-validation-and-xsanytype-elements"></a>Нестрогая проверка и элементы xs:anyType  
 В спецификации XML-схемы для элементов типа **anyType** используется **нестрогая** проверка. Поскольку в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] нестрогая проверка не поддерживалась, к элементам **anyType** применялась строгая проверка. Начиная с версии [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], предоставляется поддержка нестрогой проверки. Содержимое элементов типа **anyType** будет осуществляться при помощи нестрогой проверки.  
  
 Следующий пример демонстрирует использование нестрогой проверки. Элемент `e` схемы принадлежит типу **anyType** . В примере создаются типизированные переменные **xml** и демонстрируется нестрогая проверка элемента типа **anyType** .  
  
```  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"   
        targetNamespace="http://ns">  
   <element name="e" type="anyType"/>  
   <element name="a" type="byte"/>  
   <element name="b" type="string"/>  
 </schema>'  
GO  
```  
  
 Выполнение следующего примера завершится без ошибок, поскольку успешно завершается проверка `<e>` :  
  
```  
DECLARE @var XML(SC)  
SET @var = '<e xmlns="http://ns"><a>1</a><b>data</b></e>'  
GO  
```  
  
 Следующий пример завершается успешно. Экземпляр принимается, несмотря на то, что элементы `<c>` в схеме не определены.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<e xmlns="http://ns"><a>1</a><c>Wrong</c><b>data</b></e>'  
GO  
```  
  
 В следующем примере экземпляр XML отклонен, потому что определение элемента `<a>` не допускает строковых значений.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<e xmlns="http://ns"><a>Wrong</a><b>data</b></e>'  
SELECT @var  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Требования и ограничения для коллекций схем XML на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  

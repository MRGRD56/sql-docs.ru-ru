---
title: Аннотации XSD (SQLXML)
description: Просмотрите список аннотаций XSD (SQLXML 4,0), появившихся в SQL Server 2005 (9. x), с сравнением аннотаций XDR, появившихся в SQL Server 2000 (8. x).
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, annotations listed
- XSD schemas [SQLXML], annotations
ms.assetid: c62a6785-8d66-40a2-9c5d-80c73d600a3b
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0b2b46baac2bf4c68b8b4692fa5e1e3b592f2040
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490682"
---
# <a name="xsd-annotations-sqlxml-40"></a>Заметки XSD (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В таблице перечисляются заметки XSD, введенные в версии [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и проводится их сравнение с заметками XDR, введенными в версии [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)].  
  
|Заметка XSD|Описание|Ссылка на раздел|Заметка XDR|  
|--------------------|-----------------|----------------|--------------------|  
|**sql:encode**|Позволяет запросить URI-ссылку, когда элемент или атрибут XML сопоставлен с BLOB-столбцом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. С помощью этой URI-ссылки можно потом возвратить данные типа BLOB.|[Запрос URL-ссылок на данные большого двоичного объекта с помощью SQL: Encoded &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/requesting-url-references-to-blob-data-using-sql-encode-sqlxml-4-0.md)|**Кодирование URL-адреса**|  
|**SQL: GUID**|Позволяет указать, нужно ли использовать значение идентификатора GUID, созданное [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], или значение, заданное в диаграмме обновления для данного столбца.|[Использование заметок sql:identity и sql:guid](../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-the-sql-identity-and-sql-guid-annotations.md)|Не поддерживается|  
|**sql:hide**|Прячет элемент или атрибут, заданные в схеме результирующего XML-документа.|[Скрытие элементов и атрибутов с помощью sql:hide](../../relational-databases/sqlxml-annotated-xsd-schemas-using/hiding-elements-and-attributes-by-using-sql-hide.md)|Не поддерживается|  
|**sql:identity**|Может быть задан для любого узла, сопоставляемого со столбцом типа IDENTITY. Значение, заданное для этой аннотации, определяет, каким образом будет изменяться соответствующий столбец типа IDENTITY в базе данных.|[Использование заметок sql:identity и sql:guid](../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-the-sql-identity-and-sql-guid-annotations.md)|Не поддерживается|  
|**sql:inverse**|Указывает логике диаграмма обновления на обратную интерпретацию связи типа «родители-потомки», заданной с помощью **\<sql:relationship>** .|[Указание атрибута SQL: инверсии в SQL: relationship &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md)|Не поддерживается|  
|**sql:is-constant**|Создает XML-элемент, который не сопоставлен ни с одной из таблиц. Этот элемент появляется в выходных данных запроса.|[Создание элементов констант с помощью SQL: является константой &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-constant-elements-using-sql-is-constant-sqlxml-4-0.md)|Аналогично|  
|**sql:key-fields**|Позволяет задавать определения столбцов, которые служат уникальными идентификаторами строк в таблице.|[Определение ключевых столбцов с помощью SQL: Key-Fields &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md)|Аналогично|  
|**sql:limit-field**<br /><br /> **sql:limit-value**|Позволяет ограничить значения, возвращаемые на основе ограничения значений.|[Фильтрация значений с помощью SQL: limit-field и SQL: limit-value &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/filtering-values-using-sql-limit-field-and-sql-limit-value-sqlxml-4-0.md)|Аналогично|  
|**sql:mapped**|Позволяет исключать элементы схемы из результата.|[Исключение элементов схемы из результирующего XML-документа с помощью SQL: сопоставлено &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/excluding-schema-elements-from-the-xml-document-using-sql-mapped.md)|**Map-поле**|  
|**sql:max-depth**|Позволяет указать глубину рекурсивных связей, заданных в схеме.|[Задание глубины рекурсивных связей с использованием sql:max-depth](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-depth-in-recursive-relationships-by-using-sql-max-depth.md)|Не поддерживается|  
|**sql:overflow-field**|Определяет столбец базы данных, в котором содержатся данные переполнения.|[Получение невостребованных данных с помощью SQL: overflow-поля &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/retrieving-unconsumed-data-using-the-sql-overflow-field-sqlxml-4-0.md)|Аналогично|  
|**sql:prefix**|Создает допустимые XML ID, IDREF и IDREFS. Предваряет значения ID, IDREF и IDREFS строкой.|[Создание допустимых атрибутов типа ID, IDREF и IDREFS с помощью SQL: prefix &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-valid-id-idref-and-idrefs-type-attributes-using-sql-prefix-sqlxml-4-0.md)|Аналогично|  
|**sql:relationship**|Определяет связи между XML-элементами. Атрибуты " **родительский**", " **дочерний", "** **родительский ключ**" и " **дочерний ключ** " используются для установления связи.|[Указание связей с помощью SQL: relationship &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md)|Имена атрибутов отличаются:<br /><br /> **ключевое отношение**<br /><br /> **Внешняя связь**<br /><br /> **key**<br /><br /> **внешний ключ**|  
|**sql:use-cdata**|Позволяет задавать использование разделов CDATA для определенных элементов XML-документа.|[Создание разделов CDATA с помощью SQL: use-CDATA &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-cdata-sections-using-sql-use-cdata-sqlxml-4-0.md)|Аналогично|  
  
> [!NOTE]  
>  Собственный атрибут **targetNamespace** в XSD заменяет заметку **target-namespace** , представленную в [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] схеме сопоставления XDR.  
  
## <a name="see-also"></a>См. также  
 [Указание целевого пространства имен с помощью атрибута targetNamespace &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-4-0.md)  
  
  

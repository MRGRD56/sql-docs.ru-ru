---
title: Использование заметок в схемах XSD (SQLXML)
description: Узнайте, как использовать аннотации, поддерживаемые языком XSD-схемы в SQLXML 4,0, для указания сопоставления XML и реляционности в схеме XSD.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XSD schemas, about annotated XSD schemas
- annotations [SQLXML]
- relationships [SQLXML]
- relationships [SQLXML], hierarchical relationships
- hierarchical relationships [SQLXML]
- mapping schema [SQLXML], scenarios for using
ms.assetid: 78f318a4-7a36-473b-9852-a4bae6940ce5
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 65ae1ddc1ce718e2fad4ef06941b504fe461a7f0
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490767"
---
# <a name="using-annotations-in-xsd-schemas-sqlxml-40"></a>Использование заметок в схемах XSD (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQLXML 4.0 язык схем XSD поддерживает заметки аналогично заметкам, введенным в языке схем XDR. В XSD введены дополнительные заметки, не поддерживаемые в XDR.  
  
 Их можно использовать в схеме XSD для задания сопоставлений данных XML c реляционными данными. Сюда входит сопоставление элементов и атрибутов схемы XSD с таблицами (представлениями) и столбцами базы данных.  
  
 Если заметки не заданы, будет использоваться сопоставление по умолчанию. По умолчанию элемент XSD сложного типа сопоставляется имени таблицы или представления в заданной базе данных, а элемент или атрибут простого типа — одноименному столбцу.  
  
 Эти заметки также можно использовать для указания иерархических связей в формате XML, представляющих связи в базе данных, поскольку схема XSD представляет собой просто XML-представление реляционных данных.  
  
 Этот раздел представляет описания заметок, которые можно использовать со схемами XSD, и примеры их использования.  
  
> [!NOTE]  
>  Все примеры в этом разделе задают простые запросы XPath к схеме XSD с заметками, описанной в каждом из примеров. Предполагается, что читатель знаком с языком XPath.  
  
## <a name="in-this-section"></a>в этом разделе  
 [Аннотации XSD &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/xsd-annotations-sqlxml-4-0.md)  
 Перечисление заметок, которые можно использовать со схемами XSD, и эквивалентных им заметок для XDR.  
  
 [Сопоставление элементов и атрибутов XSD с таблицами и столбцами по умолчанию &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)  
 Рассказ о сопоставлении по умолчанию и примеры задач, использующих такое сопоставление.  
  
 [Явное сопоставление элементов и атрибутов XSD с таблицами и столбцами &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/explicit-mapping-xsd-elements-and-attributes-to-tables-and-columns.md)  
 Описание явного сопоставления с заметками **SQL: relation** и **SQL: field** , а также примеры.  
  
 [Указание связей с помощью SQL: relationship &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: relationship** .  
  
 [Указание атрибута SQL: инверсии в SQL: relationship &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md)  
 Описывает **обратную аннотацию SQL:** .  
  
 [Создание элементов констант с помощью SQL: является константой &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-constant-elements-using-sql-is-constant-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: имеет-Constant** .  
  
 [Исключение элементов схемы из результирующего XML-документа с помощью SQL: сопоставлено &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/excluding-schema-elements-from-the-xml-document-using-sql-mapped.md)  
 Содержит описание и примеры **сопоставленной аннотации SQL:** .  
  
 [Фильтрация значений с помощью SQL: limit-field и SQL: limit-value &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/filtering-values-using-sql-limit-field-and-sql-limit-value-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотаций **SQL: limit-field** и **SQL: limit-value** .  
  
 [Определение ключевых столбцов с помощью SQL: Key-Fields &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: Key-Fields** .  
  
 [Указание целевого пространства имен с помощью атрибута targetNamespace &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-4-0.md)  
 Описывает и предоставляет примеры атрибута **targetNamespace** .  
  
 [Создание допустимых атрибутов типа ID, IDREF и IDREFS с помощью SQL: prefix &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-valid-id-idref-and-idrefs-type-attributes-using-sql-prefix-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: prefix** .  
  
 [Преобразования типов данных и аннотации SQL: DataType &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: DataType** .  
  
 [Сопоставление типов данных XSD с типами данных XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md)  
 Содержит таблицу сравнения типов данных XSD, XDR и XPath с перечислением соответствующих преобразований [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Создание разделов CDATA с помощью SQL: use-CDATA &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-cdata-sections-using-sql-use-cdata-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: use-Data** .  
  
 [Запрос URL-ссылок на данные большого двоичного объекта с помощью SQL: Encoded &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/requesting-url-references-to-blob-data-using-sql-encode-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: Encoded** .  
  
 [Получение невостребованных данных с помощью SQL: overflow-поля &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/retrieving-unconsumed-data-using-the-sql-overflow-field-sqlxml-4-0.md)  
 Описывает и предоставляет примеры аннотации **SQL: overflow-поля** .  
  
 [Скрытие элементов и атрибутов с помощью sql:hide](../../relational-databases/sqlxml-annotated-xsd-schemas-using/hiding-elements-and-attributes-by-using-sql-hide.md)  
 Описывает и предоставляет примеры аннотации **SQL: Hide** .  
  
 [Использование заметок sql:identity и sql:guid](../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-the-sql-identity-and-sql-guid-annotations.md)  
 Описывает и предоставляет примеры заметок **SQL: Identity** и **SQL: GUID** .  
  
 [Задание глубины рекурсивных связей с использованием sql:max-depth](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-depth-in-recursive-relationships-by-using-sql-max-depth.md)  
 Описывает и предоставляет примеры аннотации **SQL: max-depth** .  
  
## <a name="see-also"></a>См. также  
 [Рекомендации по безопасности схемы с заметками &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/annotated-schema-security-considerations-sqlxml-4-0.md)  
  
  

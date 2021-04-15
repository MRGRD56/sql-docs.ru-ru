---
description: Язык модификации XML-данных (XML DML)
title: Язык модификации XML-данных (XML DML)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- modifying data [SQL Server], XML DML
- XML [SQL Server], DML
- XML DML [SQL Server]
- data modification language [XML DML]
- data modifications [XML DML]
- DML [XML in SQL Server]
- XQuery, XML DML
- xml data type [SQL Server], XML DML
ms.assetid: 20ce50d2-c07b-4e41-93a7-1380d2cd49cb
author: rothja
ms.author: jroth
ms.openlocfilehash: 641c2b6cc4fb72670a1d90716f59b91f1ee2af01
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488069"
---
# <a name="xml-data-modification-language-xml-dml"></a>Язык модификации XML-данных (XML DML)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Язык модификации XML-данных (XML DML) является расширением языка XQuery. Как определено консорциумом W3C, в языке XQuery отсутствует часть, касающаяся манипулирования данными (DML). Язык XML DML, представленный в этом разделе, вместе с языком XQuery обеспечивает полнофункциональный язык запросов и модификации данных, который можно использовать для данных типа **xml**.  
  
 В язык XML DML добавлены следующие ключевые слова с учетом регистра, которые отсутствуют в языке XQuery:  
  
-   **insert**  
  
-   **delete**  
  
-   **replace value of**  
  
 Как описывается в разделе [Столбцы и типы данных XML (SQL Server)](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md), можно создавать переменные и столбцы типа  **xml** и присваивать им XML-документы или фрагменты XML-документов. Чтобы модифицировать или обновить эти экземпляры XML, выполните следующее:  
  
-   Используйте [метод modify() типа данных XML](../../t-sql/xml/modify-method-xml-data-type.md) для типа данных **xml**.  
  
-   Задайте соответствующие инструкции XML DML внутри метода **modify()** .  
  
 Заметим, что существуют атрибуты, которые нельзя добавить, удалить или изменить их значение. Пример:  
  
-   Для типизированного или нетипизированного **xml** это атрибуты **xmlns**, **xmlns:\* *_ и _* xml:base**.  
  
-   Для типизированного **xml** это атрибуты **xsi:nil** и **xsi:type**.  
  
 К другим ограничениям также относятся:  
  
-   Для типизированного или нетипизированного **xml** невозможно успешное добавление атрибута **xml:base**.  
  
-   Для типизированного **xml** невозможно успешное удаление или модификация атрибута **xsi:nil**. Для нетипизированного **xml** можно удалить атрибут или модифицировать его значение.  
  
-   Для типизированного **xml** невозможна успешная модификация атрибута **xs:type**. Для нетипизированного **xml** можно модифицировать значение атрибута.  
  
 При модификации экземпляра типизированного xml конечный формат должен являться допустимым экземпляром типа, иначе возвращается ошибка проверки.  
  
## <a name="see-also"></a>См. также:  
 [insert (XML DML)](../../t-sql/xml/insert-xml-dml.md)   
 [delete (XML DML)](../../t-sql/xml/delete-xml-dml.md)   
 [replace value of (XML DML)](../../t-sql/xml/replace-value-of-xml-dml.md)   
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [Методы для типа данных XML](../../t-sql/xml/xml-data-type-methods.md)  
  
  

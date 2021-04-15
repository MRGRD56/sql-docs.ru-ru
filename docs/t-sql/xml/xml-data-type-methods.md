---
description: Методы типа данных XML
title: Методы типа данных XML
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], methods
- methods [XML in SQL Server]
ms.assetid: d112b9c9-be9f-435c-a9e6-d21b65778fb7
author: rothja
ms.author: jroth
ms.openlocfilehash: 8f693edb056ee2a4880a19956d6165495d80c4af
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490232"
---
# <a name="xml-data-type-methods"></a>Методы типа данных XML
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Методы типа данных **xml** можно использовать для выполнения запроса к экземпляру XML, хранящемуся в переменной или столбце типа **xml**. Подразделы, входящие в данный раздел, описывают использование методов типа данных **xml**.  
  
## <a name="in-this-section"></a>в этом разделе  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[query (метод) (тип данных xml)](../../t-sql/xml/query-method-xml-data-type.md)|Описывает, как использовать метод query() для запроса к экземпляру XML.|  
|[value (метод) (тип данных xml)](../../t-sql/xml/value-method-xml-data-type.md)|Описывает, как использовать метод value() для получения значения типа SQL из экземпляра XML.|  
|[exist (метод) (тип данных xml)](../../t-sql/xml/exist-method-xml-data-type.md)|Описывает, как использовать метод exist(), чтобы определить, вернул ли запрос непустой результат.|  
|[modify (метод) (тип данных xml)](../../t-sql/xml/modify-method-xml-data-type.md)|Описывает, как использовать метод modify() и указывать инструкции [языка модификации XML-данных (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md) для выполнения обновлений.|  
|[nodes (метод) (тип данных xml)](../../t-sql/xml/nodes-method-xml-data-type.md)|Описывает, как использовать метод nodes() и разделять XML на несколько строк для распространения XML-документов по наборам строк.|  
|[Привязка реляционных данных внутри данных XML](../../t-sql/xml/binding-relational-data-inside-xml-data.md)|Описывает, как выполнить внутри XML привязку данных, не относящихся к XML.|  
|[Рекомендации по использованию методов для типа данных XML](../../t-sql/xml/guidelines-for-using-xml-data-type-methods.md)|Описывает правила использования методов типа данных **xml**.|  
  
 Эти методы вызываются при помощи синтаксиса вызова метода определяемого пользователем типа. Пример:  
  
```sql
SELECT XmlCol.query(' ... ')  
FROM Table  
```  
  
> [!NOTE]  
>  Методы **query()** , **value()** и **exist()** типа данных **xml** возвращают значение NULL при применении к неопределенному (NULL) экземпляру XML. Кроме того, метод **modify()** ничего не возвращает, а метод **nodes()** возвращает наборы строк и пустой набор строк для входного значения NULL.  
  
## <a name="see-also"></a>См. также:  
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Создание экземпляров данных XML](../../relational-databases/xml/create-instances-of-xml-data.md)  
  
  

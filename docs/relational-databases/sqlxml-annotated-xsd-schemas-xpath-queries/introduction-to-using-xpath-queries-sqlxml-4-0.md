---
title: Общие сведения об использовании запросов XPath (SQLXML)
description: Изучите основные сведения об использовании запросов XPath в SQLXML 4,0.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], about XPath queries
- W3C XPath specification
- XPath queries [SQLXML], functionality
ms.assetid: 01050a8e-0ccc-4a02-a4eb-b48be5c3f4f3
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2efd6f59cf0a853fbb357c930ba24d6e4889a2e2
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491834"
---
# <a name="introduction-to-using-xpath-queries-sqlxml-40"></a>Основные сведения об использовании запросов XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Запрос на языке XPath может быть указан как часть URL-адреса или внутри шаблона. Схема сопоставления определяет структуру этого результирующего фрагмента, а значения извлекаются из базы данных. Этот процесс имеет сходные концепции с созданием представлений при помощи инструкции CREATE VIEW и написания SQL-запросов к ним.  
  
> [!NOTE]  
>  Чтобы получить представление о запросах XPath в SQLXML 4.0, необходим опыт работы с XML-представлениями и другими связанными основными понятиями — шаблонами и схемами сопоставления. Дополнительные сведения см. в статьях [Введение в схемы XSD с Заметками &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)и стандарт XPath, определенный консорциум W3C (W3C).  
  
 XML-документ состоит из узлов, таких как узел элемента, узел атрибута, текстовый узел и т. д. Например, рассмотрим следующий XML-документ:  
  
```  
<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was  
          very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>  
```  
  
 В этом документе **\<Customer>** является узлом элемента, **CID** является узлом атрибута, а **"важно"** является текстовым узлом.  
  
 XPath — это язык навигации графа, используемый для выбора набора узлов из XML-документа. Каждый оператор XPath выбирает набор узлов на основе набора узлов, выбранных предыдущим оператором XPath. Например, при наличии набора **\<Customer>** узлов XPath может выбрать все **\<Order>** узлы со значением атрибута **Date** , равным **"7/14/1999"**. Результирующий набор узлов содержит все заказы с датой заказа 7/14/1999.  
  
 Язык XPath определен консорциумом W3C (World Wide Web Consortium) как стандартный язык навигации. SQLXML 4,0 реализует подмножество спецификации W3C XPath, расположенное по адресу http://www.w3.org/TR/1999/PR-xpath-19991008.html .  
  
 Далее приведены ключевые отличия реализации XPath консорциума W3C и реализации SQLXML 4.0.  
  
-   **Корневые запросы**  
  
     SQLXML 4.0 не поддерживает корневой запрос (/). Каждый запрос XPath должен начинаться на верхнем уровне **\<ElementType>** схемы.  
  
-   **Сообщения об ошибках**  
  
     Спецификация XPath консорциума W3C не определяет условия ошибки. Запросы XPath, которые не смогли выбрать какой-либо узел, возвращают пустой набор узлов. В SQLXML 4.0 запрос может вернуть несколько типов ошибок.  
  
-   **Порядок документа**  
  
     В SQLXML 4.0 порядок документа не всегда определен. Следовательно, числовые предикаты и оси, использующие порядок документов (например, **следующие**), не реализуются.  
  
     Отсутствие порядка документа также означает, что строковое значение узла может быть вычислено, только когда этот узел соответствует одному столбцу в одной строке. Элемент с дочерними элементами, или узел IDREFS, или узел NMTOKENS не могут быть преобразованы в строку.  
  
    > [!NOTE]  
    >  В некоторых случаях заметка " **ключ-поле** " или ключи из Аннотации " **связь** " может привести к детерминированному порядку документа. Однако это не является основным применением этих заметок для получения дополнительных сведений. см [. раздел Определение ключевых столбцов с помощью SQL: key-fields &#40;sqlxml 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md) и [Указание связей с помощью sql: relationship &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md).  
  
-   **Типы данных**  
  
     SQLXML 4,0 имеет ограничения на реализацию **строк** XPath, **чисел** и **логических** типов данных. Дополнительные сведения см. в разделе [типы данных XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/xpath-data-types-sqlxml-4-0.md).  
  
-   **Запросы перекрестного произведения**  
  
     SQLXML 4.0 не поддерживает запросы перекрестного произведения XPath, такие как `Customers[Order/@OrderDate=Order/@ShipDate]`. Этот запрос выбирает все наборы узлов Customer, состоящие из узлов Order, для которых OrderDate совпадает с ShipDate любого узла Order.  
  
     Однако SQLXML 4.0 не поддерживает такие запросы, как `Customer[Order[@OrderDate=@ShippedDate]]`, которые выбирают наборы узлов Customer, состоящие из узлов Order, для которых OrderDate совпадает с ShipDate.  
  
-   **Обработка ошибок и безопасность**  
  
     В зависимости от используемых схемы и выражения запроса XPath ошибки [!INCLUDE[tsql](../../includes/tsql-md.md)] могут быть представлены пользователю при определенных условиях.  
  
 Таблицы следующего раздела содержат сведения о различиях реализации запросов XPath в SQLXML 4.0 от спецификации консорциума W3C.  
  
## <a name="supported-functionality"></a>Поддерживаемые функции  
 В следующей таблице приведены возможности языка XPath, реализованные в SQLXML 4.0.  
  
|Функция|Item|Ссылка на образцы запросов|  
|-------------|----------|----------------------------|  
|Оси|**атрибуты**, **дочерние**, **родительские** и **собственные** оси|[Указание осей в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-axes-in-xpath-queries-sqlxml-4-0.md)|  
|Предикаты с логическими значениями, включая последовательные и вложенные предикаты.||[Указание арифметических операторов в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-arithmetic-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Все реляционные операторы|=,! =, <, \<=, > , >=|[Указание реляционных операторов в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-relational-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Арифметические операторы|+, -, *, div|[Указание арифметических операторов в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-arithmetic-operators-in-xpath-queries-sqlxml-4-0.md)|  
|Явные функции преобразования|**Number ()**, **String ()**, **Boolean ()**|[Указание явных функций преобразования в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-explicit-conversion-functions-in-xpath-queries-sqlxml-4-0.md)|  
|Логические операторы|AND, OR|[Указание логических операторов в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-boolean-operators-in-xpath-queries-sqlxml-4-0.md)|  
|логические функции|**true ()**, **false ()**, **NOT ()**|[Указание логических функций в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-boolean-functions-in-xpath-queries-sqlxml-4-0.md)|  
|переменные XPath||[Указание переменных XPath в запросах XPath &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/specifying-xpath-variables-in-xpath-queries-sqlxml-4-0.md)|  
  
## <a name="unsupported-functionality"></a>Неподдерживаемые функциональные возможности  
 В следующей таблице приведены функции языка XPath, не реализованные в SQLXML 4.0.  
  
|Функция|Item|  
|-------------|----------|  
|Оси|**предка**, **предка или-Self**, **потомка**, **потомка или самого себя (//)**, **следующего**, следующего **элемента**, **пространство имен**, **предшествующее**, **предшествующее одноуровневый**|  
|Предикаты с числовыми значениями||  
|Арифметические операторы|mod|  
|Функции узлов|**предка**, **предка или-Self**, **потомка**, **потомка или самого себя (//)**, **следующего**, следующего **элемента**, **пространство имен**, **предшествующее**, **предшествующее одноуровневый**|  
|Строковые функции|**String ()**, **concat ()**, **начало-с ()**, **Contains ()**, **substring-Before ()**, substring **-After ()**, substring **()**, **string-length ()**, **нормализация ()**, **сдвиг ()**|  
|логические функции|**lang ()**|  
|Числовые функции|**Sum ()**, **этаж ()**, **ceiling ()**, **Round ()**|  
|Оператор Union|&#124;|  
  
 При указании запросов XPath в шаблоне обратите внимание на следующее поведение.  
  
-   XPath может содержать такие символы, как < или &, которые имеют особые значения в XML (а шаблон — это XML-документ). Эти символы необходимо отформатировать с помощью & кодировки XML или указать XPath в URL-адресе.  
  
## <a name="see-also"></a>См. также  
 [Использование запросов XPath в SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md)  
  
  

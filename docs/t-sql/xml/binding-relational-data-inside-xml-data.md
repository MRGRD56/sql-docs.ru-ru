---
description: Привязка реляционных данных внутри XML-данных
title: Привязка реляционных данных внутри XML-данных
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- relational data binding [SQL Server]
- XML [SQL Server], binding relational data
- xml data type [SQL Server], relational data binding
- binding relational data [XML in SQL Server]
- variables [XML in SQL Server], relational data binding
- columns [XML in SQL Server], relational data binding
ms.assetid: 03d013a9-b53f-46c3-9628-da77f099c74a
author: rothja
ms.author: jroth
ms.openlocfilehash: df2fc4aab0d9e82db0288478595392d4c3ce880d
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490381"
---
# <a name="binding-relational-data-inside-xml-data"></a>Привязка реляционных данных внутри XML-данных
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Можно указать [методы типа данных xml](../../t-sql/xml/xml-data-type-methods.md) для переменной или столбца типа данных **xml**. Например, [метод query() (тип данных xml)](../../t-sql/xml/query-method-xml-data-type.md) выполняет заданный запрос XQuery к экземпляру XML. При построении XML таким способом может понадобиться ввести значение их столбца с типом данных, отличным от XML или переменной Transact-SQL. Данный процесс называется привязкой реляционных данных внутри XML.  
  
 Для привязки реляционных данных с типом, отличным от XML, внутри XML компонент SQL Server Database Engine обладает следующими псевдофункциями:  
  
-   [Функция sql:column() (XQuery)](../../xquery/xquery-extension-functions-sql-column.md). Позволяет использовать значения реляционного столбца в выражении XQuery или XML DML.  
  
-   [Функция sql:variable() (XQuery)](../../xquery/xquery-extension-functions-sql-variable.md). Позволяет использовать значения переменной SQL в выражении XQuery или XML DML.  
  
 Эти функции можно в любое время использовать совместно с методами, предназначенными для типа данных **xml**, для получения реляционного значения внутри XML.  
  
 Нельзя использовать эти функции для создания ссылок на данные в столбцах или переменных, относящихся к типам **xml**, определяемым пользователем типам данных CLR, а также к типам данных datetime, smalldatetime, **text**, **ntext**, **sql_variant** и **image**.  
  
 Однако данная привязка доступна только для чтения, то есть нельзя записывать данные в столбцы, использующие эти функции. Например, запись sql:переменная("\@x")="*некоторое выражение*" недопустима.  
  
## <a name="example-cross-domain-query-using-sqlvariable"></a>Пример запрос между доменами с использованием функции sql:variable()  
 В этом примере показано, как **sql:variable()** позволяет приложению подготовить параметризованный запрос. ISBN-номер передается с помощью переменной SQL @isbn. Заменив константу функцией **sql:variable()** , можно использовать этот запрос для поиска любого ISBN-номера, а не только 0-7356-1588-2.  
  
```sql
DECLARE @isbn VARCHAR(20)  
SET     @isbn = '0-7356-1588-2'  
SELECT  xCol  
FROM    T  
WHERE   xCol.exist ('/book/@ISBN[. = sql:variable("@isbn")]') = 1  
```  
  
 Функцию **sql:column()** также можно применять описанным образом, что позволяет получить дополнительные преимущества. Для повышения эффективности могут использоваться индексы столбца — это решает оптимизатор запросов на основе траты ресурсов. В вычисляемом столбце можно хранить свойство, для которого было выполнено продвижение.  
  
## <a name="see-also"></a>См. также:  
 [Методы для типа данных XML](../../t-sql/xml/xml-data-type-methods.md)  
  
  

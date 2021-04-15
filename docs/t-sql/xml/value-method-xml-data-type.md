---
description: Метод value() (тип данных xml)
title: Метод value() (тип данных xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- value method
- value() method
ms.assetid: 298a7361-dc9a-4902-9b1e-49a093cd831d
author: rothja
ms.author: jroth
ms.openlocfilehash: 40f5d022ed662c7cbddbd70bee57619605ef1b49
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491360"
---
# <a name="value-method-xml-data-type"></a>Метод value() (тип данных xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Выполняет запрос XQuery к структуре XML и возвращает значение типа SQL. Данный метод возвращает скалярное значение.  
  
 Обычно этот метод применяется для извлечения значения из экземпляра XML, который хранится в столбце, параметре или переменной типа **xml**. При этом его можно использовать в запросах SELECT для объединения или сравнения данных XML с данными в столбцах других типов.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
value (XQuery, SQLType)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *XQuery*  
 Выражение *XQuery* — строковый литерал, извлекающий данные из экземпляра XML. Выражение XQuery должно возвращать не более одного значения. Иначе возвращается ошибка.  
  
 *SQLType*  
 Предпочтительный тип SQL — строковый литерал, который необходимо вернуть. Тип возвращаемого этим методом значения соответствует значению параметра *SQLType*. *SQLType* не может быть типом данных **xml**, определяемым пользователем типом CLR, типом данных **image**, **text**, **ntext** или **sql_variant**. Значением *SQLType* может быть SQL и определяемый пользователем тип данных.  
  
 Метод **value()** неявно использует оператор CONVERT [!INCLUDE[tsql](../../includes/tsql-md.md)] и пытается преобразовать результат выражения XQuery, сериализованное строковое представление, из типа данных XSD в соответствующий тип SQL, указанный преобразованием языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Дополнительные сведения о правилах приведения типов для оператора CONVERT см. в разделе [CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
> [!NOTE]  
>  Для увеличения производительности операции сравнения с реляционным значением вместо метода **value()** в предикате используйте метод **exist()** совместно с **sql:column()** . Это показано в следующем примере Г.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-the-value-method-against-an-xml-type-variable"></a>A. Использование метода value() над переменной типа xml  
 В следующем примере экземпляр XML хранится в переменной типа `xml`. Метод `value()` извлекает из переменной XML значение атрибута `ProductID`. Затем полученное значение присваивается переменной типа `int`.  
  
```sql
DECLARE @myDoc XML  
DECLARE @ProdID INT  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
  
SET @ProdID =  @myDoc.value('(/Root/ProductDescription/@ProductID)[1]', 'int' )  
SELECT @ProdID  
```  
  
 В результате возвращается значение 1.  
  
 Хотя экземпляр XML содержит только один атрибут `ProductID`, правила статической типизации требуют явно указать, что выражение пути возвращает единственное значение. Поэтому в конце выражения пути указан дополнительный знак `[1]`. Дополнительные сведения о статической типизации см. в разделе [XQuery и статическая типизация](../../xquery/xquery-and-static-typing.md).  
  
### <a name="b-using-the-value-method-to-retrieve-a-value-from-an-xml-type-column"></a>Б. Использование метода value() для извлечения значения из столбца типа xml  
 В следующем примере выполняется запрос к столбцу типа **xml** (`CatalogDescription`) в базе данных `AdventureWorks`. Запрос извлекает значения атрибутов `ProductModelID` из каждого экземпляра XML, который хранится в столбце.  
  
```sql
SELECT CatalogDescription.value('             
    declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";             
       (/PD:ProductDescription/@ProductModelID)[1]', 'int') AS Result             
FROM Production.ProductModel             
WHERE CatalogDescription IS NOT NULL             
ORDER BY Result DESC             
```  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
-   Для определения префикса пространства имен используется ключевое слово `namespace`.  
  
-   Требования к статической типизации в выражении пути обязывают в методе `[1]` добавлять `value()` в конце выражения пути, чтобы явно указать, что оно возвращает единственное значение.  
  
 Частичный результат:  
  
```  
-----------  
35           
34           
...  
```  
  
### <a name="c-using-the-value-and-exist-methods-to-retrieve-values-from-an-xml-type-column"></a>В. Использование методов value() и exist() для извлечения значений из столбца типа xml  
 В следующем примере демонстрируется совместное использование методов `value()` и [exist()](../../t-sql/xml/exist-method-xml-data-type.md), возвращающих тип данных **xml**. Метод `value()` извлекает значения атрибутов `ProductModelID` из экземпляров XML. Метод `exist()` в предложении `WHERE` фильтрует строки, извлеченные из таблицы.  
  
 Запрос извлекает идентификаторы моделей продуктов из экземпляров XML, которые среди прочих данных содержат сведения о гарантии (элемент <`Warranty`>). Условие в предложении `WHERE` приводит к тому, что метод `exist()` извлекает только те строки, которые соответствуют условию.  
  
```sql
SELECT CatalogDescription.value('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
           (/PD:ProductDescription/@ProductModelID)[1] ', 'int') AS Result  
FROM  Production.ProductModel  
WHERE CatalogDescription.exist('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
-   Столбец `CatalogDescription` является типизированным XML-столбцом. Это означает, что с ним связан набор схем. В [XQuery Prolog](../../xquery/modules-and-prologs-xquery-prolog.md) объявлено пространство имен, определяющее префикс, который затем используется в теле запроса.  
  
-   Если метод `exist()` возвращает значение `1` (True), это значит, что экземпляр XML содержит дочерний элемент <`Warranty`>.  
  
-   Метод `value()` в предложении `SELECT` затем извлекает значения атрибутов `ProductModelID` в виде целых чисел.  
  
 Частичный результат:  
  
```  
Result       
-----------  
19           
23           
...  
```  
  
### <a name="d-using-the-exist-method-instead-of-the-value-method"></a>Г. Использование метода exist() вместо метода value()  
 Для увеличения производительности операции сравнения с реляционным значением вместо метода `value()` в предикате используйте метод `exist()` совместно с `sql:column()`. Пример:  
  
```sql
CREATE TABLE T (c1 INT, c2 VARCHAR(10), c3 XML)  
GO  
  
SELECT c1, c2, c3   
FROM T  
WHERE c3.value( '/root[1]/@a', 'integer') = c1  
GO  
```  
  
 Запрос будет выглядеть следующим образом:  
  
```sql
SELECT c1, c2, c3   
FROM T  
WHERE c3.exist( '/root[@a=sql:column("c1")]') = 1  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Добавление пространств имен в запросы с WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [методов типа данных xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Язык модификации XML-данных (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  

---
title: Продвижение часто используемых XML-значений с помощью вычисляемых столбцов | Документация Майкрософт
description: Узнайте, как продвинуть часто используемые значения XML путем создания вычисляемых столбцов для более эффективного выполнения запросов.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- promoting properties [XML in SQL Server]
- property promotion [XML in SQL Server]
ms.assetid: f5111896-c2fd-4209-b500-f2baa45489ad
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 15bbb0bc454c8069bdb77750b45de6db2260b280
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487799"
---
# <a name="promote-frequently-used-xml-values-with-computed-columns"></a>Продвижение часто используемых XML-значений с помощью вычисляемых столбцов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Если часто запрашивается небольшое количество значений элементов и атрибутов, можно выполнить их продвижение до реляционных столбцов. Это полезно, если запрос выполняется для небольшой части XML-данных, тогда как извлекается весь экземпляр XML. Создавать XML-индекс для XML-столбца не требуется. Вместо этого можно проиндексировать столбец, созданный на основе свойства. В запросах необходимо использовать этот столбец, потому что оптимизатор запросов не перенаправляет запросы XML-столбца столбцу, созданному на основе свойства.  
  
 Столбец, созданный на основе свойства, может быть вычисляемым столбцом в той же таблице или отдельным пользовательским столбцом таблицы. Если для каждого экземпляра XML выполняется продвижение одинарных значений, этого достаточно. Однако в случае многозначных свойств необходимо создать для свойства отдельную таблицу. Это описывается в следующем разделе.  
  
## <a name="computed-column-based-on-the-xml-data-type"></a>Вычисляемый столбец, основанный на типе данных xml  
 Вычисляемый столбец можно создать с помощью пользовательской функции, вызывающей методы типа данных **xml** . Вычисляемый столбец может иметь любой тип SQL, в том числе XML. Это продемонстрировано в следующем примере.  
  
### <a name="example-computed-column-based-on-the-xml-data-type-method"></a>Пример Вычисляемый столбец, основанный на методе типа данных xml  
 Создание пользовательской функции, возвращающей ISBN-номер книги:  
  
```  
CREATE FUNCTION udf_get_book_ISBN (@xData xml)  
RETURNS varchar(20)  
BEGIN  
   DECLARE @ISBN   varchar(20)  
   SELECT @ISBN = @xData.value('/book[1]/@ISBN', 'varchar(20)')  
   RETURN @ISBN   
END  
```  
  
 Добавление в таблицу вычисляемого столбца для хранения значений ISBN:  
  
```  
ALTER TABLE      T  
ADD   ISBN AS dbo.udf_get_book_ISBN(xCol)  
```  
  
 Теперь вычисляемый столбец можно проиндексировать обычным способом.  
  
### <a name="example-queries-on-a-computed-column-based-on-xml-data-type-methods"></a>Пример Запросы данных из вычисляемого столбца на основе методов типа данных xml  
 Чтобы получить элемент <`book`> с ISBN-номером 0-7356-1588-2, можно выполнить следующее:  
  
```  
SELECT xCol  
FROM   T  
WHERE  xCol.exist('/book/@ISBN[. = "0-7356-1588-2"]') = 1  
```  
  
 Запрос данных из XML-столбца можно переписать так, чтобы в нем использовался вычисляемый столбец:  
  
```  
SELECT xCol  
FROM   T  
WHERE  ISBN = '0-7356-1588-2'  
```  
  
 Можно создать пользовательскую функцию, возвращающую тип данных **xml** , и вычисляемый столбец, основанный на пользовательской функции. Однако создать XML-индекс для вычисляемого XML-столбца нельзя.  
  
## <a name="creating-property-tables"></a>Создание таблиц свойств  
 Иногда целесообразно выполнить продвижение некоторых многозначных свойств XML-данных до одной или нескольких таблиц, создать индексы для этих таблиц и перенаправить запросы к этим таблицам. Типичная ситуация, когда это полезно, имеет место тогда, когда в большинстве запросов обращение происходит к небольшому числу свойств. Можно сделать следующее.  
  
-   Создать одну или несколько таблиц для хранения многозначных свойств. Иногда удобно хранить одно свойство в каждой таблице и продублировать первичный ключ базовой таблицы в таблицах свойств для их соединения с базовой таблицей.  
  
-   Если требуется сохранить относительный порядок свойств, для этого нужно создать отдельный столбец.  
  
-   Создать триггеры для XML-столбца с целью обслуживания таблиц свойств. В триггерах сделайте что-либо одно из следующего перечисленного ниже.  
  
    -   Используйте методы типа данных **xml** , такие как **nodes()** и **value()** , для вставки строк в таблицы свойств и их удаления.  
  
    -   Создайте в среде CLR потоковые возвращающие табличное значение функции для вставки строк в таблицы свойств и их удаления.  
  
    -   Напишите запросы, осуществляющие основанный на SQL доступ к таблицам свойств и основанный на XML доступ к XML-столбцу базовой таблицы, соединяя таблицы с использованием их первичного ключа.  
  
### <a name="example-create-a-property-table"></a>Пример Создание таблицы свойств  
 Предположим, что требуется выполнить продвижение свойства, представляющего имена авторов. У книги может быть несколько авторов, поэтому данное свойство является многозначным. Каждое имя хранится в отдельной строке таблицы свойств. Первичный ключ базовой таблицы дублируется в таблице свойств ради обратного соединения таблиц.  
  
```  
create table tblPropAuthor (propPK int, propAuthor varchar(max))  
```  
  
### <a name="example-create-a-user-defined-function-to-generate-a-rowset-from-an-xml-instance"></a>Пример Создание пользовательской функции для создания набора строк на основе экземпляра XML  
 Следующая возвращающая табличное значение функция, udf_XML2Table, принимает значение первичного ключа и экземпляр XML. Она извлекает имена всех авторов из элемента <`book`> и возвращает набор строк, состоящий из пар (первичный ключ / имя).  
  
```  
create function udf_XML2Table (@pk int, @xCol xml)  
returns @ret_Table table (propPK int, propAuthor varchar(max))  
with schemabinding  
as  
begin  
      insert into @ret_Table   
      select @pk, nref.value('.', 'varchar(max)')  
      from   @xCol.nodes('/book/author/first-name') R(nref)  
      return  
end  
```  
  
### <a name="example-create-triggers-to-populate-a-property-table"></a>Пример Создание триггеров для заполнения таблицы свойств  
 Триггер вставки вставляет строки в таблицу свойств:  
  
```  
create trigger trg_docs_INS on T for insert  
as  
      declare @wantedXML xml  
      declare @FK int  
      select @wantedXML = xCol from inserted  
      select @FK = PK from inserted  
  
   insert into tblPropAuthor  
   select * from dbo.udf_XML2Table(@FK, @wantedXML)  
```  
  
 Триггер удаления удаляет строки из таблицы свойств на основе значения первичного ключа:  
  
```  
create trigger trg_docs_DEL on T for delete  
as  
   declare @FK int  
   select @FK = PK from deleted  
   delete tblPropAuthor where propPK = @FK  
```  
  
 Триггер обновления удаляет строки из таблицы свойств в соответствии с обновленным экземпляром XML и вставляет в таблицу свойств новые строки:  
  
```  
create trigger trg_docs_UPD  
on T  
for update  
as  
if update(xCol) or update(pk)  
begin  
      declare @FK int  
      declare @wantedXML xml  
      select @FK = PK from deleted  
      delete tblPropAuthor where propPK = @FK  
  
   select @wantedXML = xCol from inserted  
   select @FK = pk from inserted  
  
   insert into tblPropAuthor   
      select * from dbo.udf_XML2Table(@FK, @wantedXML)  
end  
```  
  
### <a name="example-find-xml-instances-whose-authors-have-the-same-first-name"></a>Пример Поиск экземпляров XML, включающих авторов с именем "David"  
 Можно составить такой запрос для XML-столбца или найти в таблице свойств записи с именами David и выполнить обратное соединение с базовой таблицей для возврата экземпляра XML. Пример:  
  
```  
SELECT xCol   
FROM     T JOIN tblPropAuthor ON T.pk = tblPropAuthor.propPK  
WHERE    tblPropAuthor.propAuthor = 'David'  
```  
  
### <a name="example-solution-using-the-clr-streaming-table-valued-function"></a>Пример Решение, основанное на использовании потоковой функции CLR с табличным значением  
 Данное решение состоит из следующих шагов:  
  
1.  определение CLR-класса SqlReaderBase, реализующего интерфейс ISqlReader и формирующего потоковый возвращающий табличное значение выход путем применения выражения пути к экземпляру XML;  
  
2.  создание сборки и пользовательской функции языка Transact-SQL, запускающей класс CLR;  
  
3.  определение с помощью пользовательской функции триггеров вставки, обновления и удаления для обслуживания таблиц свойств.  

 Чтобы реализовать это решение, создайте сначала потоковую функцию CLR. Тип данных **xml** представляется в ADO.NET как управляемый класс SqlXml с поддержкой метода **CreateReader()** , возвращающего объект класса XmlReader.  
  
> [!NOTE]  
>  В примере данного раздела используются классы XPathDocument и XPathNavigator, которые вынуждают программиста загрузить в память все XML-документы. Используя подобный код в своих приложениях для обработки нескольких крупных XML-документов, знайте, что он плохо масштабируется. Старайтесь свести к минимуму число операций выделения памяти и используйте во всех возможных случаях потоковые интерфейсы. Дополнительные сведения о производительности см. в разделе [Архитектура интеграции со средой CLR](../clr-integration/clr-integration-architecture-clr-hosted-environment.md).  
  
```  
public class c_streaming_xml_tvf {  
   public static ISqlReader streaming_xml_tvf   
(SqlXml xmlDoc, string pathExpression) {  
      return (new TestSqlReaderBase (xmlDoc, pathExpression));  
   }  
}  
  
// Class that implements ISqlReader  
public class TestSqlReaderBase : ISqlReader {  
XPathNodeIterator m_iterator;           
   public SqlChars FirstName;  
// Metadata for current resultset  
private SqlMetaData[] m_rgSqlMetaData;        
  
   public TestSqlReaderBase (SqlXml xmlDoc, string pathExpression) {     
      // Variables for XPath navigation  
      XPathDocument xDoc;  
      XPathNavigator xNav;  
      XPathExpression xPath;  
  
      // Set sql metadata  
      m_rgSqlMetaData = new SqlMetaData[1];  
      m_rgSqlMetaData[0] = new SqlMetaData ("FirstName",    
SqlDbType.NVarChar,50);     
  
      //Set up the Navigator  
      if (!xmlDoc.IsNull)  
          xDoc = new XPathDocument (xmlDoc.CreateReader());  
      else  
          xDoc = new XPathDocument ();  
      xNav = xDoc.CreateNavigator();  
      xPath = xNav.Compile (pathExpression);  
      m_iterator = xNav.Select(xPath);  
   }  
   public bool Read() {  
      bool moreRows = true;  
      if (moreRows = m_iterator.MoveNext())  
         FirstName = new SqlChars (m_iterator.Current.Value);  
      return moreRows;  
   }  
}  
```  
  
 Затем создайте сборку и пользовательскую функцию [!INCLUDE[tsql](../../includes/tsql-md.md)] с именем SQL_streaming_xml_tvf (не показана), соответствующую функции CLR streaming_xml_tvf. Пользовательская функция применяется для определения возвращающей табличное значение функции CLR_udf_XML2Table с целью создания набора строк:  
  
```  
create function CLR_udf_XML2Table (@pk int, @xCol xml)  
returns @ret_Table table (FK int, FirstName varchar(max))  
with schemabinding  
as  
begin  
      insert into @ret_Table   
   select @pk, FirstName   
   FROM   SQL_streaming_xml_tvf (@xCol, '/book/author/first-name')  
      return  
end  
```  
  
 Наконец, определите триггеры так, как показано в разделе «Создание триггеров для заполнения таблицы свойств», заменив при этом функцию udf_XML2Table функцией CLR_udf_XML2Table. Триггер вставки показан в следующем примере:  
  
```  
create trigger CLR_trg_docs_INS on T for insert  
as  
   declare @wantedXML xml  
   declare @FK int  
   select @wantedXML = xCol from inserted  
   select @FK = PK from inserted  
  
   insert into tblPropAuthor  
      select *  
   from    dbo.CLR_udf_XML2Table(@FK, @wantedXML)  
```  
  
 Триггер удаления идентичен версии, не использующей среду CLR. Однако триггер обновления просто заменяет функцию udf_XML2Table() функцией CLR_udf_XML2Table().  
  
## <a name="see-also"></a>См. также:  
 [Использование XML в вычисляемых столбцах](../../relational-databases/xml/use-xml-in-computed-columns.md)  
  

---
title: Создание XML-индексов | Документация Майкрософт
description: Сведения о создании первичных и вторичных XML-индексов в SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- indexes [XML in SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: 6ecac598-355d-4408-baf7-1b2e8d4cf7c1
author: rothja
ms.author: jroth
ms.openlocfilehash: 7ef33369b40d0e1530240dfade0c9f3aa0b63ed7
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490950"
---
# <a name="create-xml-indexes"></a>Создание XML-индексов
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В данном разделе описано создание первичных и вторичных XML-индексов.  
  
## <a name="creating-a-primary-xml-index"></a>Создание первичного XML-индекса  
 Для создания первичного XML-индекса используется DDL-инструкция [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)]. Для XML-индексов поддерживаются не все параметры, доступные для обычных индексов.  
  
 При создании XML-индекса следует учитывать следующее:  
  
-   Для создания первичного XML-индекса таблица, содержащая индексируемый XML-столбец и называемая базовой таблицей, должна иметь кластеризованный индекс первичного ключа. Это гарантирует, что в случае секционирования базовой таблицы первичный XML-индекс может быть секционирован при использовании той же схемы и той же функции секционирования.  
  
-   Если XML-индекс уже существует, кластеризованный первичный ключ таблицы не может быть изменен. Перед изменением первичного ключа необходимо удалить все XML-индексы, созданные для таблицы.  
  
-   Первичный XML-индекс можно создать только для столбца типа данных **xml** . Никакой другой тип индекса, в котором столбец типа данных XML является ключевым, создать нельзя. Однако столбец типа **xml** может быть включен в обычный (не XML) индекс. Каждый столбец типа **xml** в таблице может иметь собственный первичный XML-индекс. Однако для столбца типа **xml** допустим только один первичный XML-индекс.  
  
-   XML-индексы существуют в том же пространстве имен, что и обычные индексы. Поэтому для таблицы не могут быть определены обычный и XML-индекс с одинаковыми именами.  
  
-   Параметры IGNORE_DUP_KEY и ONLINE для XML-индексов всегда должны устанавливаться в OFF. Можно указывать эти параметры со значением OFF.  
  
-   Сведения о файловых группах и секционировании пользовательской таблицы применимы к XML-индексам. Однако пользователи не могут задавать их для XML-индекса отдельно.  
  
-   Параметр DROP_EXISTING может использоваться для удаления и создания нового первичного XML-индекса или для удаления и создания нового вторичного XML-индекса. Однако нельзя удалить вторичный XML-индекс для создания первичного, и наоборот.  
  
-   На имена первичных XML-индексов накладываются те же ограничения, что и на имена представлений.  
  
 Нельзя создать XML-индекс для столбца типа **xml** в представлении для **табличной** переменной со столбцами типа **xml** , а также для переменных типа **xml** .  
  
-   Для изменения столбца типа **xml** с помощью параметра ALTER TABLE ALTER COLUMN с нетипизированного на типизированный XML (или наоборот) для этого столбца не должно быть определено никаких XML-индексов. Если такой индекс существует, он должен быть сначала удален.  
  
-   При создании XML-индекса параметр ARITHABORT должен быть установлен в значение ON. Для запроса, вставки, удаления или обновления значений в столбце XML методами типа данных XML этот параметр должен быть установлен для соединения. В противном случае методы типа данных XML будут завершаться ошибкой.  
  
    > [!NOTE]  
    >  Сведения об XML-индексе доступны через представления каталога, но процедура **sp_helpindex** для них не поддерживается. Ниже в этом подразделе содержатся примеры, которые демонстрируют, какие следует выполнить запросы к представлениям каталога, чтобы получить сведения об XML-индексе.  
  
 При создании или повторном создании первичного XML-индекса на столбце с типом данных XML, в котором содержатся схемы XML типов **xs:date** или **xs:dateTime** (либо любых подтипов данных типов) со значением года меньше 1, создание индекса завершается ошибкой в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] эти значения допускались, поэтому данная проблема может возникнуть при создании индексов в базе данных, сформированной в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Дополнительные сведения см. в статье [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
### <a name="example-creating-a-primary-xml-index"></a>Пример Создание первичного XML-индекса  
 В большинстве наших примеров используется таблица T (pk INT PRIMARY KEY, xCol XML) с нетипизированным XML-столбцом. Эти примеры можно легко расширить на типизированный XML. Ради простоты запросы XML-данных описываются следующим образом:  
  
```  
<book genre="security" publicationdate="2002" ISBN="0-7356-1588-2">  
   <title>Writing Secure Code</title>  
   <author>  
      <first-name>Michael</first-name>  
      <last-name>Howard</last-name>  
   </author>  
   <author>  
      <first-name>David</first-name>  
      <last-name>LeBlanc</last-name>  
   </author>  
   <price>39.99</price>  
</book>  
```  
  
 Следующая инструкция создает для XML-столбца xCol таблицы T XML-индекс с именем idx_xCol.  
  
```  
CREATE PRIMARY XML INDEX idx_xCol on T (xCol)  
```  
  
## <a name="creating-a-secondary-xml-index"></a>Создание вторичного XML-индекса  
 DDL-инструкция [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] позволяет создавать вторичные XML-индексы и указывать их тип.  
  
 При создании вторичных XML-индексов следует учитывать следующее.  
  
-   Для вторичных XML-индексов допустимы все параметры, применимые к некластеризованным индексам, за исключением параметров IGNORE_DUP_KEY и ONLINE. Для вторичных XML-индексов эти два параметра должны всегда устанавливаться в OFF.  
  
-   Вторичные индексы секционируются так же, как и первичный XML-индекс.  
  
-   Параметр DROP_EXISTING позволяет удалить вторичный индекс для пользовательской таблицы и создать другой вторичный индекс для той же таблицы.  
  
 Для получения сведений об XML-индексе можно выполнить запрос к представлению каталога **sys.xml_indexes** . Обратите внимание, что столбец **secondary_type_desc** в представлении каталога **sys.xml_indexes** указывает тип вторичного индекса:  
  
```  
SELECT  *   
FROM    sys.xml_indexes;  
```  
  
 Столбец **secondary_type_desc** может возвращать значения NULL, PATH, VALUE или PROPERTY. Для первичного XML-индекса всегда возвращается значение NULL.  
  
### <a name="example-creating-secondary-xml-indexes"></a>Пример создание вторичных XML-индексов  
 В следующем примере иллюстрируется создание вторичных XML-индексов. Здесь также выводятся сведения о созданных XML-индексах.  
  
```  
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML);  
GO  
-- Create primary index.  
CREATE PRIMARY XML INDEX PIdx_T_XmlCol   
ON T(XmlCol);  
GO  
-- Create secondary indexes (PATH, VALUE, PROPERTY).  
CREATE XML INDEX PIdx_T_XmlCol_PATH ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR PATH;  
GO  
CREATE XML INDEX PIdx_T_XmlCol_VALUE ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR VALUE;  
GO  
CREATE XML INDEX PIdx_T_XmlCol_PROPERTY ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR PROPERTY;  
GO  
```  
  
 Для получения сведений об XML-индексах можно выполнить запрос к представлению каталога `sys.xml_indexes` . Столбец `secondary_type_desc` содержит тип вторичного индекса.  
  
```  
SELECT  *   
FROM    sys.xml_indexes;  
```  
  
 Для получения сведений об индексе можно также выполнить запрос к представлению каталога:  
  
```  
SELECT *  
FROM sys.xml_indexes  
WHERE object_id = object_id('T');  
```  
  
 Можно добавить какие-либо данные, а затем просмотреть сведения об XML-индексе.  
  
```  
INSERT INTO T VALUES (1,  
'<doc id="123">  
<sections>  
<section num="2">  
<heading>Background</heading>  
</section>  
<section num="3">  
<heading>Sort</heading>  
</section>  
<section num="4">  
<heading>Search</heading>  
</section>  
</sections>  
</doc>');  
GO  
-- Check XML index information.  
SELECT *  
FROM   sys.dm_db_index_physical_stats (db_id(), object_id('T'), NULL, NULL, 'DETAILED');  
GO  
-- Space usage of primary XML index  
DECLARE @index_id int;  
SELECT  @index_id = i.index_id  
FROM    sys.xml_indexes i   
WHERE   i.name = 'PIdx_T_XmlCol' and object_name(i.object_id) = 'T';  
  
SELECT *  
FROM sys.dm_db_index_physical_stats (db_id(), object_id('T') , @index_id, DEFAULT, 'DETAILED');  
go  
--- Space usage of secondary XML index (for example PATH secondary index)  PIdx_T_XmlCol_PATH  
DECLARE @index_id int;  
SELECT  @index_id = i.index_id   
FROM    sys.xml_indexes i   
WHERE  i.name = 'PIdx_T_XmlCol_PATH' and object_name(i.object_id) = 'T';  
  
SELECT *  
FROM sys.dm_db_index_physical_stats (db_id(), object_id('T') , @index_id, DEFAULT, 'DETAILED');  
go  
  
-- Space usage of all secondary XML indexes for a particular table  
SELECT i.name, object_name(i.object_id), stats.*   
FROM   sys.dm_db_index_physical_stats (db_id(), object_id('T'), NULL, DEFAULT, 'DETAILED') stats  
JOIN sys.xml_indexes i ON (stats.object_id = i.object_id and stats.index_id = i.index_id)  
WHERE secondary_type is not null;  
-- Drop secondary indexes.  
DROP INDEX PIdx_T_XmlCol_PATH ON T;  
GO  
DROP INDEX PIdx_T_XmlCol_VALUE ON T;  
GO  
DROP INDEX PIdx_T_XmlCol_PROPERTY ON T;  
GO  
-- Drop primary index.  
DROP INDEX PIdx_T_XmlCol ON T;  
-- Drop table T.  
DROP TABLE T;  
Go  
```  
  
## <a name="see-also"></a>См. также:  
 [XML-индексы (SQL Server)](../../relational-databases/xml/xml-indexes-sql-server.md)   
 [Данные XML (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
  

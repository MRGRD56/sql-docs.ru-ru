---
title: Определение метасвойств в инструкции OPENXML | Документация Майкрософт
description: Сведения об указании атрибутов метасвойств в инструкции OPENXML для извлечения сведений из узлов XML.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- overflow in XML document [SQL Server]
- metaproperties [XML in SQL Server]
- unconsumed data
- extracting information of XML nodes [SQL Server]
- OPENXML statement, metaproperties
ms.assetid: 29bfd1c6-3f9a-43c4-924a-53d438e442f4
author: rothja
ms.author: jroth
ms.openlocfilehash: b2404d376c506c0d2168c0fd5e68b2cb575bd7c8
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487475"
---
# <a name="specify-metaproperties-in-openxml"></a>Определение метасвойств в инструкции OPENXML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Атрибутами метасвойств в документе XML называются атрибуты, описывающие свойства сущностей XML, например элементов, атрибутов и других узлов DOM. Физически эти атрибуты отсутствуют в тексте документа XML. Тем не менее инструкция OPENXML предоставляет эти метасвойства для всех сущностей XML. Эти метасвойства позволяют извлекать сведения, например данные о локальном положении и пространстве имен, об узлах XML. Эти сведения предоставляют более подробные данные, чем текстовое представление.  
  
 Метасвойства можно сопоставить со столбцами набора строк инструкции OPENXML при помощи параметра *ColPattern* . Столбцы будут содержать значения метасвойств, с которыми они связаны. Дополнительные сведения о синтаксисе инструкции OPENXML см. в разделе [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md).  
  
 Для доступа к атрибутам метасвойств предоставлено характерное для сервера SQL Server пространство имен. Это пространство имен, **urn:schemas-microsoft-com:xml-metaprop** , позволяет пользователям обращаться к атрибутам метасвойств. Инструкция OPENXML может возвращать результат запроса в виде таблицы, которая содержит столбец для каждого атрибута метасвойств, кроме метасвойства **xmltext** .  
  
 Некоторые атрибуты метасвойств используются в целях обработки. Например, атрибут метасвойства **xmltext** применяется для управления переполнением. Управление переполнением подразумевает обработку невостребованных, необработанных данных документа. Один столбец набора строк, сформированного инструкцией OPENXML, можно назначить столбцом переполнения. Для этого необходимо связать его с метасвойством **xmltext** при помощи параметра *ColPattern* . После этого в столбец будут копироваться данные переполнения. Параметр *flags* определяет, какие данные содержит столбец: все или только невостребованные.  
  
 В следующей таблице перечислены атрибуты метасвойств, которыми обладают все анализируемые элементы XML. К атрибутам метасвойств можно обращаться при помощи пространства имен **urn:schemas-microsoft-com:xml-metaprop**. Значение, заданное непосредственно в документе XML с помощью метасвойств, игнорируется.  
  
> [!NOTE]  
>  На метасвойства нельзя ссылаться при перемещении по XML с помощью XPath.  
  
|Атрибут метасвойства|Описание|  
|----------------------------|-----------------|  
|**\@mp:id**|Формируемый системой идентификатор узла DOM, действующий в пределах документа. Если документ не подлежит повторному синтаксическому анализу, этот идентификатор ссылается на тот же узел XML.<br /><br /> Идентификатор XML, равный **0** , обозначает корневой элемент. У такого узла идентификатор XML родительского элемента равен значению NULL.|  
|**\@mp:localname**|Локальная часть имени узла. Применяется вместе с префиксом и URI пространства имен для обозначения элементов и узлов атрибутов.|  
|**\@mp:namespaceuri**|URI пространства имен текущего элемента. Если значение атрибута равно NULL, пространство имен не существует.|  
|**\@mp:prefix**|Префикс пространства имен текущего элемента.<br /><br /> Если префикс отсутствует (равен значению NULL), а URI задан, значит, указанное пространство имен используется по умолчанию. Если URI не задан, пространство имен не присоединяется.|  
|**\@mp:prev**|Предыдущий элемент того же уровня. Благодаря этому можно получить сведения о порядке элементов документа.<br /><br /> Атрибут **\@mp:prev** содержит идентификатор XML предыдущего соседнего элемента с тем же родительским элементом. У первого элемента в списке соседних узлов атрибут **\@mp:prev** равен значению NULL.|  
|**\@mp:xmltext**|Применяется для обработки. Текстовая сериализация элемента, его атрибутов и подэлементов, которая используется при управлении переполнением в инструкции OPENXML.|  
  
 В следующей таблице перечислены дополнительные родительские свойства, которые позволяют получить сведения об иерархии.  
  
|Родительский атрибут метасвойства|Описание|  
|-----------------------------------|-----------------|  
|**\@mp:parentid**|Соответствует **../\@mp:id**|  
|**\@mp:parentlocalname**|Соответствует **../\@mp:localname**|  
|**\@mp:parentnamespacerui**|Соответствует **../\@mp:namespaceuri**|  
|**\@mp:parentprefix**|Соответствует **../\@mp:prefix**|  
  
## <a name="examples"></a>Примеры  
 Следующие примеры демонстрируют, как при помощи инструкции OPENXML создать различные представления наборов строк.  
  
### <a name="a-mapping-the-openxml-rowset-columns-to-the-metaproperties"></a>A. Сопоставление столбцов набора строк OPENXML с метасвойствами  
 В этом примере инструкция OPENXML применяется для создания представления набора строк учебного документа XML. В частности, пример демонстрирует, как различные атрибуты метасвойств можно связать со столбцами набора строк инструкции OPENXML при помощи параметра *ColPattern* .  
  
 Инструкция OPENXML иллюстрирует следующее:  
  
-   Столбец **id** привязан к атрибуту метасвойства **\@mp:id**, который указывает, что столбец содержит сформированный системой уникальный идентификатор XML элемента.  
  
-   Столбец **parent** привязан к атрибуту **\@mp:parentid**, который указывает, что столбец содержит идентификатор XML родительского элемента.  
  
-   Столбец **parentLocalName** привязан к атрибуту **\@mp:parentlocalname**, который указывает, что столбец содержит локальное имя родительского элемента.  
  
 Инструкция SELECT возвращает набор строк, предоставленных OPENXML:  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
-- Sample XML document  
SET @doc = N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/root/Customer/Order', 9)  
      WITH (id int '@mp:id',   
            oid char(5),   
            date datetime,   
            amount real,   
            parentIDNo int '@mp:parentid',   
            parentLocalName varchar(40) '@mp:parentlocalname')  
EXEC sp_xml_removedocument @idoc  
```  
  
 Результат:  
  
```  
id   oid         date                amount    parentIDNo  parentLocalName    
--- ------- ---------------------- ---------- ------------ ---------------  
6    O1    1996-01-20 00:00:00.000     3.5         2        Customer  
10   O2    1997-04-30 00:00:00.000     13.4        2        Customer  
19   O3    1999-07-14 00:00:00.000     100.0       15       Customer  
25   O4    1996-01-20 00:00:00.000     10000.0     15       Customer  
```  
  
### <a name="b-retrieving-the-whole-xml-document"></a>Б. Получение всего XML-документа  
 В этом примере инструкция OPENXML применяется для создания представления набора строк из одного столбца учебного XML-документа. Этот столбец ( **Col1**) связан с метасвойством **xmltext** и является столбцом переполнения. В результате столбец получает невостребованные данные. В нашем случае этими данными является весь документ.  
  
 Затем инструкция SELECT возвращает полный набор строк.  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
SET @doc = N'<?xml version="1.0"?>  
<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very   
             satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
     <MyTag>Testing to see if all the subelements are returned</MyTag>  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/')  
   WITH (Col1 ntext '@mp:xmltext')  
```  
  
 Чтобы получить весь документ без объявления XML, запрос можно указать следующим образом:  
  
```  
SELECT *  
FROM OPENXML (@idoc, '/root')  
   WITH (Col1 ntext '@mp:xmltext')  
EXEC sp_xml_removedocument @idoc  
```  
  
 Запрос возвращает корневой элемент, имеющий имя, и данные, которые он содержит.  
  
### <a name="c-specifying-the-xmltext-metaproperty-to-retrieve-the-unconsumed-data-in-a-column"></a>В. Определение метасвойства xmltext для получения невостребованных данных в столбце  
 В этом примере инструкция OPENXML применяется для создания представления набора строк учебного документа XML. Пример демонстрирует, как получить невостребованные данные XML, связав атрибут метасвойства **xmltext** со столбцом набора строк в OPENXML.  
  
 Столбец **comment** указан в качестве столбца переполнения путем привязки к метасвойству **\@mp:xmltext**. Для параметра *flags* установлено значение **9** (XML_ATTRIBUTE and XML_NOCOPY). Оно соответствует **атрибутивному** сопоставлению и указывает, что в столбец переполнения необходимо скопировать только невостребованные данные.  
  
 Затем инструкция SELECT возвращает набор строк, предоставленных OPENXML.  
  
 В этом примере метасвойство **\@mp:parentlocalname** задано для столбца **ParentLocalName** в наборе строк, созданном инструкцией OPENXML. В результате этот столбец содержит локальное имя родительского элемента.  
  
 Кроме него, набор строк содержит два столбца: **parent** и **comment**. Столбец **parent** привязан к атрибуту **\@mp:parentid**, который указывает, что столбец содержит идентификатор XML родительского элемента. Столбец comment указан в качестве столбца переполнения путем привязки к метасвойству **\@mp:xmltext**.  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
-- sample XML document  
SET @doc = N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>  
'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/root/Customer/Order', 9)  
      WITH (oid char(5),   
            date datetime,  
            comment ntext '@mp:xmltext')  
EXEC sp_xml_removedocument @idoc  
```  
  
 Результат. Поскольку столбцы идентификаторов объектов и столбцы даты уже востребованы, они отсутствуют в столбце переполнения.  
  
```  
oid   date                        comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
----- --------------------------- ----------------------------------------  
O1    1996-01-20 00:00:00.000     <Order amount="3.5"/>  
O2    1997-04-30 00:00:00.000     <Order amount="13.4">Customer was very   
                                   satisfied</Order>  
O3    1999-07-14 00:00:00.000     <Order amount="100" note="Wrap it blue   
                                   white red"><Urgency>   
                                   Important</Urgency></Order>  
O4    1996-01-20 00:00:00.000     <Order amount="10000"/>  
```  
  
## <a name="see-also"></a>См. также:  
 [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML (SQL Server)](../../relational-databases/xml/openxml-sql-server.md)  
  
  

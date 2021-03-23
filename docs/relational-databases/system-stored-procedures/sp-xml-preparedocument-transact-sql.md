---
description: sp_xml_preparedocument (Transact-SQL)
title: sp_xml_preparedocument (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xml_preparedocument_TSQL
- sp_xml_preparedocument
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xml_preparedocument
ms.assetid: 95f41cff-c52a-4182-8ac6-bf49369d214c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29377edf5fa911c47b4e3a34fa1be58046c3e915
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890786"
---
# <a name="sp_xml_preparedocument-transact-sql"></a>sp_xml_preparedocument (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Считывает входной XML-текст, проводит его синтаксический анализ при помощи средства синтаксического анализатора MSXML (Msxmlsql.dll) и выдает проанализированный документ, готовый к потреблению. Проанализированный документ является древовидным представлением различных узлов в XML-документе: элементов, атрибутов, текста, комментариев и т. д.  
  
 **sp_xml_preparedocument** возвращает маркер, который можно использовать для доступа к только что созданному внутреннему ПРЕДСТАВЛЕНИЮ XML-документа. Этот маркер действителен в течение сеанса или до тех пор, пока этот маркер не станет недействительным, выполнив **sp_xml_removedocument**.  
  
> [!NOTE]  
>  Проанализированный документ хранится во внутреннем кэше [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Средство синтаксического анализа MSXML может использовать одну восьмую общей памяти, доступной для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Чтобы избежать нехватки памяти, запустите **sp_xml_removedocument** , чтобы освободить память, как только документ больше не требуется. Во многих случаях метод nodes () может быть лучшим вариантом и помогает избежать чрезмерного использования памяти.
  
> [!NOTE]  
>  Для обеспечения обратной совместимости **sp_xml_preparedocument** СВОРАЧИВАЕТ символы CR (13)) и LF (char (10)) в атрибутах, даже если эти символы являются сущностями.  
  
> [!NOTE]  
>  Средство синтаксического анализа XML, вызываемое **sp_xml_preparedocument** , может анализировать внутренние DTD и объявления сущностей. Так как для атаки типа "отказ в обслуживании" можно использовать вредоносно сконструированные DTD и объявления сущностей, настоятельно рекомендуется, чтобы пользователи не передавали XML-документы из ненадежных источников в **sp_xml_preparedocument**.  
>   
>  Чтобы устранить рекурсивные атаки расширения сущности, **sp_xml_preparedocument** предельные значения до 10 000, которые могут быть развернуты под одной сущностью на верхнем уровне документа. Это ограничение не применяется к символьным и числовым сущностям. Ограничение позволяет хранить документы с большим количеством ссылок на сущности, но предотвращает рекурсивное расширение отдельной сущности в цепочку длиннее 10 000 расширений.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** ограничивает количество элементов, которые могут быть открыты за один раз в 256.  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_xml_preparedocument  
hdoc   
OUTPUT  
[ , xmltext ]  
[ , xpath_namespaces ]   
```  
  
## <a name="arguments"></a>Аргументы  
 *хдок*  
 Дескриптор только что созданного документа. *хдок* является целым числом.  
  
 [ *xmltext* ]  
 Исходный XML-документ. Средство синтаксического анализа MSXML анализирует этот XML-документ. *xmltext* — это текстовый параметр: **char**, **nchar**, **varchar**, **nvarchar**, **Text**, **ntext** или **XML**. Значение по умолчанию равно NULL. В этом случае создается внутреннее представление пустого XML-документа.  
  
> [!NOTE]  
>  **sp_xml_preparedocument** может обрабатывать только текст или нетипизированный XML. Если значение экземпляра, передающееся в качестве входного параметра, уже является типизированным XML, его сначала необходимо привести к новому нетипизированному экземпляру XML или к строке, после чего его можно передавать в качестве входного параметра. Дополнительные сведения см. в статье [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 [ *xpath_namespaces* ]  
 Указывает объявления пространств имен, которые используются в выражениях XPath строк и столбцов в OPENXML. *xpath_namespaces* — текстовый параметр: **char**, **nchar**, **varchar**, **nvarchar**, **Text**, **ntext** или **XML**.  
  
 Значение по умолчанию — **\<root xmlns:mp="urn:schemas-microsoft-com:xml-metaprop">** . *xpath_namespaces* предоставляет URI пространства имен для префиксов, используемых в выражениях XPath в OPENXML с помощью XML-документа правильного формата. *xpath_namespaces* объявляет префикс, который должен использоваться для ссылки на пространство имен **urn: schemas-microsoft-com: XML-metaprop**; Это предоставляет метаданные для проанализированных XML-элементов. Хотя с помощью данного приема можно переопределить префикс пространства имен для пространства имен метасвойства, это пространство имен не будет потеряно. Префикс **MP** по-прежнему действителен для **urn: schemas-microsoft-com: XML-metaprop** , даже если *xpath_namespaces* не содержит такого объявления.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или >0 (сбой)  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-preparing-an-internal-representation-for-a-well-formed-xml-document"></a>A. Подготовка внутреннего представления для XML-документа правильного формата  
 В следующем примере возвращается дескриптор вновь созданного внутреннего представления XML-документа, который передается как входной аргумент. При вызове процедуры `sp_xml_preparedocument` используется сопоставление префикса пространства имен по умолчанию.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
-- Remove the internal representation.  
exec sp_xml_removedocument @hdoc;  
```  
  
### <a name="b-preparing-an-internal-representation-for-a-well-formed-xml-document-with-a-dtd"></a>Б. Подготовка внутреннего представления для XML-документа правильного формата с DTD  
 В следующем примере возвращается дескриптор вновь созданного внутреннего представления XML-документа, который передается как входной аргумент. Хранимая процедура проверяет корректность загруженного документа с помощью DTD, включенного в документ. При вызове процедуры `sp_xml_preparedocument` используется сопоставление префикса пространства имен по умолчанию.  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(2000);  
SET @doc = '  
<?xml version="1.0" encoding="UTF-8" ?>   
<!DOCTYPE root   
[<!ELEMENT root (Customers)*>  
<!ELEMENT Customers EMPTY>  
<!ATTLIST Customers CustomerID CDATA #IMPLIED ContactName CDATA #IMPLIED>]>  
<root>  
<Customers CustomerID="ALFKI" ContactName="Maria Anders"/>  
</root>';  
  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc;  
```  
  
### <a name="c-specifying-a-namespace-uri"></a>В. Указание URI пространства имен  
 В следующем примере возвращается дескриптор вновь созданного внутреннего представления XML-документа, который передается как входной аргумент. Вызов метода `sp_xml_preparedocument` сохраняет `mp` префикс в сопоставлении пространства имен метасвойств и добавляет `xyz` префикс сопоставления в пространство имен `urn:MyNamespace` .  
  
```  
DECLARE @hdoc int;  
DECLARE @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>'  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @hdoc OUTPUT, @doc, '<ROOT xmlns:xyz="urn:MyNamespace"/>';  
```  
  
## <a name="see-also"></a>См. также:  
 <br>[Хранимые процедуры XML (Transact-SQL)](../../relational-databases/system-stored-procedures/xml-stored-procedures-transact-sql.md)
 <br>[Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
 <br>[OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md)
 <br>[sys.dm_exec_xml_handles (Transact-SQL)](../system-dynamic-management-views/sys-dm-exec-xml-handles-transact-sql.md)
 <br>[sp_xml_removedocument (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-xml-removedocument-transact-sql.md)
 <br>[nodes (метод) (тип данных xml)](../../t-sql/xml/nodes-method-xml-data-type.md)
  
  

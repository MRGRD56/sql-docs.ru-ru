---
title: Проблемы параллелизма базы данных в диаграмм обновления (SQLXML)
description: Узнайте, как использовать механизм управления оптимистичным параллелизмом в диаграмм обновления (SQLXML 4,0) для обработки проблем параллелизма базы данных.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- <before> block
- low concurrency protection
- database concurrency [SQLXML]
- timestamp column [SQLXML]
- updategrams [SQLXML], database concurrency
- high concurrency protection [SQLXML]
- optimistic concurrency control
- concurrency [SQLXML]
- intermediate concurrency protection [SQLXML]
ms.assetid: d4b908d1-b25b-4ad9-8478-9cd882e8c44e
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1c3ab2705054e74b15413e87ceb0d5179f0ffc4f
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491581"
---
# <a name="handling-database-concurrency-issues-in-updategrams-sqlxml-40"></a>Решение проблем с параллелизмом обработки в базе данных в диаграммах обновления (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Как и другие механизмы обновления базы данных, диаграммы обновления должны иметь дело с одновременными обновлениями данных в многопользовательской среде. В диаграммах обновления используется оптимистическое управление параллелизмом, которое использует сравнение данных в выбранных полях как моментальных снимков, чтобы данные, которые нужно обновить, не были изменены другим пользовательским приложением после чтения из базы данных. Диаграмм обновления включают эти значения моментальных снимков в **\<before>** блок диаграмм обновления. Перед обновлением базы данных диаграмма обновления проверяет значения, указанные в блоке, на **\<before>** значения, находящиеся в базе данных, чтобы убедиться, что обновление является допустимым.  
  
 Управление оптимистичным параллелизмом обеспечивает три уровня защиты в диаграмме обновления: низкий (нет), промежуточный и высокий. Можно выбрать нужный уровень защиты, указав соответствующую диаграмму обновления.  
  
## <a name="lowest-level-of-protection"></a>Самый низкий уровень защиты  
 Этот уровень обеспечивает «слепое» обновление, при котором обновление обрабатывается без учета других обновлений, сделанных со времени последнего чтения базы данных. В этом случае необходимо указать только первичные ключевые столбцы в **\<before>** блоке, чтобы определить запись, и указать обновленные сведения в **\<after>** блоке.  
  
 Например, новый номер телефона для связи в следующей диаграмме обновления верен, независимо от предыдущего значения телефонного номера. Обратите внимание, что **\<before>** блок определяет только первичный ключевой столбец (ContactID).  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
   <Person.Contact ContactID="1" />  
</updg:before>  
<updg:after>  
   <Person.Contact ContactID="1" Phone="111-111-1111" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="intermediate-level-of-protection"></a>Промежуточный уровень защиты  
 На этом уровне защиты диаграмма обновления сравнивает текущие значения обновляемых данных со значениями в столбцах базы данных, чтобы проверить, что значения не были изменены другой транзакцией после чтения записи данной транзакцией.  
  
 Этот уровень защиты можно получить, указав первичные ключевые столбцы и столбцы, которые обновляются в **\<before>** блоке.  
  
 Например, данная диаграмма обновления изменяет значение в столбце Phone таблицы Person.Person для контактного лица со значением ContactID, равным 1. **\<before>** Блок задает атрибут **Phone** , чтобы убедиться, что значение атрибута соответствует значению в соответствующем столбце в базе данных перед применением обновленного значения.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
   <Person.Contact ContactID="1" Phone="398-555-0132" />  
</updg:before>  
<updg:after>  
   <Person.Contact ContactID="1" Phone="111-111-1111" />  
</updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="high-level-of-protection"></a>Высокий уровень защиты  
 Высокий уровень защиты гарантирует, что запись остается неизменной со времени последнего чтения записи приложением (то есть после чтения записи приложением она не была изменена никакой другой транзакцией).  
  
 Существует два способа получить высокий уровень защиты от одновременных обновлений.  
  
-   Укажите дополнительные столбцы в таблице в **\<before>** блоке.  
  
     Если указать дополнительные столбцы в **\<before>** блоке, диаграмма обновления сравнивает значения, указанные для этих столбцов, со значениями, которые были в базе данных перед применением обновления. Если какие-нибудь столбцы записи изменились после чтения записи транзакцией, диаграмма обновления не выполняет обновление.  
  
     Например, следующий диаграмма обновления обновляет имя смены, но указывает дополнительные столбцы (StartTime, EndTime) в **\<before>** блоке, тем самым запрашивая более высокий уровень защиты от параллельных обновлений.  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
    <updg:sync >  
    <updg:before>  
       <HumanResources.Shift ShiftID="1"   
                 Name="Day"   
                 StartTime="1900-01-01 07:00:00.000"   
                 EndTime="1900-01-01 15:00:00.000" />  
    </updg:before>  
    <updg:after>  
       <HumanResources.Shift Name="Morning" />  
    </updg:after>  
    </updg:sync>  
    </ROOT>  
    ```  
  
     Этот пример задает наивысший уровень защиты, указывая все значения столбцов для записи в **\<before>** блоке.  
  
-   Укажите столбец отметок времени (если он доступен) в **\<before>** блоке.  
  
     Вместо того чтобы указывать все столбцы записи в \<before**> блоке * *, можно просто указать столбец timestamp (если таблица содержит одну) вместе с первичными ключевыми столбцами в **\<before>** блоке. База данных обновляет столбец отметки времени уникальным значением после каждого обновления записи. В этом случае диаграмма обновления сравнивает значения отметок времени с соответствующим значением в базе данных. Значение отметки времени, сохраненное в базе данных, является двоичным значением. Поэтому столбец timestamp должен быть указан в схеме как **DT: Type = "bin. Hex"**, **DT: Type = "bin. base64"** или **SQL: datatype = "timestamp"**. (Можно указать либо тип данных **XML** , либо [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] тип данных.)  
  
#### <a name="to-test-the-updategram"></a>Тестирование диаграммы обновления  
  
1.  Создайте эту таблицу в базе данных **tempdb** :  
  
    ```  
    USE tempdb  
    CREATE TABLE Customer (  
                 CustomerID  varchar(5),  
                 ContactName varchar(20),  
                 LastUpdated timestamp)  
    ```  
  
2.  Добавьте следующий образец записи:  
  
    ```  
    INSERT INTO Customer (CustomerID, ContactName) VALUES   
                         ('C1', 'Andrew Fuller')  
    ```  
  
3.  Скопируйте и вставьте следующую схему XSD в приложение «Блокнот». Сохраните код в файл с именем ConcurrencySampleSchema.xml:  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
      <xsd:element name="Customer" sql:relation="Customer" >  
       <xsd:complexType>  
            <xsd:attribute name="CustomerID"    
                           sql:field="CustomerID"   
                           type="xsd:string" />   
  
            <xsd:attribute name="ContactName"    
                           sql:field="ContactName"   
                           type="xsd:string" />  
  
            <xsd:attribute name="LastUpdated"   
                           sql:field="LastUpdated"   
                           type="xsd:hexBinary"   
                 sql:datatype="timestamp" />  
  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
4.  Скопируйте следующий код диаграммы обновления в приложении «Блокнот» и сохраните его в файле с именем ConcurrencySampleTemplate.xml в том же каталоге, в котором сохранена схема, созданная на предыдущем шаге. Обратите внимание, что указанное ниже значение отметки времени для столбца LastUpdated будет иным, нежели в примере таблицы Customer, поэтому скопируйте фактическое значение для LastUpdated из таблицы и вставьте его в диаграмму обновления.  
  
    ```  
    <ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
    <updg:sync mapping-schema="SampleSchema.xml" >  
    <updg:before>  
       <Customer CustomerID="C1"   
                 LastUpdated = "0x00000000000007D1" />  
    </updg:before>  
    <updg:after>  
       <Customer ContactName="Robert King" />  
    </updg:after>  
    </updg:sync>  
    </ROOT>  
    ```  
  
5.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Эквивалентная схема XDR:  
  
```  
<?xml version="1.0" ?>  
<Schema xmlns="urn:schemas-microsoft-com:xml-data"  
        xmlns:dt="urn:schemas-microsoft-com:datatypes"  
        xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
<ElementType name="Customer" sql:relation="Customer" >  
    <AttributeType name="CustomerID" />  
    <AttributeType name="ContactName" />  
    <AttributeType name="LastUpdated"  dt:type="bin.hex"   
                                       sql:datatype="timestamp" />  
    <attribute type="CustomerID" />  
    <attribute type="ContactName" />  
    <attribute type="LastUpdated" />  
</ElementType>  
</Schema>  
```  
  
## <a name="see-also"></a>См. также  
 [Вопросы безопасности диаграмма обновления &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  

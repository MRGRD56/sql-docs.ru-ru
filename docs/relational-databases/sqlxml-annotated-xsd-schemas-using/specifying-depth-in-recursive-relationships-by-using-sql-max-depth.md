---
title: 'Настройка рекурсивных отношений глубины с помощью SQL: max-depth'
description: 'Узнайте, как задать глубину при запросе таблиц с рекурсивной связью с помощью аннотации SQL: max-depth в XQuery.'
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- max-depth annotation
- XPath queries [SQLXML], recursive relationships
- depth in recursive relationships [SQLXML]
- annotated XSD schemas, recursive relationships
- relationships [SQLXML], recursive relationships
- self joins
- recursive relationships [SQLXML]
- recursion [SQLXML]
- sql:max-depth
- recursive joins [SQLXML]
ms.assetid: 0ffdd57d-dc30-44d9-a8a0-f21cadedb327
author: rothja
ms.author: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f917c1801acd7c5c0b219d0e2adbfd5f4b4aa221
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490753"
---
# <a name="specifying-depth-in-recursive-relationships-by-using-sqlmax-depth"></a>Задание глубины рекурсивных связей с использованием sql:max-depth
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В реляционных базах данных участие таблицы в связи с самой собой называется рекурсивной связью. Например, таблица, в которой содержатся записи о сотрудниках и возможны связи «начальник-подчиненный», участвует в связи сама с собой. В этом случае таблица сотрудников выполняет роль начальника на одной стороне связи и та же таблица выполняет роль подчиненного на другой стороне.  
  
 Схемы сопоставления могут включать рекурсивные связи, в которых элемент и его предок относятся к одному типу.  
  
## <a name="example-a"></a>Пример A  
 Рассмотрим следующую таблицу.  
  
```  
Emp (EmployeeID, FirstName, LastName, ReportsTo)  
```  
  
 В этой таблице столбец ReportsTo содержит идентификатор служащего для менеджера.  
  
 Предположим, что нужно сформировать XML-иерархию служащих, на вершине которой находится менеджер, а служащие, подчиненные менеджеру, отображаются в соответствующей иерархии, как показано в следующем образце XML-фрагмента. Этот фрагмент показывает *рекурсивное дерево* для сотрудника 1.  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
     <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
     <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
        <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
          <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
...  
...  
</root>  
```  
  
 В этом фрагменте служащий 5 подчинен служащему 4, служащий 4 подчинен служащему 3, а служащие 3 и 2 подчинены служащему 1.  
  
 Чтобы получить этот результат, можно использовать следующую схему XSD и указать запрос XPath к ней. Схема описывает **\<Emp>** элемент типа типсотрудника, состоящий из **\<Emp>** дочернего элемента того же типа, типсотрудника. Это рекурсивная связь (элемент и его предок относятся к одному типу). Кроме того, схема использует **\<sql:relationship>** для описания связи «родители-потомки» между супервизором и администратором. Обратите внимание, что в этом **\<sql:relationship>** случае EMP является родительской и дочерней таблицей.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="6" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 Поскольку связь рекурсивная, необходимо каким-то образом указать глубину рекурсии в схеме. В противном случае результатом будет бесконечная рекурсия (служащий, подчиненный служащему, подчиненному служащему, и т. д.). Аннотация **SQL: max-depth** позволяет указать, насколько глубоко должна идти рекурсия. В этом конкретном примере для указания значения **SQL: max-depth** необходимо иметь представление о том, насколько глубока иерархия управления в компании.  
  
> [!NOTE]  
>  Схема указывает заметку **SQL: limit-field** , но не задает аннотацию **SQL: limit-value** . Это ограничивает верхний узел в результирующей иерархии только служащими, которые не подчиняются никому. (Подчиняется NULL.) Если указать **SQL: limit-field** и не указать **SQL: limit-value** (значение по умолчанию NULL), это достигается в аннотации. Если требуется, чтобы результирующий XML включал все возможные деревья отчетов (дерево отчетов для каждого сотрудника в таблице), удалите заметку **SQL: limit-field** из схемы.  
  
> [!NOTE]  
>  В следующей процедуре используется база данных tempdb.  
  
#### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Проверка образца запроса XPath к схеме  
  
1.  Создайте образец таблицы с именем Emp в базе данных tempdb, на которую указывает виртуальный корневой каталог.  
  
    ```  
    USE tempdb  
    CREATE TABLE Emp (  
           EmployeeID int primary key,   
           FirstName  varchar(20),   
           LastName   varchar(20),   
           ReportsTo int)  
    ```  
  
2.  Добавьте следующий образец данных:  
  
    ```  
    INSERT INTO Emp values (1, 'Nancy', 'Devolio',NULL)  
    INSERT INTO Emp values (2, 'Andrew', 'Fuller',1)  
    INSERT INTO Emp values (3, 'Janet', 'Leverling',1)  
    INSERT INTO Emp values (4, 'Margaret', 'Peacock',3)  
    INSERT INTO Emp values (5, 'Steven', 'Devolio',4)  
    INSERT INTO Emp values (6, 'Nancy', 'Buchanan',5)  
    INSERT INTO Emp values (7, 'Michael', 'Suyama',6)  
    ```  
  
3.  Скопируйте приведенный выше код схемы и вставьте его в текстовый файл. Сохраните файл под именем maxDepth.xml.  
  
4.  Скопируйте следующий шаблон и вставьте его в текстовый файл. Сохраните файл под именем maxDepthT.xml в том же каталоге, где был сохранен файл maxDepth.xml. Запрос в шаблоне возвращает всех сотрудников в таблице Emp.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="maxDepth.xml">  
        /Emp  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу схемы сопоставления (файл maxDepth.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\MyDir\maxDepth.xml"  
    ```  
  
5.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон. Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  

 Результат:  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
    <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
      <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
        <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
          <Emp FirstName="Nancy" EmployeeID="6" LastName="Buchanan">  
            <Emp FirstName="Michael" EmployeeID="7" LastName="Suyama" />   
          </Emp>  
        </Emp>  
      </Emp>  
    </Emp>  
  </Emp>  
</root>  
```  
  
> [!NOTE]  
>  Чтобы создать различные глубины иерархий в результате, измените значение заметки **SQL: max-depth** в схеме и снова выполните шаблон после каждого изменения.  
  
 В предыдущей схеме все **\<Emp>** элементы имели одинаковый набор атрибутов (**EmployeeID**, **FirstName** и **LastName**). Следующая схема была немного изменена, чтобы вернуть дополнительный атрибут **подчиняется** для всех **\<Emp>** элементов, подчиненных руководителю.  
  
 Например, этот XML-фрагмент показывает подчиненных служащего 1:  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
<Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2"   
       ReportsTo="1" LastName="Fuller" />   
  <Emp FirstName="Janet" EmployeeID="3"   
       ReportsTo="1" LastName="Leverling">  
...  
...  
```  
  
 Ниже приведена измененная схема:  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:documentation>  
      Customer-Order-Order Details Schema  
      Copyright 2000 Microsoft. All rights reserved.  
    </xsd:documentation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"  
                  parent-key="EmployeeID"  
                  child="Emp"  
                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
                   type="EmpType"   
                   sql:relation="Emp"   
                   sql:key-fields="EmployeeID"   
                   sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmpType">  
    <xsd:sequence>  
       <xsd:element name="Emp"   
                    type="EmpType"   
                    sql:relation="Emp"   
                    sql:key-fields="EmployeeID"  
                    sql:relationship="SupervisorSupervisee"  
                    sql:max-depth="6"/>  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:int" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
    <xsd:attribute name="ReportsTo" type="xsd:int" />  
  </xsd:complexType>  
</xsd:schema>  
```  
  
## <a name="sqlmax-depth-annotation"></a>Заметка sql:max-depth  
 В схеме, состоящей из рекурсивных связей, глубина рекурсии должна быть явно указана в схеме. Это необходимо для успешной подготовки соответствующего запроса FOR XML EXPLICIT, который возвращает запрошенные результаты.  
  
 Используйте аннотацию **SQL: max-depth** в схеме, чтобы указать глубину рекурсии рекурсивной связи, описанной в схеме. Значение аннотации **SQL: max-depth** является положительным целым числом (от 1 до 50), указывающим число рекурсий: значение 1 останавливает рекурсию в элементе, для которого задана Аннотация **SQL: max-depth** ; значение 2 останавливает рекурсию на следующем уровне из элемента, в котором указан **SQL: max-depth** . и т. д.  
  
> [!NOTE]  
>  В базовой реализации запрос XPath, заданный для схемы сопоставления, преобразуется в SELECT... Запрос FOR XML EXPLICIT. Для этого запроса необходимо указать конечную глубину рекурсии. Чем выше значение, заданное для **SQL: max-depth**, тем больше создается запрос FOR XML EXPLICIT. Это может увеличить время выборки.  
  
> [!NOTE]  
>  Диаграммы обновления и массовая загрузка XML не учитывают заметку max-depth. Это означает, что рекурсивные обновления и вставки будут происходить независимо от значения, заданного для max-depth.  
  
## <a name="specifying-sqlmax-depth-on-complex-elements"></a>Задание sql:max-depth для сложных элементов  
 Аннотацию **SQL: max-depth** можно указать для любого сложного элемента содержимого.  
  
### <a name="recursive-elements"></a>Рекурсивные элементы  
 Если **SQL: max-depth** задан как для родительского элемента, так и для дочернего элемента в рекурсивной связи, приоритет имеет заметку **SQL: max-depth** , указанную для родителя. Например, в следующей схеме Аннотация **SQL: max-depth** задается как в родительском, так и в дочернем элементах Employee. В этом случае **SQL: max-depth = 4**, указанная в **\<Emp>** родительском элементе (которая играет роль супервизора), имеет приоритет. Значение **SQL: max-depth** , заданное в дочернем **\<Emp>** элементе (которая играет роль контролируемого элемента), игнорируется.  
  
#### <a name="example-b"></a>Пример Б  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo"   
                          sql:max-depth="3" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="2" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 Для проверки этой схемы выполните шаги, приведенные для образца А ранее в этом разделе.  
  
### <a name="nonrecursive-elements"></a>Нерекурсивные элементы  
 Если Аннотация **SQL: max-depth** задана для элемента в схеме, который не вызывает рекурсию, он игнорируется. В следующей схеме **\<Emp>** элемент состоит из **\<Constant>** дочернего элемента, который, в свою очередь, имеет **\<Emp>** дочерний элемент.  
  
 В этой схеме Аннотация **SQL: max-depth** , указанная в **\<Constant>** элементе, игнорируется, поскольку между **\<Emp>** родительским и **\<Constant>** дочерним элементами отсутствует рекурсия. Но существует рекурсия между **\<Emp>** предком и **\<Emp>** дочерним объектом. Схема указывает аннотацию **SQL: max-depth** для обоих. Поэтому приоритет имеет заметку **SQL: max-depth** , указанную для предка ( **\<Emp>** в роли супервизора).  
  
#### <a name="example-c"></a>Пример В  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"   
                  child="Emp"   
                  parent-key="EmployeeID"   
                  child-key="ReportsTo"/>  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
               sql:relation="Emp"   
               type="EmpType"  
               sql:limit-field="ReportsTo"  
               sql:max-depth="1" />  
    <xsd:complexType name="EmpType" >  
      <xsd:sequence>  
       <xsd:element name="Constant"   
                    sql:is-constant="1"   
                    sql:max-depth="20" >  
         <xsd:complexType >  
           <xsd:sequence>  
            <xsd:element name="Emp"   
                         sql:relation="Emp" type="EmpType"  
                         sql:relationship="SupervisorSupervisee"   
                         sql:max-depth="3" />  
         </xsd:sequence>  
         </xsd:complexType>  
         </xsd:element>  
      </xsd:sequence>  
      <xsd:attribute name="EmployeeID" type="xsd:int" />  
    </xsd:complexType>  
</xsd:schema>  
```  
  
 Для проверки этой схемы выполните шаги, приведенные для примера А ранее в этом разделе.  
  
## <a name="complex-types-derived-by-restriction"></a>Сложные типы, наследуемые по ограничению  
 При наличии сложного типа, производного от **\<restriction>** , элементы соответствующего базового сложного типа не могут задавать аннотацию **SQL: max-depth** . В этих случаях в элемент производного типа можно добавить аннотацию **SQL: max-depth** .  
  
 С другой стороны, если имеется сложный тип, производный от **\<extension>** , элементы соответствующего базового сложного типа могут указать аннотацию **SQL: max-depth** .  
  
 Например, следующая схема XSD создает ошибку, так как Аннотация **SQL: max-depth** задана для базового типа. Эта аннотация не поддерживается для типа, производного **\<restriction>** от другого типа. Чтобы устранить эту проблему, необходимо изменить схему и указать для элемента в производном типе аннотацию **SQL: max-depth** .  
  
#### <a name="example-d"></a>Пример Г  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:msdata="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:complexType name="CustomerBaseType">   
    <xsd:sequence>  
       <xsd:element name="CID" msdata:field="CustomerID" />  
       <xsd:element name="CompanyName"/>  
       <xsd:element name="Customers" msdata:max-depth="3">  
         <xsd:annotation>  
           <xsd:appinfo>  
             <msdata:relationship  
                     parent="Customers"  
                     parent-key="CustomerID"  
                     child-key="CustomerID"  
                     child="Customers" />  
           </xsd:appinfo>  
         </xsd:annotation>  
       </xsd:element>  
    </xsd:sequence>  
  </xsd:complexType>  
  <xsd:element name="Customers" type="CustomerType"/>  
  <xsd:complexType name="CustomerType">  
    <xsd:complexContent>  
       <xsd:restriction base="CustomerBaseType">  
          <xsd:sequence>  
            <xsd:element name="CID"   
                         type="xsd:string"/>  
            <xsd:element name="CompanyName"   
                         type="xsd:string"  
                         msdata:field="CName" />  
            <xsd:element name="Customers"   
                         type="CustomerType" />  
          </xsd:sequence>  
       </xsd:restriction>  
    </xsd:complexContent>  
  </xsd:complexType>  
</xsd:schema>   
```  
  
 В схеме **SQL: max-depth** задается для сложного типа **кустомербасетипе** . Схема также задает **\<Customer>** элемент типа **CustomerType**, который является производным от **кустомербасетипе**. Запрос XPath, указанный в такой схеме, выдаст ошибку, поскольку **SQL: max-depth** не поддерживается для элемента, определенного в базовом типе ограничения.  
  
## <a name="schemas-with-a-deep-hierarchy"></a>Схемы с глубокой иерархией  
 Схема может иметь глубокую иерархию, в которой элемент содержит дочерний элемент, который в свою очередь содержит другой дочерний элемент, и т. д. Если заметка **SQL: max-depth** , указанная в такой схеме, создает XML-документ, содержащий иерархию более чем 500 уровней (с элементом верхнего уровня на уровне 1, его дочерний элемент на уровне 2 и т. д.), возвращается ошибка.  
  
  

---
title: Объект SqlXmlCommand (SQLXML)
description: Сведения о методах и свойствах объекта SqlXmlCommand.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- void ExecuteNonQuery() method
- namespaces property
- Stream ExecuteStream() method
- CommandType property
- RootTag property
- OutputEncoding property
- XmlReader ExecuteXmlReader() method
- Managed Classes [SQLXML], SqlXmlCommand object
- SQLXML Managed Classes, SqlXmlCommand object
- Base Path property
- void ClearParameters() method
- public void ExecuteToStream(Stream outputStream) method
- SqlXmlCommand object
- CommandText property
- XslPath property
- SchemaPath property
- SqlXmlParameter CreateParameter() method
- ClientSideXML property
- CommandStream property
ms.assetid: c1f9e0bb-a89d-4d6a-a96e-289ef516a3a6
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 220eb6809e86042f0ed5731831acdbf12f8457a3
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491705"
---
# <a name="sqlxml-managed-classes---sqlxmlcommand-object"></a>Управляемые классы SQLXML — объект SqlXmlCommand
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Это конструктор для объекта SqlXmlCommand:  
  
```  
public SqlXmlCommand(string cnString)  
```  
  
 Где `cnString` — строка подключения ADO или OLEDB, идентифицирующая сервер, базу данных и сведения об имени входа, например `Provider=SQLOLEDB; Server=(local); database=AdventureWorks; Integrated Security=SSPI"` .  
  
 В строке соединения параметр `Provider` должен иметь значение SQLOLEDB, а параметр `Data Provider` в строку поставщика не включается).  
  
 Рабочий пример см. в разделе [Выполнение SQL-запросов &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
## <a name="methods"></a>Методы  
 Объект Сесклксмлкомманд поддерживает несколько методов, включая следующие методы для исполнения команды:  
  
 void ExecuteNonQuery ()  
 Выполняет команду, но не возвращает никакого результата. Этот метод полезен для выполнения команд, которые не являются запросами (то есть команд, которые ничего не возвращают). Например, выполнение диаграммы обновления или дельты, которая изменяет записи, но ничего не возвращает.  
  
 Stream Ексекутестреам ()  
 Возвращает новый объект потока. Этот метод полезен, если нужно вернуть результаты запроса в новом потоке. Рабочий пример см. в разделе [Выполнение SQL-запросов &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 открытая void Ексекутетостреам (Stream outputStream)  
 Записывает результаты запроса в существующий поток. Этот метод полезен при наличии потока, к которому должны быть добавлены результаты (например, чтобы результаты запроса записывались в System. Web. HttpResponse. OutputStream). Рабочий пример см. в разделе [Выполнение SQL-запросов &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 XmlReader ExecuteXmlReader ()  
 Возвращает объект XmlReader. Этот метод можно использовать для непосредственного управления данными в объекте XmlReader или подключения к связанной архитектуре System.Xml. Дополнительные сведения см. в документации по платформе [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework. Рабочий пример см. в разделе [Выполнение SQL-запросов с помощью метода ExecuteXMLReader](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md).  
  
 Объект Сесклксмлкомманд также поддерживает следующие дополнительные методы:  
  
 SqlXmlParameter CreateParameter ()  
 Создает объект SqlXmlParameter. Можно задать значения для параметров *Name* и *value* этого объекта. Этот метод полезен для передачи параметров команды. Рабочий пример см. в разделе [Выполнение SQL-запросов &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 void ClearParameters ()  
 Очищает параметры, созданные для данного командного объекта. Это удобно, если нужно выполнить несколько запросов, используя один и тот же командный объект.  
  
## <a name="properties"></a>Свойства  
 Объект SqlXmlCommand также поддерживает следующие свойства:  
  
 клиентсидексмл  
 Если для этого свойства задать значение True, преобразование набора строк в XML будет происходить на клиенте, а не на сервере. Это свойство полезно, если нужно переместить вычислительную нагрузку на средний уровень архитектуры. Это свойство также позволяет упаковывать существующие хранимые процедуры в инструкции FOR XML для получения вывода в формате XML.  
  
 SchemaPath  
 Название схемы сопоставления вместе с путем к каталогу (например, C:\x\y\MySchema.xml). Это свойство полезно для задания схемы сопоставления в запросах XPath. Может быть указан абсолютный или относительный путь. Если путь является относительным, то базовый путь, указанный в качестве базового пути, используется для разрешения относительного пути. Если базовый путь не указан, то относительный путь разрешается относительно текущего каталога. Рабочий пример см. в разделе [доступ к функциям SQLXML в среде .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
 ксслпас  
 Имя XSL-файла, включая путь к каталогу. Может быть указан абсолютный или относительный путь. Если путь является относительным, то базовый путь, указанный в качестве базового пути, используется для разрешения относительного пути. Если базовый путь не указан, то относительный путь разрешается относительно текущего каталога. Рабочий пример см. в разделе [применение преобразования XSL &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md).  
  
 Базовый путь  
 Базовый путь (путь к каталогу). Это свойство полезно для разрешения относительного пути, указанного для XSL-файла (с помощью свойства Ксслпас), файла схемы сопоставления (с помощью свойства Счемапас) или ссылки на внешнюю схему в XML-шаблоне (указывается с помощью атрибута **Mapping-Schema** ).  
  
 OutputEncoding  
 Указывает кодировку потока, который возвращается при выполнении программы. С помощью этого свойства удобно задавать конкретную кодировку для возвращаемого потока. Примеры распространенных кодировок: UTF-8, ANSI и Unicode. По умолчанию используется UTF-8.  
  
 Пространства имен  
 Разрешает выполнение запросов XPath, использующих пространства имен. Дополнительные сведения о запросах XPath с пространствами имен см. в разделе [исполнение запросов XPath с пространствами имен &#40;управляемые классы SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-with-namespaces-sqlxml-managed-classes.md). Рабочий пример см. в разделе [выполнение запросов XPath &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-sqlxml-managed-classes.md).  
  
 руттаг  
 Задает единственный корневой элемент для XML-документа, сформированного при выполнении команды. У допустимого XML-документа может быть только один тег на корневом уровне. Если выполненная команда формирует фрагмент XML (у которого нет единственного элемента верхнего уровня), можно задать корневой элемент для возвращаемого фрагмента XML. Рабочий пример см. в разделе [применение преобразования XSL &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md).  
  
 CommandText  
 Текст команды. Это свойство используется для задания текста команды, которую нужно выполнить. Рабочий пример см. в разделе [Выполнение SQL-запросов &#40;управляемых классов SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md).  
  
 CommandStream  
 Командный поток. Это свойство служит для выполнения команды из файла (например, XML-шаблона). При использовании CommandStream поддерживаются только значения CommandType **"Template"**, **"диаграмма обновления"** и **"DiffGram"** . Рабочий пример см. в разделе [выполнение файлов шаблонов с помощью свойства CommandStream](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandstream-property.md).  
  
 CommandType  
 Идентифицирует тип команды. Это свойство используется для задания типа команды, которую нужно выполнить. Значения в следующей таблице задают тип команды. Рабочий пример см. в разделе [доступ к функциям SQLXML в среде .NET](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
|Значение|Описание|  
|-----------|-----------------|  
|Склксмлкоммандтипе. SQL|Выполняет команду SQL (например, `SELECT * FROM Employees FOR XML AUTO`).|  
|Склксмлкоммандтипе. XPath|Выполняет команду XPath (например, `Employees[@EmployeeID=1]`).|  
|Склксмлкоммандтипе. шаблон|Выполняет шаблон XML.|  
|Склксмлкоммандтипе. TemplateFile|Выполняет файл шаблона, расположенный по указанному пути.|  
|Склксмлкоммандтипе. Диаграмма обновления|Выполняет диаграмму обновления.|  
|Склксмлкоммандтипе. DiffGram|Выполняет дельту.|  
  
## <a name="see-also"></a>См. также  
 [Объект SqlXmlParameter &#40;управляемые классы SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlparameter-object.md)   
 [Объект Склксмладаптер &#40;управляемые классы SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmladapter-object.md)  
  
  

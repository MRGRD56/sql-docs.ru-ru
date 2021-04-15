---
title: Загрузка XML-данных | Документация Майкрософт
description: Узнайте о нескольких методах передачи XML-данных в базы данных SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XML data [SQL Server], loading
- loading XML data
ms.assetid: d1741e8d-f44e-49ec-9f14-10208b5468a7
author: rothja
ms.author: jroth
ms.openlocfilehash: 640007b28617c9bc703c9850be2be2a3a9251bf5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488272"
---
# <a name="load-xml-data"></a>Загрузка XML-данных
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Есть несколько способов передачи XML-данных в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] . Пример:  
  
-   Если в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] данные хранятся в столбце типа [n]text или image, то эту таблицу можно импортировать с помощью служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Изменить тип столбца на XML можно с использованием инструкции ALTER TABLE.  
  
-   Массовое копирование данных из другой базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно выполнить с использованием команды bcp out, после чего с помощью команды bcp in произвести массовую вставку данных в базу данных более поздней версии.  
  
-   Если в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] данные хранятся в реляционных столбцах, необходимо создать новую таблицу со столбцом [n]text и, возможно, с первичным ключевым столбцом для идентификации строк. Чтобы получить XML-данные, созданные на сервере с помощью инструкции FOR XML, и записать их в столбец **[n]text** , требуется программный код на клиентской стороне. Затем эти данные необходимо передать в базу данных более поздней версии, выбрав любую из вышеупомянутых методик. XML-данные можно напрямую записать в XML-столбец базы данных более поздней версии.  
  
## <a name="bulk-loading-xml-data"></a>Массовая загрузка XML-данных  
 Массовую загрузку XML-данных на сервер можно осуществить при помощи реализованных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]средств массовой загрузки, таких как bcp. Инструкция OPENROWSET позволяет загрузить данные в XML-столбец из файлов. Это показано в следующем примере.  
  
##### <a name="example-loading-xml-from-files"></a>Пример Загрузка XML-данных из файлов  
 Следующий пример показывает, как вставить строку в таблицу T. Значение XML-столбца загружается из файла «C:\MyFile\xmlfile.xml» как объект CLOB, а целочисленному столбцу назначается значение 10.  
  
```  
INSERT INTO T  
SELECT 10, xCol  
FROM    (SELECT *      
    FROM OPENROWSET (BULK 'C:\MyFile\xmlfile.xml', SINGLE_CLOB)   
 AS xCol) AS R(xCol)  
```  
  
## <a name="text-encoding"></a>Кодировка текста  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] хранит XML-данные в кодировке Юникод (UTF-16). XML-данные, извлекаемые из баз данных сервера, предоставляются в кодировке UTF-16. Если требуются данные в другой кодировке, извлеченные данные нужно преобразовать. Иногда XML-данные могут быть представлены в другой кодировке. Если это так, во время загрузки данных нужно быть внимательным. Пример:  
  
-   Если текст XML представлен в кодировке Юникод (UCS-2, UTF-16), можно назначить его XML-столбцу, переменной или параметру без каких-либо проблем.  
  
-   Если кодировка отлична от Юникода и неявна из-за исходной кодовой страницы, строковая кодовая страница в базе данных должна быть той же самой или совместимой с элементами кода, которые следует загрузить. Если нужно, используйте предложение COLLATE. Если такой кодовой страницы на сервере не существует, необходимо добавить явную XML-декларацию с корректной кодировкой.  
  
-   Чтобы явно задать кодировку, воспользуйтесь типом **varbinary()** , который не зависит от кодовых страниц, либо символьным типом для соответствующей кодовой страницы. После этого назначьте данные XML-столбцу, переменной или параметру.  
  
### <a name="example-explicitly-specifying-an-encoding"></a>Пример явное указание кодировки  
 Предположим, что есть XML-документ vcdoc, хранящийся как **varchar(max)** , который не объявлен явно как XML. Приведенная ниже инструкция добавляет объявление XML с кодировкой "iso8859-1", присоединяет к нему XML-документ, приводит результат к типу **varbinary(max)** (чтобы сохранить двоичное представление) и, наконец, приводит его к типу XML. Это позволяет процессору XML выполнять синтаксический анализ данных в соответствии с указанной кодировкой «iso8859-1» и создавать для строковых значений соответствующее представление UTF-16.  
  
```  
SELECT CAST(   
CAST (('<?xml version="1.0" encoding="iso8859-1"?>'+ vcdoc) AS VARBINARY (MAX))   
 AS XML)  
```  
  
### <a name="string-encoding-incompatibilities"></a>Несоответствия кодировок строк  
 При копировании и вставке XML как строкового литерала в окно редактора запросов служб в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]могут возникнуть несоответствия с кодировкой строк типа (N)VARCHAR. Это будет зависеть от кодировки копируемого экземпляра XML. Во многих случаях может возникнуть необходимость удаления XML-декларации. Пример:  
  
```  
<?xml version="1.0" encoding="UTF-8"?>  
  <xsd:schema ...  
```  
  
 Затем нужно будет добавить N, чтобы сделать экземпляр XML экземпляром Юникода. Пример:  
  
```  
-- Assign XML instance to a variable.  
DECLARE @X XML  
SET @X = N'...'  
-- Insert XML instance into an xml type column.  
INSERT INTO T VALUES (N'...')  
-- Create an XML schema collection  
CREATE XML SCHEMA COLLECTION XMLCOLL1 AS N'<xsd:schema ... '  
```  
  
## <a name="see-also"></a>См. также:  
 [Данные XML (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
  

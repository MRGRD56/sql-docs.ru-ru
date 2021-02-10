---
description: Сохранение записей в формате XML
title: Сохранение записей в формате XML | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- XML persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: f3113ec4-ae31-428f-89c6-bc1024f128ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 6f29f624fe9f55287dcd8944fd04da0426d9552c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032626"
---
# <a name="persisting-records-in-xml-format"></a>Сохранение записей в формате XML
Как и в формате АДТГ, сохраняемость **набора записей** в формате XML реализуется с помощью поставщика сохраняемости Microsoft OLE DB. Этот поставщик создает однопроходный набор строк только для чтения из сохраненного XML-файла или потока, содержащего сведения о схеме, созданные ADO. Аналогичным образом он может принимать **набор записей** ADO, формировать XML и сохранять его в файл или любой объект, реализующий интерфейс COM **IStream** . (Фактически, файл — это просто еще один пример объекта, который поддерживает **IStream**.) В версиях 2,5 и более поздних, ADO использует Microsoft XML Parser (MSXML) для загрузки XML в **набор записей**; Поэтому msxml.dll является обязательным.  
  
> [!NOTE]
>  Некоторые ограничения применяются при сохранении иерархических **наборов записей** (фигур данных) в формате XML. Нельзя сохранить в XML, если иерархический **набор записей** содержит ожидающие обновления, и нельзя сохранить параметризованный иерархический **набор записей**. Дополнительные сведения см. в разделе [Сохранение отфильтрованных и иерархических наборов записей](../../../ado/guide/data/persisting-filtered-and-hierarchical-recordsets.md).  
  
 Самый простой способ сохранить данные в XML и загрузить их обратно через ADO — с помощью методов **Save** и **Open** соответственно. В следующем примере кода ADO показано сохранение данных из таблицы **titles** в файл с именем Titles. sav.  
  
```  
Dim rs as new Recordset  
Dim rs2 as new Recordset  
Dim c as new Connection  
Dim s as new Stream  
  
' Query the Titles table.  
c.Open "provider=sqloledb;data source=MySQLServer;initial catalog=pubs;Integrated Security='SSPI'"  
rs.cursorlocation = adUseClient  
rs.Open "select * from titles", c, adOpenStatic  
  
' Save to the file in the XML format. Note that if you don't specify   
' adPersistXML, a binary format (ADTG) will be used by default.  
rs.Save "titles.sav", adPersistXML  
  
' Save the recordset into the ADO Stream object.  
rs.save s, adPersistXML  
rs.Close  
c.Close  
  
set rs = nothing  
  
' Reopen the file.  
rs.Open "titles.sav",,,,adCmdFile  
' Open the Stream back into a Recordset.  
rs2.open s  
```  
  
 ADO всегда сохраняет весь объект **набора записей** . Если нужно сохранить подмножество строк объекта **Recordset** , используйте метод **Filter** , чтобы сократить количество строк или изменить предложение Selection. Однако необходимо открыть объект **набора записей** с клиентским курсором (**CursorLocation**  =  **адусеклиент**), чтобы использовать метод **фильтрации** для сохранения подмножества строк. Например, чтобы получить заголовки, начинающиеся с буквы «b», можно применить фильтр к открытому объекту **Recordset** :  
  
```  
rs.Filter "title_id like 'B*'"  
rs.Save "btitles.sav", adPersistXML  
```  
  
 ADO всегда использует набор строк обработчика курсоров клиента, чтобы создать объект **набора записей** с возможностью прокрутки, который можно закладкать поверх данных, созданных поставщиком сохраняемости.  
  
 Этот раздел содержит следующие подразделы.  
  
-   [Формат сохраняемости XML](../../../ado/guide/data/xml-persistence-format.md)  
  
-   [Пространства имен](../../../ado/guide/data/namespaces.md)  
  
-   [Раздел схемы](../../../ado/guide/data/schema-section.md)  
  
-   [Раздел данных](../../../ado/guide/data/data-section.md)  
  
-   [Иерархические наборы записей в XML](../../../ado/guide/data/hierarchical-recordsets-in-xml.md)  
  
-   [Динамические свойства набора записей в XML](../../../ado/guide/data/recordset-dynamic-properties-in-xml.md)  
  
-   [Преобразования XSLT](../../../ado/guide/data/xslt-transformations.md)  
  
-   [Сохранение в объект DOM XML](../../../ado/guide/data/saving-to-the-xml-dom-object.md)  
  
-   [Вопросы безопасности при работе с XML](../../../ado/guide/data/xml-security-considerations.md)  
  
-   [Сценарий сохраняемости набора записей XML](../../../ado/guide/data/xml-recordset-persistence-scenario.md)
